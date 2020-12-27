Linux build notes
===================
Build is cmake based, so anything which can work with it should do just fine. The cmake is like preprocessor
to generate platform specific make, etc. files. One writes build instructions in cmake syntax in CmakeLists.txt
file, and cmake will generate the make files for actual build system to use. It can generate many different ones.
You can as well build with cmake from command line, or use Eclipse, other IDE, etc. QTCreator is an exception,
it can use CmakeLists.txt directly. The cmake version should not make much difference, the projects here do not
use the new cmake features.

QT creator is good choice. I have used QT creator 4.0.1, QT  5.6 and cmake for Linux builds. To build a project,
start QT creator and open CmakeLists.txt file from project’s root folder. QT creator is easy to set up and use,
and works well. I appreciate easy debugging and it pointing out the build errors.


Dependencied:
sudo apt-get install libv4l-dev


200111, updated 27.12.2020/pekka
