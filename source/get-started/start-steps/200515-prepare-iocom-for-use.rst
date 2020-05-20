Prepare IOCOM for use
===========================================


We avoid placing compiled binaries in shipping com

* json tool
* iocompython

ESP32/Arduino: Run scripts


Place library source code in /coderoot directory
************************************************
At least for now, the library development has been done under directory /coderoot. I strongly recommend to do so,
since using other paths is untested and would require modifications to build, etc, scripts. 
To create the /coderoot directory and set it's owner:

.. code-block:: shell

   cd /
   sudo mkdir coderoot
   sudo chown <yourusername> coderoot

On windows create directory c:\\coderoot



200515, updated 19.5.2020/pekka
