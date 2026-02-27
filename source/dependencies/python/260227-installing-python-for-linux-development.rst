Installing Python for Linux development
===================================================================

At the time of writing, Python version is 3.12.3, any Python 3
version should work just fine.

Check version number, you may already have python3 installed

::

    python3 --version

If not, install Python 3 for Linux:

::

    sudo apt update
    sudo apt upgrade (updates all packages to newest versions, you may want to skip this)
    sudo apt install python3
    sudo apt install python3-pip


Python libraries and headers are needed to build iocompython. This is
C extension to allow Python to use iocom. To install these:

::

   sudo apt update
   sudo apt install build-essential
   sudo apt install python3-dev

260227, last updated 27.2.2026/pekka
