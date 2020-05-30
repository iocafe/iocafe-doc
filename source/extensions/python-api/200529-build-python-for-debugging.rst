iocpython - python build
===================================
Used for debugging python C extensions.
Stick with Python version 3.7 for now (30.5.2020), I tried 3.9 and ended up in dead end with Kivy.
I did not try 3.8.

Directories
************

::

    /coderoot/cpython Python source code cloned from GitHub
    /coderoot/python Installation of self compiled python

Install dependencies
*********************

* Notice that we use 3.8 dependencies even we are builing for 3.7, those do not exists readily in this form. 

::

   sudo apt-get update
   sudo apt-get install build-essential 
   sudo apt-get build-dep python3.8

Just to make it doubly sure that we got all, run

::
   
   sudo apt-get update \
      && sudo apt-get install -y build-essential git libexpat1-dev libssl-dev zlib1g-dev \
     libncurses5-dev libbz2-dev liblzma-dev \
     libsqlite3-dev libffi-dev tcl-dev linux-headers-generic libgdbm-dev \
     libreadline-dev tk tk-dev   
  

Clone python sources from Github
*********************************

::

    cd /coderoot
    git clone https://github.com/python/cpython.git

Select python release
**********************
Compile stable branch, I select python version 3.9. 

::

    cd /coderoot/cpython
    git checkout 3.7


Build
******

::

    cd /coderoot/cpython && ./configure --prefix=/coderoot/python \
      --enable-loadable-sqlite-extensions \
      --enable-shared \
      --with-lto \
      --enable-optimizations \
      --with-system-expat \
      --with-system-ffi \
      --enable-ipv6 --with-threads --with-pydebug --disable-rpath \
      && make 

Install
********
Compiled Python will be installed in /coderoot/python

::

	cd /coderoot/cpython
	make install

Test that it works so far
*******************

::

    cd /coderoot/python/bin
    export LD_LIBRARY_PATH=/coderoot/python/lib
    ./python3.7

Make python environment to work (virtual environment)
******************************************************

::

   cd /coderoot/python/bin
   export LD_LIBRARY_PATH=/coderoot/python/lib
   ./python3.7 -m venv debugpython 
   ... stuff happens ...

   cd /coderoot/python/bin
   . ./debugpython/bin/activate


Simply type deactivate if you need to deactivate

::
    deactivate

When running in pythondebuf virtual environment, terminal prompt should show something like 
"(debugpython) (base) john@iocafe:/coderoot/python/bin$". The (base) means that we have
no anaconda virtual environment activated. 
This is a bit confusing: We have two kinds of Python virtual environments, those with
conda and those with venv.

Installing Kivy
****************
Kivy from "master" branch must be used, older did not work with Python 3.9, etc new stuff (30.5.2020). 
I needed to run install also with sudo to make installation complete "sudo pip install kivy[base]..." ? 

::
    sudo apt-get install -y \
        ffmpeg \
        libsdl2-dev \
        libsdl2-image-dev \
        libsdl2-mixer-dev \
        libsdl2-ttf-dev \
        libportmidi-dev \
        libswscale-dev \
        libavformat-dev \
        libavcodec-dev \
        zlib1g-dev
    
    pip install kivy[base] kivy_examples --pre --extra-index-url https://kivy.org/downloads/simple/

Debug Python with QT creator 
******************************
QT creator needs to be started with script which sets LD_LIBRARY_PATH and PYTHONPATH do that debugging works.

* Select run in terminal if opening from file explorer.
* Remember to build DEBUG version of iocompython.

::
    
    /coderoot/iocom/extensions/iocompython/scripts/start-qtcreator-for-python-debug.sh

Select [Debug][Start Debugging][Start and Debug External Application].
Set Python program to debug as command line argument.

.. figure:: pics/start-debugging-python.png

   debug python as external application.



sudo update-alternatives  --set python /coderoot/python/bin/python3.9d
   sudo update-alternatives  --set python /coderoot/python/bin/python3.9d

   udo update-alternatives --install /usr/bin/python python /coderoot/python/bin/python3.9d 2