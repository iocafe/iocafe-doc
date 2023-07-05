Linux build notes
===================
Build is cmake based, so any tool which can works with cmake should work fine. The cmake is like preprocessor
to generate platform specific make, etc. files. One writes build instructions in cmake syntax in CmakeLists.txt
file, and cmake will generate the make files for actual build system to use. It can generate many different ones.
You can as well build with cmake from command line, or use Eclipse, other IDE, etc. QTCreator is an exception,
it can use CmakeLists.txt directly. The cmake version should not make much difference, the projects here do not
use the new cmake features.

Visual Studio Code or QT creator are good choices for IDE. I have mostly used QT creator 4.0.1, QT 5.6 and cmake for
Linux builds. To build a project, start QT creator and open CmakeLists.txt file from project’s root folder.
QT creator is easy to set up and use, and works well. I appreciate easy debugging and it pointing out the build
errors.


Dependencies:
**************

Camera:

- sudo apt-get install libv4l-dev

egui/glass:

- sudo apt-get install libglfw3-dev libglew-dev 

cmake specific:

- sudo apt-get install --reinstall pkg-config cmake-data


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


200111, updated 6.7.2023/pekka
