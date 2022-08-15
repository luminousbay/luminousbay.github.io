---
layout: post
title: ESP8266-01 as Server and Client using PuTTY and Realterm
published: true
---
_Hello guys.. in my previous posts I have mentioned some of the AT commands to configure your ESP and establish a communication between server and client locally. In this post I will mention how to do the same using PuTTY and Realterm._
{: .text-justify}


![nodemcu_1.png](https://lh4.googleusercontent.com/mtl9KqBkxLfKY9vIwqUpacBza77i0lQ6lS2QIXIDpFDWPPGmVFxg748rFXOYVA0Vxcw=w2400)
<!--more-->
Configure your ESP8266-01 and connect to an AP and get the IP address using the command:

```html
AT+CIFSR
```

1. Open PuTTY

2. Select “_Telnet_” as the connection type

3. Type the IP address and the port number as 80

4. Click on “_Open_”


![putty.png](https://lh4.googleusercontent.com/HEMhubcG37oxzOfjWRJpW0kq-iKHCrtoqbd5uei23ywbipFlrLGPGieHtYQkcSa8RJw=w2400)


A new terminal will open.


![W.jpg](https://lh3.googleusercontent.com/4DgcEDWSzfh571HYqrrQn43Zb1cJt1FvieaswX_db_TuV12FhFQUac8gUmdAXgL2GiU=w2400)


Now to configure Realterm

Open Realterm and migrate to  _Display_ and select _baud rate_ and _PORT_


![realterm.png](https://lh3.googleusercontent.com/55JhDCt3mV_Kh2pR2fbx3GboIQoym5iV-VGiZbyJoyUR7HEVoFqPCjkweOHsUkeaVKE=w2400)


click _Open_ and move to _Send_ and do the setting as shown below


![rt.png](https://lh4.googleusercontent.com/K4KAqzIwWCWJIF5GnVJ_WZdPnx7aVdPJyNxjtQ3TmTZzDNizET6chehNgnlNIVsiO_0=w2400)


Send AT Command to check the response

Further to send command click _Send ASCII_  Button

Put forth all these commands to reconfigure or to reassure

```html
AT+CIPMUX=1
AT+CIPSERVER=1,80
```

Then migrate to the PuTTY terminal and type your desired message to print into Realterm screen.

This time you are sending message as a client to the local sever.


![q.png](https://lh4.googleusercontent.com/CQ1TrVqro5bosx-EmvextSuzQWK9esSTjtKYt7ygfDm1UI4E8zkeT68NmWHvKpmjGrQ=w2400)


Now to send text message from Realterm to PuTTY follow these steps:

Open PuTTY and set connection type as _Raw_


![p.png](https://lh5.googleusercontent.com/djbByFfw88T8utgbB2Zbmq7M5K7MzJ9gQD8nE-jHTd7G0gUe4ZzUjzULLfQcG1yKKpk=w2400)


Always after opening Realterm configure your ESP8266 there is no harm in doing this

You may get this message in return that ‘_Link is built_‘

Now type this command on Realterm terminal

```html
AT+CIPSEND=0,5
```

And after this, type your desired message but it should be under the prescribed bytes which you've already mentioned in the above command.

And immediately you will notice your message on PuTTY terminal and this time don’t use:

```html
AT+CIPCLOSE=0
```

which we have done when we are viewing messages on browser else this will close the PuTTY terminal.





































