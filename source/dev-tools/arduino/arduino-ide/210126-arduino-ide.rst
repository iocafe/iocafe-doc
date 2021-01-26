Arduino IDE 1.8.6 installation, Linux 
======================================

Prequsities
*************
Clone /coderoot/eosal, /coderoot/iocom and /coderoot/pins git repositories. These are, at least for now, needed for JSON to C conversion scripts.
Also JSON compression tool (C code) needs to compiled. This is in /coderoot/eosal/util/json directory.

Arduino IDE installation.
**************************
Arduino version 1.8.6 is used here because it works with teensyduino. 
You may want to check which new versions work with your hardware.

Download Arduino IDE 1.8.6, extract it to /acoderoot/arduino-1.8.6. It should be as follows:

::

   cd /coderoot/arduino-1.8.6
   tree -L 1:

   (base) john@iocafe:/coderoot/arduino-1.8.6$ tree -L 1
   .
   ├── arduino
   ├── arduino-builder
   ├── arduino-linux-setup.sh
   ├── examples
   ├── hardware
   ├── install.sh
   ├── java
   ├── lib
   ├── libraries
   ├── reference
   ├── revisions.txt
   ├── tools
   ├── tools-builder
   └── uninstall.sh


Run scripts to finalize Arduino IDE installation

::

   cd /coderoot/arduino-1.8.6
   ./install.sh
   ./arduino-linux-setup.sh

Aduino IDE setup
******************
Setup Arduino IDE: Go to [File] preferenses, and uncheck "Check for updates on startup". Check "Show verbose output during compilation". 
Select Compiler warnings "More". Select/setup your board from [Tools].

Setting serial RX and TX buffers for Arduino AVR (UNO) build:
This is the tricky part with Arduino IDE. There is no easy way to set compiler defines. So far best what
I found is to modify /coderoot/arduino-1.8.6/hardware/arduino/avr/platform.txt after Arduino IDE installation.
Close Arduino IDE and open platform.txt in editor. Change lines "compiler.c.extra_flags=" and "compiler.cpp.extra_flags="
to following:

:: 
   
   compiler.c.extra_flags=-DSERIAL_RX_BUFFER_SIZE=256 -DSERIAL_TX_BUFFER_SIZE=64
   compiler.cpp.extra_flags=-DSERIAL_RX_BUFFER_SIZE=256 -DSERIAL_TX_BUFFER_SIZE=64

These settings get close to maximum RAM use on UNO. If this is problem, you may try setting RX buffer to 128.
If UNO runs our of RAM, it crashes due to stack overflow and may reboot itself.

Add EOSAL AND IOCOM libraries
*******************************
The library source code files is collected from source tree by scripts in arduino or esp32 subdirectory within /coderoot/lib folder. 
This is done to follow Arduino source library convention. Platform IO will use files directly from this location. 
For Arduino IDE, individual library folders under /coderoot/lib/esp32 or arduino are zipped into "zip libraries". 
These .zip libraries can then be loaded by Arduino IDE. 

If these .zip libraries are ready, load them using Arduino IDE: 

* [Sketch][Include library][Add .ZIP Library] 
* Browse to /coderoot/lib/arduino-zips (or wherever the .zip libs are) and add *-eosal.zip library.
* Add *-iocom.zip and optionally *-pins.zip libraries the same way.

Arduino code examples
**********************
See documentation of "arduino" and "uno" examples.

Use json linter https://jsonlint.com/ to verify that signals.json syntax is valid. If not, build will fail and it may be hard to see where the probelm is.

Run scripts/config_to_c_code.py to covert signal configuration to C code and compressed JSON for device info memory block.

Creating arduino zip libraries
*******************************
If you do not have Arduino .zip libraries ready, and you have checked out /coderoot/eosal, coderoot/iocom and coderoot/pins,
these can be created by script. Creating the .zip libraries also removes the old versions from Arduino IDE.

::

   cd /coderoot/iocom/scripts
   ./make-arduino-zips.sh

To do
*******

* Write JSON to C scripts so that they can be used without cloning whole git repos.
* *WRITE ABOUT PACKAGES NEEDED. ARDUINO IDE, STMDUINO, TEENSYDUINO VERSIONS. 
* USING TEENSYDUINO SOCKET 
* LIBRARY FOR STM32/WIZ5500.  
* MODIFICATION TO SOCKET LIBRARY.
 
210126, updated 26.1.2021/pekka
