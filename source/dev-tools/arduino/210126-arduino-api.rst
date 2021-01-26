Arduino API and IDE
====================
Over the years arduino API has expanded as "de facto" API to microcontrollers and development boards 
from many manufacturers. In addition to original Arduino boards, the API is supported SMT32 chips as 
stmduino, Teensy developer boards as teensyduino and is supported for ESP32. 

Even standardization is far from perfect and we end up with some HW specific stuff, it is generally 
easy and clear API to use. Best widely accepted effort in standardization of wild west of microcontrollers.

Most popular two development environments for Arduino API are the original Arduino IDE and 
Visual Studio Code + PlatformIO. PlatformIO is more advanced of the two, includes debuggings,
etc. Arduino IDE is simpler to use. This chapter is about 

Arduino IDE notes, linux

.. toctree::
   :maxdepth: 2
   :caption: Contents:

   200303-duino-boards.rst
   arduino-ide/210126-arduino-ide.rst


The organized files will be in subdirectory arduino or esp32 within /coderoot/lib folder. Platform IO
will use files directly from this location. Zipping individual library folders under /coderoot/lib/esp32
or arduino will result zip libraries usable in Arduino IDE. 

Todo
***********************************
NOTE: WRITE ABOUT PACKAGES NEEDED. ARDUINO IDE, STMDUINO, TEENSYDUINO VERSIONS. USING TEENSYDUINO SOCKET 
LIBRARY FOR STM32/WIZ5500.  MODIFICATION TO SOCKET LIBRARY. SETTING SERIAL COMMUNICATION TX AND RX TO 256 BYTES.


210126, updated 26.1.2021/pekka
