Linux build notes
===================
Build is cmake based, so any IDE, etc tool which can works with cmake should
be fine. I recommend Visual Studio Code. As well, it is possible to run cmake directly from the command.

The developer writes build instructions in CmakeLists.txt file at project's root directory. The cmake is like a preprocessor, it generates platform and build tool specific make, etc. files. It supports many build tools, like GNU make, Ninja...

QTCreator is an exception, it uses CmakeLists.txt files directly.

Visual Studio Code or QT creator are good choices for IDE. In past I have mostly used QT creator for Linux builds, nowdays mostly Visual Studio Code.


Dependencies:
**************

pins (for camera):

- sudo apt-get install libv4l-dev

egui/glass:

- sudo apt-get install libglfw3-dev libglew-dev 

cmake specific:

- sudo apt-get install pkg-config cmake-data

Document build:

- sudo apt-get install python3-sphinx

Raspberry PI specific
**********************

- raspicam, see /coderoot/pins/downloads/raspberry
- pigpio, see /coderoot/pins/downloads/raspberry


Raspberry PI, openGL for egui/glass
**************************************

To activate the OpenGL drivers you need to run raspi-config from a terminal window as this option is not available in the version in the menu.

- Enter the comand: sudo raspi-config
- Select 7 Advance Options
- GL Drivers
- Select "GL (Fake KMS) Desktop Driver"

I think OpenGL ES 2.0 is needed??? Check:

- https://github.com/zertyz/hello_imgui_rpi
- https://github.com/ocornut/imgui/pull/2837

200111, updated 25.2.2026/pekka
