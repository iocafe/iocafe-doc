Atollic True Studio notes
=============================
Atollic True Studio is professional eclipse based development environment primarily for STM32 micro-controllers.  The IDE is similar to Keil or even QT or Visual Studio, and debugging works very nicely.  In addition to IDE, there is Cube. Cube makes assigning micro-controller pins and timing easy (much much easier than reading thick reference manuals and doing it by hand). It generates code to initialize the micro-controller, which can then be used in True Studio project. 

Advice: If you are new to programming micro-controllers, starting out cold with building the iocom library with True Studio will be hard. Better to start either with True Studio examples, or even better with STM32duino examples. Way from hello world “blink led”, proceeding step at a time, is likely to yield results faster than time than trying to take it all in with single bite.

What is great about Atollic True Studio and Cube
***************************************************

* Cube is excellent tool for assigning micro-controller pins and timing.
* True Studio IDE has excellent debugging trough IDE. Notice that Nucleo boards come with
  integrated ST-link USB connector, or if you have board without one,  a $6 “ST-Link V2 Mini
  STM8 STM32 Emulator Downloader” from Amazon works well (connects USB to micro-controller pins). 
* Nice professional look and feel. Full functionality without any licensing fees. Experienced
  IDE users familiar with Eclipse, Kail, QTCreator, Visual Studio, etc. feel quickly at home.
* Integration of LWIP and FreeRTOS to Cube.
* Great ready support and examples for ST Microelectronics ARM32 products.
* Good, although not trouble free, system to keep developers up to date. 

What is not so great
***************************************************

* Wants to integrate Cube generated code with application and hardware. 
* This easily leads to bloated, non reusable application projects. Extra work is needed to keep 
  IO device application code portable, to separate it from Cube generated files, and to avoid
  duplicating same Cube and C source files.
* Low abstraction level, chip specific implementations are often needed. For instance Ethernet
  requires separate implementations for Wiznet and for LWIP, and micro controller chip specific
  serial port driver code and flash programming. HAL provides some level of portability, but not
  much. When comparing STMduino/Teensyduino which has APIs for these (others but for flash):
  This approach allows developer great control of detail, but also consumes lot of time and 
  requires expertise to get the reliability.
* Much steeper learning curve than STM32duino or TeensyDuino.
* Dependency on Cube versions. Cube and True Studio version updates have caused, and are 
  likely to cause future headaches. 
* Cube support for other manufacturers than ST microelectronics practically null. 
  True Studio can still be used for generic ARM builds and should work for debugging.

Static libraries and settings
***************************************************

* Static libraries like iocom, eosal and w5500 have True Studio project in build/atollic folder. 
  This will build the static library in build/atollic/Debug, etc, folder.
* Compiler defines E_OS_metal and STM32L476xx (or other) are set for static libraries. 
  The E_OS_metal indicates that we are compiling for “bare metal” or close to it, anyhow to system
  without typical operating system (we may still use HAL/CMSIS and FreeRTOS). The STM32L476xx sets 
  micro-controller type. This is needed by HAL.
* Include directories coderoot/eosal, coderoot/iocom allow build with eosal and iocom headers. 
* Include directory coderoot/eosal/libraries/WIZnetIOlibrary/Ethernet allows build with WizNET w5500 IO library.
* Include directories related to HAL and CMSIS and come with STM32 Cube. Once support for a specific micro-controller
  family, lets say STM32F4, is installed within Cube, full copy HAL and CMSIS appears in user’s home 
  repository ~/STM32Cube/Repository/STM32Cube_FW_L4_V1.13.0. 

200111, updated 22.5.2020/pekka
