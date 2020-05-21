IOCOM scope
===========

Intended audience and use
*************************
* Small scale automation 
* For developers skilled in C/C++, microcontrollers and embedded platforms. 
* A module for developer to add secure communication to IoT devices, IO boards, etc. 
* Cloud enabled distributed applications which also work stand alone 


What IOCOM is?
**************
* Communication library written in C
* MIT license 
* Primary transports TCP/Ethernet and serial communication
* TLS and device/server authentication
* Link between IoT/IO devices and application running on controller or server
* Optimized for transferring "real time" IO signals for automation. 
* Plain C. No strong dependency on any specific hardware, platform or operating system.
* Hardware/platform dependent code is isolated behind abstraction layer.


What IOCOM is not?
******************
* Application or user interface. 
* General purpose communication protocol.
* Structured object oriented model suitable for passing complex data
* Not optimized for high latency or intermitten connections. 
* High abstraction level communication. Scope of the library is limited, and will not be expanded beyound needs to automation.
* Hardware/platform abstraction doesn't cover everything, implementing plaform interface layer may be needed to run on sepcific hardware.


Notes
*****
* EOSAL library is the operating system/platform/hardware abstraction layer. It presents the device and environment to application code
  trough uniform interface, and allows us to run same IO device application on different micro-controllers, embedded systems or on Linux/Windows.
* PINS library is an optional add on, which can be used to abstract IO PIN addressess and underlying IO. This approach may be
  useful if there is need to write portable IO board code to support future HW versions of the board.
* Python API: Python API is useful for quick conceptual testing and implementing features which are not performance or resource
  use critical (like IO device flash programmer/configurator over network or blue toots, IO device testers, etc). 
* The server and IO device use the same IOCOM library to communicate. These may be very different environments: IO microcontroller could
  have 100kB or RAM run on plain metal. Server side could have full fledged linux. The library is scaled to target platform by set of
  compiler defines, which enable or disable use of features like multi threading, dynamic memory allocation, device authentication, etc.
* Real time multiplayer games: Communication between player and game service ovr internet is very similar to one needed for connecting
  IoT/IO devices. There are many game oriented GUI libraries for Python, like Panda3D, Pyglet, Kivy. I have written simple games this way 
  to test communcation, just for fun.


200101, updated 20.5.2020/pekka
