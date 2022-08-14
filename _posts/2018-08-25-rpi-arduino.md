---
layout: post
title: >-
  Display measured data over local network following server/client relationship
  using Arduino and Raspberry Pi
published: true
---
Hello! Today i’ll show you how to measure DC voltage using Arduino and display the measured data on request from client over same network using some Python code and Raspberry Pi.
{: .text-justify}

Voltage measurement is the simplest task that we can perform using Arduino. Arduino’s internal ADC reference voltage is 5V (Vref=5V) so maximum voltage that we can measure without using external circuit is 5V. It is having 10-bit resolution, 210=1024 values for 0 to 5V scale. 0V corresponds to 0 ADC reading and 5V corresponds to 1023. Single ADC value represents 4.88mV. So the maximum voltage that we measure here is 5V.
{: .text-justify}


![rpi1](https://lh3.googleusercontent.com/84C_lcmbQ3A5HFLaJGs3u-e1_Ian-9wAuyGID3AHXU0qOvC0dkGZRgr-W5s6NQiq-3o=w2400)
<!--more-->