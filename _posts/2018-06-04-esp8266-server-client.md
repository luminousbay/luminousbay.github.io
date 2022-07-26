---
layout: post
title: ESP8266-01 as Server and Client
published: true
---

_The ESP8266-01 is a Wi-Fi module that allows any microcontroller system access to a Wi-Fi network. This module is a self-contained SOC (System On a Chip) that doesn’t necessarily need a microcontroller to manipulate inputs and outputs as you would normally do with an Arduino. ESP8266-01 basically having two GPIO pins. We can simply program the ESP8266 to not only have access to a Wi-Fi network, but to act as a microcontroller as well. This makes the ESP8266 very versatile. In this tutorial I am going to communicate with ESP8266-01 module._
{: .text-justify}


![esp.jpg](https://lh4.googleusercontent.com/zVT5Jr5yWMWeaseowqlNNKKyJDu3tTSJuRXkr1Zq5t5HitT1rG9lQOVc2C_AV-w9neE=w2400)
<!--more-->
## _ESP-01 Setup_

When you buy the ESP8266 ESP-01, it comes with a pre-installed AT firmware. It is possible to program the chip with another firmware such as LUA firmware, for example. However, AT firmware is compatible with the Arduino IDE.

### _ESP8266 ESP-01 Pinout_


| ![a.jpg](https://lh5.googleusercontent.com/dPFyM8T7KNtYmkOTk5CW2CC1gkWDDYU0FfmeLyJnFfvzd02JOfo2kSDMsImxqk-MV6A=w2400)| 
|:--:| 
| *pinout of esp8266* |


### _ESP8266 Wiring Setup_


| ![b.jpg](https://lh5.googleusercontent.com/-5nusHEhbXfhYLdzaZnuWqJ8ZkP-CKLcp_xpvVr5aqkrpIKEfFkYXibhb6hbwDEogS8=w2400)| 
|:--:| 
| *connection without bypassing Arduino* |


Upload the _BareMinimum_ example to ensure that no previous programs are running and using the serial communication channel.

One more thing in my previous post I have mentioned to bypass arduino in order to establish communication with esp module.

Just make low the RESET pin of arduino

- This will make you to communicate with esp at the baudrate of 115200

Else at 9600 it will display garbage value.

If you do not wish to bypass arduino board then remain RESET pin as it is

- This will make you to communicate with esp in both baudrate of 115200 and 9600.

Next, open the serial monitor and type the following command:

```html
AT
```

You should get an “OK” response. Do it several times to receive OK response. Now we are ready to test a two way communication between the module and another device.

### Basic AT commands  

The ESP8266 ESP-01 module has three operation modes:

1. _Station (STA)_

2. _Access Point (AP)_

3. _Both_

In **AP** the Wi-Fi module acts as a Wi-Fi network, or access point (hence the name), allowing other devices to connect to it. It simply establishes a two way communication between the ESP8266 and the device that is connected to ESP via Wi-Fi.

In **STA** mode, the ESP8266-01 can connect to an AP such as the Wi-Fi network from your smartphone or simply Hotspot.

The third mode of operation permits the module to act as both an **AP** and a **STA**.

Here we are going to set the module to operate in **STA** mode:

```html
AT+CWMODE=1
```

The corresponding number for each mode of operation is as follows:

- STA = 1

- AP = 2

- Both  = 3

If you want to check what mode your Wi-Fi module is in, you can simply type the following command:

```html
AT+CWMODE?
```

This will display a number (1, 2, or 3) associated with the corresponding mode of operation.

Now if you want to print the firmware version then just put this command:

```html
AT+GMR
```


![ESP1.jpg](https://lh5.googleusercontent.com/cQyr81C2KqcOzseFTTL0mZv0AyFnYUOyUQZqcrd823iVbqLJ0a6h499dRrhiZ0WUxd4=w2400)


Once we have the ESP-01 operating in STA mode, we need to connect to a Wi-Fi network. First we can check if we are already connected to one by sending the command:

```html
AT+CIFSR
```


![ESP2.jpg](https://lh3.googleusercontent.com/YWPYSyK5mmh8JaxWpP6Ur7HC3a-ecA13-k0agt9ssktUXU6oR-Yd_1PIJKOjiDDbUnE=w2400)


This will display the station IP address of our ESP-01 module. If you don’t get an IP address after entering the previous command, that means you were previously not connected. Now to search available access point around you type the command:

```html
AT+CWLAP
```

_CWLAP basically stands for list access point_


![ESP3.jpg](https://lh5.googleusercontent.com/-m6gfLkFeIbbPo-EaIpakR--hWVnYemLLJ0ic0u807dr3irYwFAH6WC9Uk_cZA2I8e0=w2400)


Now to join any Access Point printed on your screen use the following command to connect to the network:

```html
AT+CWJAP= “Wi–FiNetwork”,“Password”
```

_CWJAP Basically stands for Join Access Point_


Type the name of your Wi-Fi network and the password to connect to it. Make sure you include the quotation marks. After a couple of seconds, you should get an “OK” response. You can check again to see if you have an IP address using the AT+CIFSR command.

Then we need to enable multiple connections that is enable / disable multiplex mode (up to 4 connections) before we can configure the ESP8266 ESP-01 module as a server. Type the next command:

```html
AT+CIPMUX=1
```

Once again, each number is associated with a type of connection:

### PARAMETERS:
- mode:

    - 0: Single connection

    - 1: Multiple connections (MAX 4)

### NOTE:
This mode can only be changed after all connections are disconnected. If server is started, reboot is required.

The following step is to start the server at port 80:

```html
AT+CIPSERVER=1,80
```

The first number is used to indicate whether we want to close server mode (0), or open server mode (1). The second number indicates the port that the client uses to connect to a server. We chose port 80 because this is the default port for HTTP protocol.

### PARAMETERS:
- mode:

	- 0: Delete server (need to follow by restart)

	- 1: Create server

	- port: port number, default is 333

### NOTE:
**_Server can only be created when AT+CIPMUX=1_**

Now, when we open a web browser and type the IP address of our ESP module we get the following response:


![ESP5.jpg](https://lh4.googleusercontent.com/RJLO8leUSSP4bIB2nEPCimEl5Wib_B2esXSPpF5OQx4VD-TlbVdcJRndGiLYimbtyA4=w2400)


This is the HTTP request that our computer (browser) sends to the server to fetch a file. It contains some interesting information such as what file you want to retrieve, name of the browser and version, what operating system you are using, what language you prefer to receive the file in, and more. Now when ESP8266 will get this fetching request from the client it can now send data.

We can now use the following commands to send some data and display it in our web browser’s window:

```html
AT+CIPSEND=0,5
```

Here the “0” indicates the channel through which the data is going to be transferred,  while “5” represents the number of characters that are going to be sent. MAX IS 2048 Bytes. When we hit enter, the symbol “>” appears. This indicates that we can now type the characters that we want to send to the browser. In this example we chose “hello.” After a couple of seconds we get the response “SEND OK.” This means that the data has been transmitted successfully to the client.

However, nothing appears on the web browser’s window yet. This is because it is required to close the channel first in order to display the characters. We use the following command to close the channel:

```html
AT+CIPCLOSE=0
```


![ESP6.jpg](https://lh6.googleusercontent.com/_zKSCI5MhU8JAW9UrsC_OrM3MyQasGY0FGPoygj--jo3nbyLWhocXhykEUQ9I4UdDd8=w2400)


“0” indicates the channel that is being closed. Once we hit enter, our message is displayed on the web browser’s window:

You can refer to the following site to see the ESP8266 ‘AT’ Command Set:

[here](https://room-15.github.io/blog/2015/03/26/esp8266-at-command-reference/)

Originally taken from [jayconsystems](www.jayconsystems.com)







