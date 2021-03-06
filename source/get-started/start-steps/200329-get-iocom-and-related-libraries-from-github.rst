﻿Get IOCOM and related libraries
===========================================

NOTICE: WE ARE IN PROCESS OF LIBRARY DEVELOPMENT. THE TEXT BELOW IS ONLY A PLAN HOW TO DEAL WITH STABLE VERSIONS 
ONCE WE GET THE FIRST IOCOM RELEASE PUBLISHED. 

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


Get Git
************************************************
You need Git, if you have not it already. Git is a distributed version control system which allows many people 
to share and work on the same source code. On Debian based Linux distributions:

.. code-block:: shell

    sudo apt install git


Clone IOCOM, EOSAL and PINS libraries
************************************************
The IOCOM and related source code is stored in GitHub repository. The GitHub is a website and service that stores 
code repositories. IOCOM is public repository, GitHub account is not needed. To clone the library code
to your computer do: 

.. code-block:: shell

   cd /coderoot
   git clone https://github.com/iocafe/iocom.git
   git clone https://github.com/iocafe/eosal.git
   git clone https://github.com/iocafe/pins.git

You should get something like tree below under /coderoot directory:

.. code-block:: text

    ├── eosal
    │   ├── code
    │   ├── dependencies
    │   ├── doc
    │   ├── examples
    │   ├── extensions
    │   ├── metals
    │   ├── osbuild
    │   ├── scripts
    │   └── util
    ├── iocom
    │   ├── code
    │   ├── config
    │   ├── doc
    │   ├── examples
    │   ├── extensions
    │   ├── osbuild
    │   └── scripts
    ├── pins
    │   ├── code
    │   ├── doc
    │   ├── examples
    │   ├── extensions
    │   ├── osbuild
    │   └── scripts


Updating library code
************************************************
To get the newest library code, pull it for GitHub. If you have a working development environment,
make a backup of /coderoot directory before doing this: Since the library is in development phase, 
compatibility issues may arise. 

.. code-block:: shell

   cd /coderoot/iocom
   git pull
   cd /coderoot/eosal
   git pull
   cd /coderoot/pins
   git pull

200329, updated 19.5.2020/pekka
