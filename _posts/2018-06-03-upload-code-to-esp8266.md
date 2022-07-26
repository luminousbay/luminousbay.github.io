---
layout: post
title: How to upload code to ESP8266-01 through Arduino board
published: true
---

_It's a very simple task to accomplish this and is very convenient also. So here I'm writing how to simply upload your well written code to ESP8266-01 module using any Arduino board._
{: .text-justify}

![_config.yml]({{ site.baseurl }}/images/arduino.jpg)
<!--more-->
###  Step 1: Installing Board to Arduino IDE

First, install ESP8266 to Arduino IDE. If you have already installed the board to boards manager of Arduino IDE, skip this step else follow the steps

- Start the Arduino IDE

- Go to File > Preferences

- Copy and paste the below given link to Additional Boards Manager URLs
[http://arduino.esp8266.com/stable/package_esp8266com_index.json](http://arduino.esp8266.com/stable/package_esp8266com_index.json)



| ![arduino_ide_preferences_kHcfDP5CUm.jpg](https://lh5.googleusercontent.com/d4DYu7ygStwp3AsdgPPVsiwmLhtYTCdz0W_cG3Odog4wKG1-QZvjVK6K5J90oWWzG-c=w2400)| 
|:--:| 
| *Arduino IDE Preferences* |



- Go to Tools > Boards > Boards Manager...

- Search ESP8266



| ![arduino_ide_preferences_kHcfDP5CUm.jpg](https://lh4.googleusercontent.com/yEY37a2aHzPQnoxLOyBwcnyspZHmp_7GDP8F0XPHzAc4JNYWfCnp0wDFZ8r3gC0jc0Y=w2400)| 
|:--:| 
| *ESP8266 in Arduino Boards Manager* |


- Click the Install button to install the ESP8266 Board

- Now close the Boards Manager window and select the Generic ESP8266 Module from the board selection list



| ![arduino_ide_preferences_kHcfDP5CUm.jpg](https://lh5.googleusercontent.com/ya3Ie9jhnQuwH3LPGLT8tY738ne_ciEBh_hBb-WcvGHbgqj68CT1Zz2H3VpsmjvvRWw=w2400)| 
|:--:| 
| *Selection of ESP8266 as board in Arduino IDE* |



- Installation of ESP8266 in Arduino IDE is done.


### Step 2: Circuit Connection

| **ESP8266** |  **Arduino Board**  |
|:-----------:|:-------------------:|
|      Tx     |          Tx         |
|      Rx     |          Rx         |
|     Vcc     | 3.3 volt of Arduino |
|     Gnd     |         Gnd         |
|    CH_PD    | 3.3 volt of Arduino |
|    GPIO_0   |         Gnd         |
{: .tablelines}

The most important thing is to connect the RESET pin of Arduino to GND in order to bypass the Arduino board and to make it a simple intermediator. This will make you enter into the flashing mode of the ESP8266-01.


![_config.yml]({{ site.baseurl }}/images/aaa.jpg)


### Step 3: Flashing ESP8266-01 using required firmware

To do this you need to choose the firmware as per your requirement as you will come to know these supports different functions. I have uploaded both flasher and the latest firmware in my drive. The link is provided below.

[ai-thinker-v1.1.1](https://github.com/dil-se/acceleration)

One thing to be kept in mind is to choose the right COM PORT while flashing.And if you face some problem do restart the flasher again.After flashing till 100% you will see the message 'Failed to leave flash mode'...nothing to worry about.


![_config.yml]({{ site.baseurl }}/images/Untitled1.png)


Just close the flasher and simultaneously disconnect the Arduino board and GPIO_0 from GND.

### Step 4: Program ESP8266 using Arduino

Make the circuit as per the above directions. Power up the Arduino UNO board and wait till the Arduino Board boots up successfully. (It will take around 5 seconds) . Then open the Arduino ide and simply choose the correct COM PORT  and what then ... just upload the program.
