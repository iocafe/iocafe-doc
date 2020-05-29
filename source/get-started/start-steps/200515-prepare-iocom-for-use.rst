Prepare IOCOM for use
===========================================

Build C tools
*************
There are a few C tools needed for the system and these need to be build with cmake or Visual Studio before
proceeding to compile examples or other IO applications.

* /coderoot/eosal/util/json - Command line utility to compress/decompress JSON
* /coderoot/iocom/extensions/iocompython - Python API to IOCOM
* /coderoot/eosal/dependencies/eosal_jpeg - JPEG compression library


Copy library code for ESP32/Arduino development
***********************************************
The Arduino style environment expect libraries as source code, organized in a specific manner under one folder. 
Scripts like copy-iocom-and-extras-for-esp32.sh do this. You need to run these scripts every time the library
code is updated, either by you or updated pulled from Git, before running build for target hardware.

.. code-block:: shell

   cd /coderoot/iocom/scripts
   ./copy-iocom-and-extras-for-esp32.sh 
   ./copy-iocom-and-extras-for-arduino.sh 


Open issues
***********
* There are a few pre compiled Linux/Windows binaries for Mbed TLS and OpenSLL in /coderoot/eosal/dependencies.
* Compiling iocompython requires Python libraries. In both Linux and Windows some version of these comes
  with Python installation. Unfortunately not all builds are included and include library paths do vary. 


200515, updated 19.5.2020/pekka
