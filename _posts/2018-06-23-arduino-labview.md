---
layout: default
title: Program Arduino with LabVIEW
published: true
---
_Hey pals! Today i’m gonna show you how to control led’s using Arduino Mega and LabVIEW. This is the most simplest control one can apply. The package LabVIEW Interface for Arduino (LIFA) has been replaced with LINX. It is highly recommended to migrate to LINX as there will be no further development for LIFA._
{: .text-justify}


![rpi.jpg](https://lh5.googleusercontent.com/rMrwiIUCbjJDHjX-SuourIGFDhv0_ELL3-zuwPNcch_FQ1raL02kk6t1JC4OG_bco_4=w2400)
<!--more-->


Install the LabVIEW Interface for Arduino toolkit

To install the toolkit, complete the following steps

- Make sure you have the most recent version of VI Package Manager (VIPM) installed on your system. You can download VIPM Community Edition for free at [VI Package Manager](https://www.vipm.io/).

- Launch VIPM


![VIPM](https://lh3.googleusercontent.com/xm5IQE8QzrlL2vqQAUnmZXbRK16rCgzgSGclK4lEozd1qqo2EgTK6pKU-J4Gg1IrfTY=w2400)


- Browse to LabVIEW Interface for Arduino in the list of packages.


![LI_VIPM](https://lh5.googleusercontent.com/vQuuxm2VOQH_mqdjTJd-Gaxt6O6gZQ2M5rrvABGoncpidgamMK0sIdyc9aBpM1i-ejA=w2400)


- Click the Install & Upgrade Packages button


![Install_VIPM](https://lh3.googleusercontent.com/ZWm6wSTdIflRE2_4lFCdIv1mh_D-PQA1wOB7m33OOXUohSL9hbtq3xC0_2G9X0L5uMo=w2400)


- Click **Continue** and then **Finish**

- The LabVIEW Interface for Arduino is now installed on your system. Once the toolkit is installed you can use VIPM to check for updates for it.

Open the Arduino IDE click 
_File»Open and browse to LIFABase.ino found in C:\Program Files\National Instruments\LabVIEW 2017\vi.lib\LabVIEW Interface for Arduino\Firmware\LIFABase\LIFABase.ino_


![ino.png](https://lh5.googleusercontent.com/7hGPMKuMzqjFj_97_fjogWabfOB58EHUeLE0PSvdQpiDckOTNZR_cr_MEk1EkfR0enQ=w2400)


clicking _Tools»Board » Arduino Mega 2560_

Choose the COM port by clicking _Tools_ » _Serial Port_ and choosing the COM port that corresponds to your Arduino Mega

Click the Upload button to upload the firmware to the Arduino Mega

You are now ready to use the Arduino Mega with the LabVIEW Interface for Arduino. The LIFA toolkit contains a number of examples which can be accessed through the functions palette.
{: .text-justify}

Then open LabVIEW and hit Create Project and choose Blank VI

Following is the Block Diagram to control four led’s connected to digital 4, 5, 6 and 7 pin of Arduino Mega.
{: .text-justify}

To check the current I/O port used in the Block Diagram click **Tools** then **Measurement and Automation Explorer (MAX)** and then move to **Devices and Interfaces**.
{: .text-justify}


![links.png](https://lh3.googleusercontent.com/CzVg89QTWkYL1MYTr2YniCktnb5H41OtdCix2vECSkLiFttdHvF4qYGVugdMHlhUqdQ=w2400)


Here a GIF file is attached to show the same in real time.


![model.gif](https://lh3.googleusercontent.com/6ajv_9Jw2YkjXjKJvsSGfUqRv21tG9VfkYI-laEFmNT9nmspL28B2eWZ8gSwauEAFW0=w2400)
