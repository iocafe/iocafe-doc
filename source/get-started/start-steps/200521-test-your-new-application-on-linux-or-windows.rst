Test your new application on Linux or Windows
=============================================

It is a lot easier to build and debug the micro-controller application in PC than in actual target environment.
Making and testing PC build first allows one to work out most of bugs.

Build
***************

* Linux: Cmake build can be used from command line, from QT creator, etc.
* Windows: Use Visual Studio 2019, the library has native .sln files for it.


Test communication
******************

* Run both ends of communication in PC. 
* Often you can test your IO code against one of the examples
* ispy example can be used to view how it works.


Serial communication
********************

* In Windows we have com0com to generate loop back serial ports. Unfortunately this has issues with modern Windows and requires disabling much security to work.
* In linux, tty0tty is really useful. It generates loop back serial port.




