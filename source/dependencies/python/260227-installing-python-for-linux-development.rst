Installing Python for Linux development
===================================================================

At the time of writing, Python version is 3.12.3, any Python 3
version should work just fine.

Check version number, you may already have a new python3 version installed:

::

    python3 --version

If not, install Python 3 for Linux:

::

    sudo apt update
    sudo apt upgrade (updates all packages to newest versions, you may want to skip this)
    sudo apt install python3
    sudo apt install python3-pip


Python libraries and headers are needed to build iocompython. The iocompython
is a C extension to allow use of iocom from Python. To install the Python components
for this use:

::

   sudo apt update
   sudo apt install build-essential
   sudo apt install python3-dev

Set PYTHONPATH in as last line of .profile file in home directory. When user logs in,
the .profile is processed. This enables Python to find iocompython.pyd, etc.

::

   export PYTHONPATH=${PYTHONPATH}:/coderoot/bin/linux

260227, last updated 27.2.2026/pekka
