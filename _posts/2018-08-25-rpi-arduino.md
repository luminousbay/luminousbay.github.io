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
