Debuggin Python API C extension (iocompython) 
===================================================================

Install dependencies
*********************

::

   sudo apt-get update
   sudo apt-get install build-essential 
   sudo apt-get build-dep python3.8
   sudo apt-get install python3-dev

Just to make it doubly sure that we got all, run

::
   
   sudo apt-get update \
      && sudo apt-get install -y build-essential git libexpat1-dev libssl-dev zlib1g-dev \
     libncurses5-dev libbz2-dev liblzma-dev \
     libsqlite3-dev libffi-dev tcl-dev linux-headers-generic libgdbm-dev \
     libreadline-dev tk tk-dev   
  
Debug Python with QT creator 
******************************
QT creator needs to be started with script which sets LD_LIBRARY_PATH and PYTHONPATH do that debugging works.

* Select run in terminal if opening from file explorer.
* Remember to build DEBUG version of iocompython.
* Here debugging with Python code /coderoot/iocom/extensions/iocompython/examples/ispy/ispy.py

::
    
    /coderoot/iocom/extensions/iocompython/scripts/start-qtcreator-for-python-debug.sh

Select [Debug][Start Debugging][Start and Debug External Application].
Set Python program to debug as command line argument.

.. figure:: pics/start-debugging-python.png

   debug python as external application.


