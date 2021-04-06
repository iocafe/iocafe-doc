Platform abstraction
================================
Operating system and hardware abstraction layer. 

The IOCOM library uses separate operating system abstraction layer, called eosal library. The eosal library wraps
operating system and hardware dependent functionality as function call interface. This interface is is similar for
all platforms: Purpose of the operating system abstraction is to separate platform dependent code from the bulk of
the library. Thus the IOCOM library can run on multiple platforms, like Windows, Linux and several micro-controllers, 
and be ported to new systems.

The eosal core functionality is general purpose functionality which we build on, and extensions are options which can be included.

Extensions can be turned on or off by complier defines. 
This provides the scalability: For example if we are to run serial communications on Arduino UNO board, we need to turn
almost all extensions and options off to save resources. Then in some other environment we may need security, handle 
network topology, etc, thus need these features.


.. toctree::
   :maxdepth: 1
   :caption: Core functionality

   initialization/210406-osal-initialization-and-shut-down
   threads/210403-threads

* platform configuration defines
* events
* thread synchronization
* timers
* string functions
* data types
* console
* debugging errors
* resource monitor
* error handling
* memory allocation

.. toctree::
   :maxdepth: 1
   :caption: Extensions

   serialization/191026-serialization-and-streams
   serialization/191026-integer-serialization
   serialization/191026-float-serialization
   serialization/191026-json-serialization
   persistent-data/200202-persistent-parameters
   programming-flash/flashcode

* network initialization 
* transport, security and streams
* network and device state
* persistent data
* file system
* date and time
* iocom as framework
* helper functions (math, checksum, etc)

