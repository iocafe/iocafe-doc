Arduino IDE 1.8.6 installation, Linux 
======================================
Note: Arduino version 1.8.6 is used here because it works with teensyduino. You may want to check which new versions work with your hardware.

Prequsities
*************
Clone /coderoot/eosal, /coderoot/iocom and /coderoot/pins git repos - These are (et least for now) needed for JSON to C scripts, etc 
/coderoot/bin/linux/json build

download Arduino IDE 1.8.6, extract it to /acoderoot/arduino-1.8.6. 

cd /coderoot/arduino-1.8.6
tree -L 1:

(base) john@iocafe:/coderoot/arduino-1.8.6$ tree -L 1

::

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

./install.sh
./arduino-linux-setup.sh


Setup Arduino IDE
*******************
Go to [File] preferenses
Uncheck "Check for updates on startup"
Check "Show verbose output during compilation"
Select/setup your board from [Tools].

Add libraries
***************
The library source code files will be in subdirectory arduino or esp32 within /coderoot/lib folder. Platform IO
will use files directly from this location. Zipping individual library folders under /coderoot/lib/esp32
or arduino will result zip libraries usable in Arduino IDE. 

[Sketch][Include library][Add .ZIP Library], browse to /coderoot/lib/arduino-zips (or wherever the .zip libs are) add *-eosal.zip library.
Add *-iocom.zip and *-pins.zip libraries the same way.

Add file to set serial RX buffer size ro 256 (or at least to 128)


C code

arduino.ino
config
scripts

Use json linter https://jsonlint.com/ to verify that signals.json syntax is valid. If not, build will fail and it may be hard to see where the probelm is.

Run scripts/config_to_c_code.py to covert signal configuration to C code and compressed JSON for device info memory block.

Creating arduino zip libraries
*******************************
If you do not have Arduino .zip libraries ready, and you have checked out /coderoot/eosal, coderoot/iocom and coderoot/pins,
these can be created by script. Creating the .zip libraries also removes the old versions from Arduino IDE.

cd /coderoot/iocom/scripts
./make-arduino-zips.sh

To do
Write JSON to C scripts so that they can be used without cloning whole git repos.


210126, updated 26.1.2021/pekka
