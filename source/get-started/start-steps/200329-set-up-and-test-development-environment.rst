Set up and test development tools
=================================

PC development environment
**************************
I list here the tools which we have used. Code is plain C code, and it is possible to use other tools as well. 

* Linux: cmake build system and QT creator as IDE.
* Windows: Visual Studio 2019 community edition. Large business with > $1 million revenue need to use the commercial version. 
* Most of scripting and some server side programming is Python 3.
* Kivy and Pyglet Python libraries are used in some examples (kivy in ispy).
* Sphinx is used for documentation.
* I recommend Git Kraken, it is free for non commercial use.

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
********************************************
To make sure that build, uploading, debugging and hardware really work, build and test example projects which
come with the development environment. Build and test especially some socket client. Development environments
come usually with very good documentation and examples.

Notes about uploading the program
*********************************
During development, code is usually uploaded to micro-controller trough USB, UART (TTL level serial) or JTAG 
port. The development computer has USB, so unless the IO device has USB port capable of programming one needs
either USB/JTAG adapter, like ESP prog, or USB to TTL level serial converter. 

The USB serial ports like /dev/ttyUSB0 typically require extra configuration on Linux end: At minimum one must 
give permissions to normal user to access these. More often than not, one must copy rules file to /etc/udev/rules.d
directory to set up serial port. For example on this computer I have files 45-maple.rules, 49-stlinkv2.rules,
49-teensy.rules, and so on. The rules file comes with micro-controller development tools.

If one is using a virtual machine, the USB adapter needs to be connected to virtual machine, not to host.
This is done from vmware menu.

Then port, like /dev/ttyUSB0 needs to be selected in the development environment. For PlatformIO this is
in platformio.ini.

Connecting the JTAG or TTL serial adapter to micro-controller board takes some care: With JTAG, connect both
wires for JTAG and for serial port to monitor board output.

We will place IO device specific notes and links in "micro-controllers" or "development tools" sections 
of this document.

Notes about debugging
*********************
Development tools like PlatformIO/VisualStudio Code and STM True Studio integrate GNU debugger into
user interface. Once you get wiring and settings right, this is extremely useful. It is not foolproof
anyhow and includes several pitfalls. For example when micro-controller crashes, it becomes "undebuggable",
other than stack trace. 

Keeping also PC builds up to date pays back quickly. Debugging the code in PC helps to find majority of
problems easily, unfortunately not all of them. Thus best way of working is to test software in PC environment
as far as possible, and then continuing with micro-controller testing.


200329, updated 19.5.2020/pekka

