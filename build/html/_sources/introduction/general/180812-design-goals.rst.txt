Design goals
============
The IOCOM library connects IO devices to control computer in a heterogeneous network environment. The same library code run on computers and micro controllers. 

* Connect distributed IO devices to controller
* Make it simple, reliable and fast.
* Open source, free to use also as part of commercial code.
* No compromises. Only one simple way to the specific job. 
* Reference implementation and documented protocol to connect the devices.
* Code can be ported to any system with standard C compiler and sufficient resources.  
* Data is seen as memory blocks. Memory blocks are plain byte arrays.
* Memory blocks are transferred to one direction only. 
* Application never locks up. If communication doesn’t keep up with changes by application, frames are dropped.
* Preserve order of changes and data transfer synchronization, so that application will know that whole memory block is up to date. 
* Low small transaction latency. Typical goal is around 1 milliseconds, depending on hardware.
* High through output in bigger transactions.
* Large address space size. Typically 32k bytes for 8/16 bit micro-controllers and 2GB for 32/64 bit systems. Practical maximum memory block size is set by available system memory.
* Point to point connection over TCP socket or full duplex serial communication.
* Low overhead. Small message headers, transfer only changes, compress transferred data.
* Secure TLS connections and device identification when needed.
* RTOS multi-threading support, when needed.
      
180812, updated 20.5.2020/pekka

