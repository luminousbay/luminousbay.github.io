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


From the main menu exit raspi-config by pressing the right arrow twice to navigate to the 'Finish' option and press enter to exit. You will be prompted to reboot your Raspberry Pi if you are enabling it for first time for changes to take effect – select Yes.
{: .text-justify}

### Test the camera

To test that the camera is functioning correctly we’ll take one image and save it to /home/pi using the command below. Later you can view the image by navigating to the above mentioned location.
{: .text-justify}
  
```shell
raspistill -o /home/pi/image.jpg
```

When the command is executed you will see a camera preview appear on your display. The preview will stay on for few seconds and then an image will be taken.
{: .text-justify}

To capture a 10 second video with your Raspberry Pi camera module, run at the prompt, where “video” is the name of your video and “10000” is the number of milliseconds.
{: .text-justify}

```shell
raspivid -o video.h264 -t 10000
```

where 10 seconds be the length of video.

### Raspberry Pi camera as USB video device

The Raspberry Pi camera can appear in /dev as a standard USB video device (required by MJPG) if we load the “Video for Linux 2” (V4L2) module for the corresponding hardware (BCM2835). We do this as follows:
{: .text-justify}

```shell
sudo modprobe bcm2835-v4l2
```

Please keep in mind that the above command will persist on your Raspberry Pi until the system shuts down. After the system is re-booted you will have to type this command again.
{: .text-justify}

Upon successful execution of this command you should see **Video0** device file inside /dev directory. To verify, run the command below.
{: .text-justify}

```shell
ls /dev | grep vid
```


![rpi_modprobe.png](https://lh6.googleusercontent.com/squIeGaQCc3Cq008RyQ9HLQfsbiYD2-9B-aFyFPvlETH006A6GBETttlS0BlNghm3K4=w2400)


### Install pre-requisites

This requires the presence of libjpeg-dev, imagemagick, libv4l-dev. Install them as follows:

```shell
sudo apt-get install libjpeg8-dev imagemagick libv4l-dev
```
 
### Building MJPG-streamer

The MJPG-streamer application doesn’t come in a package form that we can install using sudo apt-get install command. Instead, what we have to do is to download and compile it from source code.
{: .text-justify}

Source code usually comes in the form of a “tarball” i.e. multiple files packed together into an archive using the tar utility and then compressed using gzip utility to reduce the size of the archive.
{: .text-justify}

_Download MJPG-streamer tarball using the command below_

```shell
wget http://terzo.acmesystems.it/download/webcam/mjpg-streamer.tar.gz
```

Uncompress and unpack the tarball as follows

```shell
tar -xvzf mjpg-streamer.tar.gz
```

Make a symlink between two libraries that will be used to compile MJPG-streamer

```shell
sudo ln -s /usr/include/libv4l1-videodev.h /usr/include/linux/videodev.h
```

Navigate into MJPG-streamer directory and edit Makefile using an editor

```shell
cd mjpg-streamer/
nano Makefile
```

In this file, comment out the **input_gspcav1** plugin by inserting “#” at the beginning of the line, like so

```python
# PLUGINS += input_gspcav1.so
```

You can search the code by clicking _Ctrl+W_ and can make the changes

Finally, run make to compile the program

```shell
make
```

Compilation should only take couple of seconds and should produce no errors. When successful we’ll have an MJPG-streamer application executable built.
{: .text-justify}

 

### Run MJPG-streamer server

The command below will run the server

```shell
sudo ./mjpg_streamer -i "./input_uvc.so -f 10 -r 640x320 -n -y" -o "./output_http.so -w ./www -p 80"
```

Here you can adjust the frame per second and resolution by editing

```html
-f 10  and  -r 640x320
```

If you see the following output that means Server is running correctly


![rpi_server.png](https://lh6.googleusercontent.com/y_lrekC_0m9mN1wyOaIyAEC7NmjmF3778POqBJqtNQUdxgQDL3agcEjLpiUOwQCN89s=w2400)


Then sign in into Dataplicity and copy the line of code starting with curl into your Raspberry Pi terminal to activate your remote shell
{: .text-justify}

Then verify your account through the link they have mailed you

### Enable Wormhole

If you haven’t already done so, log in to your Dataplicity account and go to the device (e.g. the Pi) you want to link to. At the top of the page will be a link to ‘Activate Wormhole’.
{: .text-justify}

Press it and enable Wormhole. The link will contain your device ID

### Access your camera video stream via Dataplicity Wormhole

To see our video stream embedded using HTML we’ll go to the address below (remember to replace \<YOUR_ID> with your device Wormhole URL from your Dataplicity account).
{: .text-justify}

```html
https://<YOUR_ID>.dataplicity.io/stream_simple.html
```

_Now let us make an android app using MIT App Inventor_

Necessary block diagram for this is shown below


![rpi_app.png](https://lh6.googleusercontent.com/jSd8Q4ovoZUh0SQiJ1iq8zBwlvYZCC7Ch13lYLOWHHxNcHeNoRj08v6kMBhIKIjbzao=w2400)


Now build this app choose an icon and install in your android phone and stream your Raspberry pi Camera with one-click.

Originally taken from  [docs.dataplicity.com](docs.dataplicity.com)
