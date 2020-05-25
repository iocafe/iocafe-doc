Arduino IDE, board and build notes
===================================
Even this is is called Arduino build,  I recommend using 32 bit ARM architecture micro-controllers and
ESP32/Arduino, STM32Duino or TeensyDuino.  

* ESP32 is The low cost IoT device of today. It features inbuilt WiFi, two cores and plentiful resources.
  In practice the chip is so complex, that it cannot be used as metal: The Espressif operating system
  including FreeRTOS is a must. Noticing low ADC top speed of approximately 100kHz limits use for some applications.
* Teensy 3.6 has really good performance and format that it can be plugged directly into my own PCB. 
  Unfortunately has two big minuses: lack of debugging support and incomplete LWIP library port.
* STM32F429, etc, Nucleo, on the other hand shines where Teensy lack, but it also has a downside when it
  comes to development boards: Nucleo cannot legally be reused “as is” as part of commercial project:
  You have to draw your own PCB and put the STM chip on it, starting already from first customer tests
  alpha version. STM gives you two good coices for development tools, TrueStudio or Arduino based platformio.
* Performance on more powerful versions of actual Arduino is being tested. Even it is possible to load
  iocom in very minimal serial communication configuration to UNO with 2kB of RAM and 32kB flash, this
  is recommended. The iocom alone eats up practically all RAM and flash, leaving very little for anything else.

Copy iocom, etc. libraries to use
***********************************
An Arduino library is compressed package of source code, not collection of compiled object files. 
These must be organized in a specific way. Running scripts below will copy and organize source files
for arduino-ide use (STM32Duino, TeensyDuino, ESP32/Arduino or actual Arduino). 

cd /coderoot/eosal/build/arduino-library
copy-iocom-and-extras-for-arduino.sh
copy-iocom-and-extras-for-esp32.sh

The organized files will be in subdirectory arduino or esp32 within /coderoot/lib folder. Platform IO
will use files directly from this location. Zipping individual library folders under /coderoot/lib/esp32
or arduino will result zip libraries usable in Arduino IDE. 

Remonder to update this page
***********************************
NOTE: WRITE ABOUT PACKAGES NEEDED. ARDUINO IDE, STMDUINO, TEENSYDUINO VERSIONS. USING TEENSYDUINO SOCKET 
LIBRARY FOR STM32/WIZ5500.  MODIFICATION TO SOCKET LIBRARY. SETTING SERIAL COMMUNICATION TX AND RX TO 256 BYTES.


200303, updated 23.5.2020/pekka
