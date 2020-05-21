Signals
=======

What is signal ?
****************
Imagine a IO board with temperature sensor. 

* If we wish to pass temperature though communication as celsius degrees, with 0.01 degree preciosion, we might use
  a 4 byte floating point number for these. We need to map these 4 bytes to our memory block.
* The IO board or may not be connected, or it might be broken. We use a status byte to indicate if temperature signal is connected state if it errornous.
  One byte is used for this. 
* We want to name out signal and in code refer to it with name instead of pin address. This allows us to move sensor to other pin, to newer 
  version of IO board. We call this signal simply "my_temperature"
* Bith IO board and the control computer need to know in which memory block address this signal is and how it is stored.

Signal setup in JSON
********************
Signals are configured in JSON configuration file. We write in the JSON that we have signal "my_temperature", which is "float". 
We can also set address within memory block, but usually we leave this out and let it be calculated from JSON configuration.
See "JSON configuration" section for details.

The JSON file is converted by scripts to C code which allows us to use signals from code by signal name. It is also compressed as
binary JSON (further converted to C code and linked to IO board code): This makes content of "info" memory block so anyone who
connects to IO board can find out that there is signal "my_temperature" at address X. Five bytes are reserved for the signal,
first one byte for status bits and then four bytes for float value.

Get or set signal in C code
***************************

IO device networks
******************

If we have one confrol computer with bund of IO boards, we do not need concept of IO device network. 
It comes to play when we have server which needs to handle multiple systems. Cloud server is the most obvous example.

Two people, Pekka and Markku both have their own home automation network. Within house both these these have local server
so that the houses are operational without internet connection. Cloud server is used to give remote address to both
Pekka and Markku. Same cloud server is used for both Pekka and Markku. But at cloud server the two networks must
be kept securely apart even they are connected trough same cloud server. This is solved so that Pekka's and Markku's
devices are in separate IO device networks "pekkanet" and "markkunet", each with their own user account information.


﻿IO path to signal
*****************
How do I turn on the lights on in my garage from an application running in cloud server?

* I call my application “homerapp” and my home network “pekkanet”.
* There may be many instances of “homerapp” application running in same process in the cloud server, one for 
  every connennected home network. One of those instances runs “pekkanet”.
* Now my “garagelight” output signal would be connected to IO device named “garagectrl”. Since “pekkanet” can
  (at least in theory) have many garages, so this particular device is number 1, thus “garagectrl1”. 
* Now the “homerapp” could call function to set “garagelight.imp.garagectrl1.pekkanet” to one. Here the “imp”
  identifies the memory block used to transfer “garagelight” signal to IO device. 
* First the “.pekkanet” is not needed, since the instance of ”homerapp” running “pekkanet” can basically only
  access IO on “pekkanet” (not in markkunet). 
* Then explicit memory block name “imp” or “exp” is not needed, since signal name is unique within the device,
  the library can figure that out.
* The device name “garagectrl1” is needed only if we have multiple garages or signal names for other reason
  are not unique within the IO network. Here I assume that I have only one, so I can drop it.
* Consequently the command to turn on “garagelight” can be set “garagelight.*.*.*” to 1. Now there is no need 
  to write the wildcards, these are assumed. So final command would be set “garagelight” to one. 
* If signal names are not unique within the network, we need also the device name, and set command would be set “garagelight.*.garagectrl1” to one.

So full IO path is written as 
* to signal: signal_name.mblk_name.device_name.network_name
* to memory block: mblk_name.device_name.network_name
* to IO device: device_name.network_name

In software we know are we expecting path to signal, memory block, io device or just network name.

200521, updated 22.5.2020/pekka

