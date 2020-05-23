From simple comm to secure IoT
=======================================
At simplest IOCOM can be used as serial communication protocol between two devices. This doesn’t need to do with named register maps, network topology, security, etc. Then when we want to scale up to IoT with secured communication and connect trough shared server in cloud, we need to keep adding complexity.

Operating system abstraction layer capabilities can be turned on or off at eosal compile time by preprocessor defines. These include support for serial ports, TCP sockets, TLS, blue tooth, multi threading, persistent storage, etc. For example to build minimalistic support which can run on Arduino UNO, we would most likely to turn off everything but serial port.

The base iocom library is simple and small. The advanced features are implemented as extension libraries, which can be added as needed.

190527, updated 20.5.2020/pekka
