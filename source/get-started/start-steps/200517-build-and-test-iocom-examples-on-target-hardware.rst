Build and test IOCOM example on target hardware
=================================================

Stable versions
***************
If your goal is to create IO application, use stable version. If there is one for your hardware and development environment. 
The stable versions will be marked by github tags. If there is none, please email me pekka.lehtikoski@iocafe.org.


Latest version
**************
The latest version should be used if you participate in development of IOCOM or another related library. 
If your goal is to create IO device application, latest code from "master" is likely to bring you unwelcome issues.


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


200517, updated 19.5.2020/pekka
