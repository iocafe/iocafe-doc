Set up and test development tools
=================================

PC development environment
**************************
I list here the tools which we have used. Code is plain C code, and it is possible to use other tools as well. 

* Linux: cmake build system and QT creator as IDE.
* Windows: Visal Studio 2019 community edition. Large business with > $1 million revenue need to use the commerical version. 
* Most of scripting and some server side programming is Python 3.
* Kivy and Pyglet Python libraries are used in some examples (kivy in ispy).
* Sphinx is used for documentation.
* I recommend Git Kraken, it is free for non commerical use.

Micro-controller development tools
**********************************
* ESP32 is the first target micro-controller for IOCOM release 1.0.
* ESP32: PlatformIO + Visual studio code.
* STM32: We have run tests to support STMicroelectonics TrueStudio + Cube, but this is not useful for now.
* Arduino: Arduino IDE has bee tested for builds, but support for it may not be up to date.

Download virtual machine?
*************************
If one is unfamiliar with development tools, I suggest downloading a ready linux virtual machine from iocafe.org. 
It has operating system, iocom code and development tools preloaded. It may be useful also as reference to set up
your own development environment.

* First virtual machine supports PlatformIO/Visual Studio Code for ESP32 development and QT creator as IDE for 
  Linux development. Distribution is Ubuntu 20.04 DDE desktop remix. 

Build and test micro-controller example code
*******************************************
To make sure that build, uploading, debugging and hardware really work, build and test example projects which
come with the development environment. Build and test expecially some socket client. Development environments
come usually with very good documentation and examples.

200329, updated 19.5.2020/pekka

