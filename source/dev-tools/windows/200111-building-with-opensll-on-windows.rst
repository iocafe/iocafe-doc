Building OpenSSL on Windows VS 2019
=======================================

Either MbedTLS or OpenSLL library is used to provide transport layer security on both Linux
and Windows. This paper describes how to take OpenSSL to use for Visual Studio 2019 development. 
The resulting build will be part of eosal repository, do redoing this is necessary only when 
taking new OpenSSL version to use or changing OpenSSL configuration used with eosal. 

Download OpenSSL source archive
********************************

* The OpenSSL sources can be found at https://www.openssl.org/source/. I downloaded the 
  openssl-1.1.1c.tar.gz (prevalent version at 9.9.2019). Something like 7-Zip is needed 
  to extract files from the .gz archive.
* Extract OpenSSL sources into a specific path
* Extract files from .gz archive into c:\coderoot\openssl folder. Rename openssl directory with version number as plain “openssl” so include paths, etc, work without modification. * Resulting file structure should look as in picture below:

.. figure:: pics/openssl-folders.png

   to get idea of directory tree.


Install Perl and NASM
**********************
Download and install Perl. I installed “Strawberry Perl 5.30.0.1 (64bit)” from http://strawberryperl.com/.

NASM can be downloaded from https://www.nasm.us/, I used 	version 2.14.02 with binary installer https://www.nasm.us/pub/nasm/releasebuilds/2.14.02/win64/nasm-2.14.02-installer-x64.exe.

Add C:\Program Files\NASM to PATH.

Configure OpenSSL 
******************
For now I have been building with default options, would be better to select only what is needed. 
Building OpenSSL 
Read the openssl/INSTALL note containing some specific build information common to both the 64 and 32 bit versions. Here we build 64 bit AMD architecture for Windows:

* launch a “x64 Native Tool Command Prompt for VS 2019”
* cd \coderoot\openssl
* nmake clean
* perl Configure -MT -Z7 threads no-deprecated no-shared VC-WIN64A
* perl Configure -MTd -Z7 threads no-deprecated no-shared debug-VC-WIN64A (This could be used to build debug libraries, but those are not used, at least for now)
* nmake

Copy OpenSSL libraries to use
******************************
Copy libssl.lib and libcrypto.lib from c:\coderoot\openssl directory to c:\coderoot\eosal\libraries\win64_vs. This will move these two libraries to be part of eosal repository, so steps in this paper are not needed in every computer using Visual Studio 2019. 

Copy also the “openssl” include directory from c:\coderoot\openssl\include to c:\coderoot\eosal\libraries\include. This will move necessary headers to be part of eosal repository. 
Do the same for the 32 bit build

* launch a “x86 Native Tool Command Prompt for VS 2019”
* cd \coderoot\openssl
* nmake clean
* perl Configure -MT -Z7 threads no-deprecated no-shared VC-WIN32
* nmake

Copy libssl.lib and libcrypto.lib from c:\coderoot\openssl directory to c:\coderoot\eosal\libraries\win32_vs. Headers were already copied with 64 bit build.


200111, updated 22.5.2020/pekka
