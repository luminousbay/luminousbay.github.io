---
layout: post
title: >-
  Display measured data over local network following server/client relationship
  using Arduino and Raspberry Pi {: .text-justify}
published: true
---
Hello! Today i’ll show you how to measure DC voltage using Arduino and display the measured data on request from client over same network using some Python code and Raspberry Pi.
{: .text-justify}

Voltage measurement is the simplest task that we can perform using Arduino. Arduino’s internal ADC reference voltage is 5V (Vref=5V) so maximum voltage that we can measure without using external circuit is 5V. It is having 10-bit resolution, 210=1024 values for 0 to 5V scale. 0V corresponds to 0 ADC reading and 5V corresponds to 1023. Single ADC value represents 4.88mV. So the maximum voltage that we measure here is 5V.
{: .text-justify}


![rpi1](https://lh3.googleusercontent.com/84C_lcmbQ3A5HFLaJGs3u-e1_Ian-9wAuyGID3AHXU0qOvC0dkGZRgr-W5s6NQiq-3o=w2400)
<!--more-->


![Analog](https://lh6.googleusercontent.com/JjldPoC6rYozWSxKp99KufbQb_1gDZU5Hj0MIiv0r5MfUKUcnd-5q4HL3KpF3DF30gE=w2400)


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