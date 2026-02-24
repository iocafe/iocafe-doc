MbedTLS 3.6.5 communication security
=======================================================

The MbedTLS library is used by eosal library for network communication 
Transport Layer Security, TLS. This enables secure communication
trough insecure network, like internet.   
The mbedTLS library is used for platforms like Linux, Windows 
and ESP32, which are powerful enough to support secured TCP communication.
It is not needed for light weight microcontrollers, wchich use
serial or unencrypted sockets for communication. 

Notice that OpenSLL has been tested with eosal, but implementation
is not currently (Feb 2026) up to date. 

.. toctree::
   :maxdepth: 2

   260222-mbedtls-3.6.5-linux
   260215-mbedtls-3.6.5-windows-visual-studio

260224, updated 24.2.2026/pekka
