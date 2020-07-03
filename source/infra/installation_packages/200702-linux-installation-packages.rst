Linux installation packages
============================

Linux installation packages are .deb files. When a program in linux based device receives a .deb file
trough IOCOM, it quietly installs it. The installation is done as user to which account device program runs.


Creating a Debian package, basics 
**********************************

This text refers to candy (camera) application in /coderoot/iocom/examples/candy directory. All we want
is to install to target computer is one file, application binary /coderoot/bin/linux/candy. Build candy 
project as release to create the binary.

First create the debian package structure directories. All files to create the debian package
will be here:

:: 

   cd /coderoot/iocom/examples/candy
   mkdir -p "pack/linux/DEBIAN"

Create the DEBIAN control file. I used here gedit as text editor, use one you prefer. 
This will open editor with empty file:

:: 

   cd /coderoot/iocom/examples/candy/pack/linux/DEBIAN
   gedit control

Copy paste lines below into the control file and modify at least package name, maintainer and description.
Save the file.

:: 

   Package: iocafe-candy
   Version: 1.0
   Section: custom
   Priority: optional
   Architecture: amd64
   Essential: no
   Installed-Size: 1024
   Maintainer: iocafe.org
   Description: Camera IO device software

Create directory tree which represents the path where our program will be installed in the target computer, 
and copy the executable into it:

:: 

   cd /coderoot/iocom/examples/candy/pack/linux
   mkdir -p "coderoot/production/bin/linux"
   cd /coderoot/iocom/examples/candy/pack/linux/coderoot/production/bin/linux
   cp /coderoot/bin/linux/candy .


Create a debian package 
To do that we use the dpkg-deb tool. 

::

   cd /coderoot/iocom/examples/candy/pack
   dpkg-deb --build linux


This creates package linux.deb, which needs to be renamed so that the file name includes the IO 
application name, program version and the package architecture. Convention used by us is to
keep all installation packages in /coderoot/packages directory.

::

   mkdir -p "/coderoot/packages/linux"
   cd /coderoot/iocom/examples/candy/pack
   mv linux.deb /coderoot/packages/linux/iocafe-candy-1.0-amd64.deb


To do or to consider
*********************

* Now installation target is always /coderoot/production. Should we follow common linux conventions to set installation target?
  We need to consider read only disk partitions (stable Raspberry setup), and how operating system, application code, settings
  and data are placed on disk separate disc partitions.
* The "production" directory name could be replaced with install date. When new software is installed, we would first make
  copy of the latest install directory to todays directory, and install new package on top of it. Newest install is 
  directory is always used at boot. This would enable very straight forward rollback, just delete the newest install 
  directory and newest of the remaining ones will be used. 
* Notes here for creating .deb packages are good enough to get started. For real we should set owner and permissions for 
  files to back into .deb by debian guidelines. This has not been done. Also we should incorporate license information in the
  debian package. 
* Now this is plain installation. It is necessary to plan how the installed packages are started when the linux computer
  boots, or after software upgrade. To allow multiple applications to run in same linux computer, we might write common daemon
  which starts applications for specific device numbers. Notice that in theory it same application can be started multiple
  times with different serial numbers, or for example raspberry can act as both IO device and controller.
