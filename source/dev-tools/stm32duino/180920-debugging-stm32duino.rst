Debugging video clip
=====================
hobby project notes 20.9.2018/pekka

https://hackaday.com/2017/03/30/the-2-32-bit-arduino-with-debugging/
https://youtu.be/D-MbC9N5U7o

Select from Arduino IDE
ST Link
Optimize, Debug

STM32 build tools: /home/pekka/.arduino15/packages/STM32/tools/arm-none-eabi-gcc/6-2017-q2-update/bin

Arduino IDE build folder: /tmp/arduino_build_176781
All source code in one big file like : /tmp/arduino_build_176781/sketch/WebServer.ino.cpp

Connect to device using st-util. Download from github, build and install globally ldconfig:
st-util -p 1234

cd to build folder, start exactly same gdb as used to compile:
/tmp/arduino_build_176781
/home/pekka/.arduino15/packages/STM32/tools/arm-none-eabi-gcc/6-2017-q2-update/bin/arm-none-eabi-gdb -tui WebServer.ino.elf
target extended-remote :1234
load
break osal_socket_accept
continue
n
bt

GOT LOCKED UP?

* Download ST link utility on windows,
* I've run the ST-LINK Utility (on windows) and I went to Target > Option Bytes... > Disable Read Out Protection (combo box). Done, issue solved.


