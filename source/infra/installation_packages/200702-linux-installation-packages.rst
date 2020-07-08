Linux installation packages
============================

Linux installation packages are .deb files. When a program in linux based device receives a .deb file
trough IOCOM, it quietly installs it. The installation is done as root user.

Note: Release build is needed for installation package. This is easy to miss when using QT creator
as IDE.

Creating a debian package
**************************

This text refers to candy (camera) application in /coderoot/iocom/examples/candy directory. All we want
is to install to target computer is one file, application binary /coderoot/bin/linux/candy as 
/opt/organization/bin/linux/candy. Build candy project as release to create the binary.

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
   sudo gedit control

Copy paste lines below into the control file and modify at least package name, maintainer and description.
Save the file.

:: 

   Package: iocafe-candy
   Version: 200703-0915
   Section: embedded
   Priority: optional
   Architecture: amd64
   Installed-Size: 1024
   Maintainer: iocafe.org
   Description: Camera IO device software

Create directory tree which represents the path where our program will be installed in the target computer, 
and copy the executable into it. Replace "organization" with your name of choice:

:: 

   cd /coderoot/iocom/examples/candy/pack/linux
   sudo mkdir -p "opt/organization/bin/linux"
   cd /coderoot/iocom/examples/candy/pack/linux/opt/organization/bin/linux
   sudo cp /coderoot/bin/linux/candy .

Set owner, groups and permissions.
04755 for binaries: The file owned by root and we need to give run time root permission user, 
we add '4' to set setuid bit. Running as root allows application to install debian packages
for software updates, but is a security concern: App infrastructure needs to be set up so
that malicious packages cannot be received trough IOCOM, and application cannot spawn malicious
executable code with root rights.
If we would have data files, we may want to use other permissions than 0755 for those. 
:: 

   cd /coderoot/iocom/examples/candy/pack/linux
   sudo chown --recursive root *
   sudo chgrp --recursive root *
   sudo chmod --recursive 0755 * 
   sudo chmod --recursive 04755 opt/organization/bin/linux

Create a debian package 
To do that we use the dpkg-deb tool. 

::

   cd /coderoot/iocom/examples/candy/pack
   dpkg-deb --build linux


This creates package linux.deb, which needs to be renamed so that the file name includes the IO 
application name, program version and the target architecture. Convention used by us is to
keep all installation packages in /coderoot/packages directory. I make version numbers simply
by YYMMDD-HHMM. This allows sorting versions to time order and selecting newest one. 
Convention of naming debian packages as organization-application-versiondate-versiontime-osname-architecture.deb
enables meaningful searches. For example to find newest available package for application "candy" 
running on "amd64" architecture.

::

   mkdir -p "/coderoot/packages/linux"
   cd /coderoot/iocom/examples/candy/pack
   mv linux.deb /coderoot/packages/linux/iocafe-candy-200703-0915-linux-amd64.deb

There is good tool "lintian" to verify that generating .deb package matches to Debian guidelines.

::

   lintian /coderoot/packages/linux/iocafe-candy-200703-0915-linux-amd64.deb

Script to create debian packages
*********************************
The make_debian_package.py scripts can be used to create simple debian installation package. This script
is at least for now limited to very basic applications consisting only one binary executable file.

::

   /coderoot/eosal/scripts/make_debian_package.py

One can write short application/target system specific script to call make_debian_package.py to create debian 
package for the application on specific installation target. See Candy camera's script to generate installation
for 64 bit linux:

::

   /coderoot/iocom/examples/candy/scripts/make_linux_amd64_package.py


Packages are installed in /opt/organization directory, where "organization" is name of organization, 
"iocafe" by default. 

The script doesn't use "pack" directory under product to prepare debian package, but creates
temporary directory under "/tmp" directory.


To do/consider
***************

* Now installation target is always /opt/organization. We really cannot follow common linux conventions to set installation target?
  We need to consider read only disk partitions (stable Raspberry setup), and how operating system, application code, settings
  and data are placed on disk separate disc partitions.
  /usr/local/bin is for normal user programs not managed by the distribution package 
  manager, e.g. locally compiled packages. You should not install them into /usr/bin 
  because future distribution upgrades may modify or delete them without warning.
* Notes here for creating .deb packages are good enough to get started. We should set/check owner and permissions for 
  files in .deb more precisely by debian guidelines. Also license information (copyright file) and change log need to be 
  included in the debian packages. 
* Now this is plain installation. It is necessary to plan how the installed packages are started when the linux computer
  boots, or after software upgrade. To allow multiple applications to run in same linux computer, we might write common daemon
  which starts applications for specific device numbers. Notice that in theory it same application can be started multiple
  times with different serial numbers, or for example raspberry can act as both IO device and controller.
* Currently examples contain fixed rpath values to find .so libraries. It is necessary to get rid of these dependencies.
* We need to take GPG signing and verification as practice as soon as time allows. Installation packages may be emailed
  and copied, and malicious tampering is a possibility.
  https://blog.packagecloud.io/eng/2014/10/28/howto-gpg-sign-verify-deb-packages-apt-repositories/
* now osal_create_process() in linux modifies real user and group to start dpkg with root permissions. Functions
  getuid(), getgid(), setuid(), setgid() are used for this. It would be nicer to elevate effective user with 
  geteuid(), getegid(), seteuid(), and setegid() functions. These for some reason did not work.
* List which packages from linux distribution are needed to make this work, at least binutils, lintian.
* Cosmetic: If extra time, set up override disable lintian warning "dir-or-file-in-opt". We need to install
  application binaries in directory which can be mapped to specific partition, so /usr/local/bin will not do.

Hints
******
* If you get "insufficient user privileges" message, then most likely "setuid bit" in binary file permissions is not
  set. See "Set owner, groups and permissions" in this paper.

200702, updated 8.7.2020/pekka

