Thread safety
==============
If multi-threading is enabled for the library, the function calls are reentrant and 
can be called from different threads. 

The define below controls weather multi-threading support is enabled , set either 1 or 0. 
There are system specific defaults for this, but this can be overridden by setting compiler define.

::

    #define OSAL_MULTITHREAD_SUPPORT 1


200303, updated 18.5.2019/pekka
