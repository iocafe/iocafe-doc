Classes
==========
The IOCOM library is an object oriented design written in plain C. 

* The root object is starting point for communication object hierarchy. If it is deleted, 
  all resources allocated by the library are released. 
* Memory blocks store incoming or outdoing data. The application accesses data by reading and 
  writing these memory blocks. 
* A connection object represent connection from this process to another process. A connection 
  object corresponds to a socket, etc. 
* Transfer buffers are the bridge between the memory blocks and connection objects. These take care 
  about buffering what should be sent, what is being received and about synchronization.

.. toctree::
   :maxdepth: 2
   :caption: Contents:

   200110-iocom-root
   200111-memory-blocks
   200112-connections
   200113-transfer-buffers
   200114-object-interconnections
