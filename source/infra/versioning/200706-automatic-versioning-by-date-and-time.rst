Automatic version numbers by date and time
===========================================

Iocafe convention: version numbers are simply date/time of latest build in YYMMDD-HHMM format. 
This allows automatic version number generation and ensures that I do not forget to change the
version number. This format sorting versions to time order, selecting newest version of
a specific package and matching installation package loosely to sources in git repository
(assuming that the installation packages are created from latest version). 

In other word, version number is just build timestamp. This very simple convention works in fine 
for small team when installation package is almost always generated from latest code in repository.

This is only a convention, feel free to use other king of versioning.
Downsides on simple date/time based versioning are:

* Version numbers are "longish".
* Major and minor versions are not separated. 
* Doesn't work well for projects where branching is common or packages are not generated from latest code.

Scrip to generate version numbers
***********************************

The /coderoot/eosal/scripts/set_version.py writes current date and time from computers clock
as version number to /coderoot/eosal/eosal_version.h and  /coderoot/eosal/eosal_version.txt.
First one is intended to be included from C code and second one read by scripts generating
installation packages, etc. 

Version information files
**************************

The /coderoot/eosal/eosal_version.h just defines three macros OSAL_BUILD_DATE, OSAL_BUILD_TIME
and OSAL_BUILD_DATETIME.

::

    #define OSAL_BUILD_DATE "200707"
    #define OSAL_BUILD_TIME "0953"
    #define OSAL_BUILD_DATETIME "200707-0953"

The /coderoot/eosal/eosal_version.txt contains only OSAL_BUILD_DATETIME string without quotes.

::

    200707-0953

Script for copying and renaming installation packages
******************************************************

Script /coderoot/eosal/scripts/copy_package.py can be used to copy the installation package
into /coderoot/packages directory and rename it. It reads the eosal_version.txt to get version (build date+time)
and incorporates it into package name.

:: 

    python3 /coderoot/eosal/scripts/copy_package.py /tmp/.../firmware.bin -a candy -s esp32 -h espcam -o iocafe

Library version defines 
************************
Main library headers include version number define. At the time of writing these are:
These can be used as to indicate which versions have been used to build a package.

* #define EOSAL_VERSION "0.3"
* #define IOCOM_VERSION "0.3"
* #define PINS_VERSION "0.3"

Warning: These version number defines are are not updated daily with work flow, so this
version number doesn't explicitly specify the library code used. We plan to change these
only with new major release of the library.


200706, updated 6.7.2020/pekka

