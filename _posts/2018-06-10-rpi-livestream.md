---
layout: post
title: Live streaming using Raspberry Pi Camera module
published: true
---
_Here I have used Raspberry pi in headless mode using VNC. Prior to proceeding with the steps let us first make sure that the latest updates are installed on our Pi. To update our Pi we need to run the two commands below._
{: .text-justify}

![rpi.jpg](https://lh4.googleusercontent.com/S75UVSuC51CxpK7z5jWCTiJXbSZpn8x2IE38sXVxEV3Qsu4a6pHIlRr4tfI7t06p230=w2400)
<!--more-->

```shell
sudo apt-get update
sudo apt-get upgrade
```

### Initial set up of RPi Camera module

The first thing we need to do is to set up the camera and make sure it’s working. Connect the camera to Pi as shown below with the blue marking on the camera tape connector facing the Ethernet port.
{: .text-justify}

![rpi_setup.png](https://lh4.googleusercontent.com/uubayR44k047HYb1ZsjmKA_Xx-dduz8G8L1us0FqaEKX1AEx8kspouJ2TVg3AHx2-S8=w2400)

After the camera is physically connected we have to enable it. This is done using the native Raspberry Pi configuration tool – raspi-config. Run the tool using the command below.
{: .text-justify}

```shell
sudo raspi-config
```

A menu with different configuration options will appear. Navigate to Interfacing Options and press Enter.
{: .text-justify}

![rpi_config.png](https://lh4.googleusercontent.com/V08xzNbbJhIxB47Rb-h6Ju4PiGIuZOEN6dnlasanDhNyvyXazOquHcuzVAd2nUAiMeQ=w2400)

Then select Camera and press Enter.

![rpi_config2.png](https://lh4.googleusercontent.com/o0Q2unLugQy2OJvV8P30hUy31wo5XKniYWSUc4mCtaRWIKnvt-ZtFakSuwlXmtP0mek=w2400)

Then enable Camera by clicking Yes option

![rpi_config3.png](https://lh3.googleusercontent.com/6m21sz-Lo3gTgpw3GyZfDTuExCOW80EV3HvNOeP59VGSOouc6vUEkGdt4cPfNU85hvE=w2400)

From the main menu exit raspi-config by pressing the right arrow twice to navigate to the “<Finish>” option and press enter to exit. You will be prompted to reboot your Raspberry Pi if you are enabling it for first time for changes to take effect – select Yes.
{: .text-justify}

### Test the camera

To test that the camera is functioning correctly we’ll take one image and save it to /home/pi using the command below. Later you can view the image by navigating to the above mentioned location.
{: .text-justify}
  
```shell
raspistill -o /home/pi/image.jpg
```


