Build and test IOCOM examples on Linux or Windows
=================================================

We start with this, because building and debuggin on PC is much easier than on microcontroller.
This also gives idea how things are supposed to work.


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



200516, updated 19.5.2020/pekka
