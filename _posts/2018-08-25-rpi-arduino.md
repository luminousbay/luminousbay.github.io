---
layout: post
title: >-
  Visualise measured data over local network following server/client
  relationship between Arduino and Raspberry Pi
published: true
---
Hello! Today i’ll show you how to measure DC voltage using Arduino and display the measured data on request from client over same network using some Python code and Raspberry Pi.
{: .text-justify}

Voltage measurement is the simplest task that we can perform using Arduino. Arduino’s internal ADC reference voltage is 5V (Vref=5V) so maximum voltage that we can measure without using external circuit is 5V. It is having 10-bit resolution, 210=1024 values for 0 to 5V scale. 0V corresponds to 0 ADC reading and 5V corresponds to 1023. Single ADC value represents 4.88mV. So the maximum voltage that we measure here is 5V.
{: .text-justify}


![rpi1](https://lh3.googleusercontent.com/84C_lcmbQ3A5HFLaJGs3u-e1_Ian-9wAuyGID3AHXU0qOvC0dkGZRgr-W5s6NQiq-3o=w2400)
<!--more-->

 
![Analog](https://lh6.googleusercontent.com/JjldPoC6rYozWSxKp99KufbQb_1gDZU5Hj0MIiv0r5MfUKUcnd-5q4HL3KpF3DF30gE=w2400)
{: .image-center}

A 10K ohm Potentiometer is used to regulate the output voltage. Connect the three wires from the potentiometer to your board. The first goes to ground from one of the outer pins of the potentiometer. The second goes to 5 volts from the other outer pin of the potentiometer. The third goes from the middle pin of the potentiometer to analog input 0. By turning the shaft of the potentiometer, you change the amount of resistance on either side of the wiper which is connected to the center pin of the potentiometer. This changes the voltage at the center pin. This voltage is the analog voltage that you’re reading as an input.
{: .text-justify}

```cpp
void setup()
{
Serial.begin(9600);
}
void loop()
{
int sense = analogRead(A0);
float voltage = sense * (5.0/1023.0);
Serial.println(voltage);
delay(500);
}
```

Open the Serial Monitor from (Tools>>Serial Monitor), Set Baud rate to 9600, See the voltage in serial monitor.


![volt_measure](https://lh3.googleusercontent.com/07SqOj_KCauTVCJRxQNDu32rKJ-UW4RnU0gVDoALjFbmmZNYhKGMxqCaFHwEBImiT-o=w2400)


But here in this tutorial we won’t convert analog values to corresponding voltages right now instead we do it when we read these values from the saved file in Raspberry pi and hence we must edit the above lines of code as follows:
{: .text-justify}

```cpp
void setup() 
{
Serial.begin(9600);
}
void loop() 
{
int sense = analogRead(A0);
Serial.println(sense);
delay(500);
}
```

The output at Serial Monitor is shown below:


![volt_measure](https://lh3.googleusercontent.com/hBI9LQ_hOFW01vYvH6_qfZnPFt_7m_OssGyWQFtwwVs4zqKQCN0afZXmS1NZgzTkGKM=w2400)


Now we connect Arduino to Raspberry Pi board through USB port in order to save the serial data within Raspberry Pi in truncated form. Here the serially incoming data is saved in truncated form in order to get access to the most recent measured value. It is saved in CSV format such that whenever newer reading arrives the older saved one gets replaced automatically. On the other hand RPi is programmed such that on client’s GET command it reads the data from the file and displays it on client’s terminal.
{: .text-justify}

Plug Arduino to one of the USB port of Raspberry Pi and then we need to find device ID for the connected Arduino in order to use it. To do so we use the command:
{: .text-justify}

```shell
sudo ls /dev/
```

It generally means to list all the device connected to Raspberry Pi. Hence it will show a similar result as shown below:


![device_list](https://lh5.googleusercontent.com/d1g96WKz5bbpyCSVPavpduBEFldzbl1y7XPyQzH3MrX8ydizGwxd9yDCJlwrVHhXC-8=w2400)


From this it becomes very difficult to identify the exact device ID for Arduino. So the trick is that we’ll save the list in a text file when Arduino is not connected and again we’ll save the list in another text file but this time when Arduino is connected and then we figure out which extra device has been added up.
{: .text-justify}

To save the device list when Arduino is not connected:

```shell
sudo ls /dev/ >>device.txt
```

To save the device list when Arduino is connected:

```shell
sudo ls /dev/ >>arduino.txt
```

Now we have the list of previously and currently connected devices. To identify the currently connected device we’ll take difference of both the file
{: .text-justify}

```shell
diff device.txt arduino.txt
```

This will list all the extra connected device and will surely contain the device ID of Arduino. Once we get this device ID we can easily communicate with it. This device ID is used to read serial data from Arduino.
{: .text-justify}


![device_list1](https://lh6.googleusercontent.com/cMSH2k8OMkO7UYCSUwq-YOf2dXdjjdTbQtW0ivHfWx3iPqAFPg3X9JelL1S0q0pJAOk=w2400)


A small python code is written for the same

```python
import serial
import csv
import time
ser = serial.Serial('/dev/ttyACM0')
ser.flushInput()
while True:
     try:
          ser_bytes = ser.readline()
          decoded_bytes = float(ser_bytes[0:len(ser_bytes)-2].decode("utf-8"))
          voltage = decoded_bytes * (5.0/1023.0)
          volt = round(voltage,2) #Rounding voltage to 2 digit
          csv_file = open("/home/pi/Desktop/data.csv","a")
          with open("/home/pi/Desktop/data.csv","a") as f:
               csv_file.seek(0) #To return the cursor back to starting point
               csv_file.truncate()
               local = time.asctime(time.localtime(time.time()))
               writer = csv.writer(f,delimiter=",")
               writer.writerow([local,volt])
     except:
          print("Keyboard Interrupt")
          break
```

In the above excerpt of code, analog value is converted into corresponding DC voltages and is rounded up-to 2 figure. The CSV file is saved in the location _/home/pi/Desktop_  and is opened with “a” so as to append data in that file. Time is also appended here as further customization in a temporary variable ‘_local_’. Data is saved in truncated format. This must be run from the terminal only as we require Thonny editor for running another piece of code. Now to create local server on Raspberry pi open python editor (Thonny) and run the following code.
{: .text-justify}

```python
import socket
import os
host = ''
port = 5560
def catacomb(): #To read the truncated data from arduino
     f = open('/home/pi/Desktop/data.csv', 'r')
     storedValue = f.read(1024)
     return storedValue
def measure_temp():
     temp = os.popen("vcgencmd measure_temp").readline()
     return temp
def setupServer():
     s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
     print("Socket created.")
     try:
          s.bind((host, port))
     except socket.error as msg:
          print(msg)
     print("Socket bind complete.")
     return s
def setupConnection():
     s.listen(1) #Allows one connection at a time
     conn, address = s.accept()
     print("Connected to: " + address[0] + ":" + str(address[1]))
     return conn
def dataTransfer(conn):
     while True:
          data = conn.recv(1024) #Receive the data
          data = data.decode('utf-8')
          dataMessage = data.split(' ', 1)
          command = dataMessage[0]
          if command == 'DATA': #This command will send the data from arduino
               reply = catacomb()
          elif command == 'TEMP': #To send the processor temperature
               reply = measure_temp()
          elif command == 'EXIT': #Only client will be exited
               print("Client has left us.")
               break
          elif command == 'OFF': #Server and client will both get turn off
               print("Server is shutting down.")
               s.close() #Close the connection
               break
          else:
               reply = 'Unknown Command'
          conn.sendall(str.encode(reply)) #Send the reply back to client
          print("Data has been sent!")
     conn.close()
s = setupServer()
while True:
     try:
          conn = setupConnection()
          dataTransfer(conn)
     except:
          break
```


Now after successfully running the above python code you’ll get the following result on terminal.


![output](https://lh6.googleusercontent.com/vuAelqSkYoUv5f7RW4CU3U2739bzQ5qFKajGSpcTkyqnHf13RsS5oX-QbBEne3_1vIU=w2400)


Here i have used Thonny editor that comes inbuilt in Raspbian Stretch. Here it is to be noted that all the above codes must be run inside Raspberry Pi. Next step is to work on client side. To do so open any python editor (pyCharm) in your PC and make sure that your PC is connected to the same network as your Raspberry Pi, simply it may be your mobile hotspot. Run the following code.
{: .text-justify}


```python
import socket
import csv
 
host = '192.168.xxx.xxx' #Use your Raspberry Pi's IP Address
port = 5560
 
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect((host, port)) #Connect to the Server
 
while True:
    command = input("Enter your command:")
    if command == 'EXIT':
        s.send(str.encode(command))
        break
    elif command == 'OFF':
        s.send(str.encode(command))
        break
    s.send(str.encode(command))
    reply = s.recv(1024) #Receive from Server
 
    print(reply.decode('utf-8'))
    csv_file = open("E:\ voltage.txt", "a")
    with open("E:\ voltage.txt", "a") as f:
        writer = csv.writer(f, delimiter=" ")
        writer.writerow(reply.decode('utf-8'))
s.close()
```


The above lines of code is used to ask the server for the voltage that Arduino is reading on the other side. To read the voltage along with time the command ‘DATA’ is used and  command ‘TEMP’ is used to get your Pi’s temperature. To exit the client but keeping the server running ‘EXIT’ command is used and to turn off both the side ‘OFF’ command is used.
{: .text-justify}


![output1](https://lh4.googleusercontent.com/WUMvY8HRnNQavNEvtdAKwRjNxT82SWZnB4FC6Y3onkBkcjn8D25QXnROvHyi4FoiT64=w2400)


One more function which is added here is that the data which is received by client side will get automatically saved in a file of text format located in E drive as voltage.txt. The extension of the file can be changed to CSV format also. You basically don’t need to manually create the file but it’ll auto create itself. To show the same a screenshot is added below.
{: .text-justify}


![output2](https://lh5.googleusercontent.com/ZcIOq9K9HEp70Di_pcDOBrBA3si2-ux3L68dWCqrZcOpACtBsQxHy-ElXxXZlNxIJFk=w2400)





