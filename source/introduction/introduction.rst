Introduction
================================

The IOCOM library implements communication between a control computer and IO devices over TCP network or 
full duplex serial communication wire. The library supports scalable range of functionality. 
Minimum configuration supports only serial communication on plain metal, while some applications may 
require multi-threading and secured communication over Internet.

Micro-controllers can be used as plain metal, use Arduino derived or device manufacturer's own libraries,
FreeRTOS, LWIP/W5500 for networking, and TLS library. The IOCOM is not bound tightly to any underlying system,
so it can be ported basically to any environment with sufficient resources for the task. Still, using
configuration which has been implemented and tested makes things much easier.

The same code as in micro-controller runs on Windows and Linux. Linux on should cover Raspberry PI 
and similar devices. We are considering MAC and Android testing as well. 

Security is becoming paramount in IoT and manufacturing environments. Thus TLS integration, strong 
encryption and device identification are essential part of the project. 

.. toctree::
   :maxdepth: 2
   :caption: Contents:

   general/general
   mechanics/mechanics

