Using the library
=================

Data types and functions
************************
The Gazerbeam receiver structure holds settings and current state of monitoring photo transistor signal. 

* Initialize gazerbeam structure and set pin to monitor.
* Poll for new received messages.


The library source code
***********************
* The source code is located in /coderoot/iocom/extensions/gazerbeam directory. 


Building the library
********************
How to build the library depends on target platform and tools used. Cmake builds are quite straight 
forward, there is CMakeLists.txt in root folder, which can be opened for example in QT creator.

* Cmake: Either build project separately or include it as sub-project within in your project
* ESP32/Arduino/Platform IO. Copy files. Configure platformio.ini.
* TODO: Add here link to build instructions for different platforms.

Include headers and link library with your project
The path iocom/extensions/gazerbeam should be added to your project build. This allows including 
the library headers by “#include “gazerbeam.h”.


Dependencies
************
The gazerbeam depends on libraries:

* eosal - platform/operating system abstraction
* pins - portable access to IO pins 
* iocom - some data types, not really much needed but dependency is there.


HW support
**********

* Tested on  ESP32/Arduino/PlatformIO environment. 
* To port to other hardwares, the HW support needs to be implemented in eosal and pins libraries.  

200318, updated 20.5.2020/pekka

