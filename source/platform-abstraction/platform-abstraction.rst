Platform abstraction
================================
Operating system and hardware abstraction layer. 

The IOCOM library uses separate operating system abstraction layer, called eosal library. The eosal library wraps
operating system and hardware dependent functionality as function call interface. This interface is is similar for
all platforms: Purpose of the operating system abstraction is to separate platform dependent code from the bulk of
the library. Thus the IOCOM library can run on multiple platforms, like Windows, Linux and several micro-controllers, 
and be ported to new systems.

.. toctree::
   :maxdepth: 2
   :caption: Contents:

   serialization/191026-integer-serialization
   serialization/191026-float-serialization
   serialization/191026-json-serialization
   serialization/191026-serialization-and-streams
