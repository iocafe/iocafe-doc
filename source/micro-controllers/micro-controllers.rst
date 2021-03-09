Micro-controllers
====================

* C/C++, mostly plain vanilla C. gcc is dominant and always good + some others are also fine, plain standard C needed. 
* Arduino library derivatives like stm32duino and teensyduino simplify and make apps more portable.
* FreeRTOS and RT-Thread (later not tested by me) - useful for some apps - a necessity for multi-core micro-controllers.
* LWIP or uIP Ethernet stack can run on micro-controllers, smart Ethernet chips like WizNET have inbuilt stack and small API lib for micro-controller.
* Use standard TLS - comes as dev library for many micro-controller environments or development tools, OpenSSL to implement for Linux/Windows.
* ARM32 architecture dominant – one assembler to know - one set of build tools compilers - very scalable, from $1 to $20.
* Using “Raspberry PI style” Linux devices is an option. Read only SD file system is required for reliable operation and adds to complexity.

.. toctree::
   :maxdepth: 2
   :caption: Contents:

   esp32/esp32
   zynq/EBAZ4205/EBAZ4205
   

