Linux installation packages
============================

Linux installation packages are .deb files. When a program in linux based device receives a .deb file
trough IOCOM, it quietly installs it. The installation is done as root user


Creating a debian package
**************************

This text refers to candy (camera) application in /coderoot/iocom/examples/candy directory. All we want
is to install to target computer is one file, application binary /coderoot/bin/linux/candy. Build candy 
project as release to create the binary.

File permissions, owner and group. All files in installation package are owned by root.
If binary needs to update software, set setuid bit for it needs to be set so it will
run as root. All files which are not to be modified by user should have 0755 permissions,
except 04755 for binary files capable of software updates. Data files which can be modified
by user should have 0664 or 0666. Using 0664 allows extra protection by user group, but
this requires some extra configuration.

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
   sudo mkdir -p "coderoot/production/bin/linux"
   cd /coderoot/iocom/examples/candy/pack/linux/coderoot/production/bin/linux
   sudo cp /coderoot/bin/linux/candy .

Set owner, groups and permissions.
04755 for binaries: The file owned by root and we need to give run time root permission user, 
we add '4' to set setuid bit. Running as root allows application to install debian packages
for software updates, but is a security concern: App infrastructure needs to be set up so
that malicious packages cannot be received trough IOCOM, and application cannow spawn malicious
executable code with root rights.
If we would have data files, we may want to use other permissions than 0755 for those. 
:: 

   cd /coderoot/iocom/examples/candy/pack/linux
   sudo chown --recursive root *
   sudo chgrp --recursive root *
   sudo chmod --recursive 0755 * 
   sudo chmod --recursive 04755 coderoot/production/bin/linux

Create a debian package 
To do that we use the dpkg-deb tool. 

::

   cd /coderoot/iocom/examples/candy/pack
   dpkg-deb --build linux


This creates package linux.deb, which needs to be renamed so that the file name includes the IO 
application name, program version and the package architecture. Convention used by us is to
keep all installation packages in /coderoot/packages directory. I make version numbers simply
by YYMMDD-HHMM. This allows sorting versions to time order and selecting newest one. 
Convention of naming debian packages as organization-application-versiondate-versiontime-architecture.deb
enables meaningfull searches. For example to find newest available package for application "candy" 
running on "amd64" architecture.

::

   mkdir -p "/coderoot/packages/linux"
   cd /coderoot/iocom/examples/candy/pack
   mv linux.deb /coderoot/packages/linux/iocafe-candy-200703-0915-amd64.deb

There is good tool "lintian" to verify that generating .deb package matches to Debian guidelines.

::

   lintian /coderoot/packages/linux/iocafe-candy-200703-0915-amd64.deb


To do/consider
***************

* Now installation target is always /coderoot/production. Should we follow common linux conventions to set installation target?
  We need to consider read only disk partitions (stable Raspberry setup), and how operating system, application code, settings
  and data are placed on disk separate disc partitions.
* The "production" directory name could be replaced with install date. When new software is installed, we would first make
  copy of the latest install directory to todays directory, and install new package on top of it. Newest install is 
  directory is always used at boot. This would enable very straight forward rollback, just delete the newest install 
  directory and newest of the remaining ones will be used. 
* Notes here for creating .deb packages are good enough to get started. We should set/check owner and permissions for 
  files in .deb more precisely by debian guidelines. Also license information (copyright file) and change log need to be 
  included in the debian packages. 
* Now this is plain installation. It is necessary to plan how the installed packages are started when the linux computer
  boots, or after software upgrade. To allow multiple applications to run in same linux computer, we might write common daemon
  which starts applications for specific device numbers. Notice that in theory it same application can be started multiple
  times with different serial numbers, or for example raspberry can act as both IO device and controller.
* Currently examples contain fixed rpath values to find .so libraries. It is necessary to get rid of these dependencies.
* Making installation package can be automated as Python or shell script, which could be called from cmake when a release
  build is completed.
* We need to take GPG signing and verification as practise as soon as time allows. Installation packages may be emailed
  and copied, and malicious tampering is a possibility.
  https://blog.packagecloud.io/eng/2014/10/28/howto-gpg-sign-verify-deb-packages-apt-repositories/
  
200702, updated 3.7.2020/pekka
