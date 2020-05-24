IOCOM root
==================

An iocRoot object is the root of communication object hierarchy, and must be initalized before using any other iocom functions. It holds first and last object pointers for memory block list, connection list, etc.

If the library is compiled to support multi-threading, the root object holds also the mutex to synchronize access to communication object hierarchy. 

An application can have multiple root objects to keep communication. This is useful when using multi-threading in client, to make sure that separate socket connects, etc work independently without delays caused by the other.

The ioc_initialize_root() function initializes the root object. The root object can always be allocated
global variable. In this case pointer to memory to be initialized is given as argument  and return value is the same pointer. If dynamic memory allocation is supported, and the root argument is OS_NULL, the root object is allocated by the function. 

::

    void ioc_initialize_root(
        iocRoot *root);

The ioc_release_root() function releases resources allocated for the root object. Memory allocated for the root object is freed, if it was allocated by ioc_initialize_root(). 

::

    void ioc_release_root(
        iocRoot *root);

Lock functions are are used to lock object hierarchy for the root so it can be accessed only by one thread at the time. Once the ioc_lock() is called by one threads, other threads are paused when they ioc_lock(), until the first thread calls ioc_unlock().

::

    void ioc_lock(
        iocRoot *root);

    void ioc_unlock(
        iocRoot *root);

If the multi-threading support is not enabled for the build, these lock/unlock functions are replaced by empty macros, which do nothing.

200110, updated 22.5.2020/pekka
