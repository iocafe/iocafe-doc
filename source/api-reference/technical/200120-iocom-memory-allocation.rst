IOCOM memory allocation
==========================
Heap based dynamic memory allocation is used by default in systems that support it. Anyhow using heap 
based memory allocation is not recommended for micro controllers or other systems with limited memory 
resources, even if it was supported. I recommend to set up dedicated memory pool in micro controller 
environments. It can be set up also for linux/windows, which is useful when developing code to run
in micro controllers in PC environment.

The OSAL_DYNAMIC_MEMORY_ALLOCATION define (OS abstraction layer, eosal library) is set depending if 
dynamic memory allocation is supported for the system, either 1 or 0. This usually follows system 
specific defaults, but this can be overridden by setting the compiler define.

The IOCOM library has it’s own functions for memory allocation, ioc_malloc() and ioc_free().
By default these map directly to corresponding eosal library memory allocation functions.

The trick is here: A static memory buffer can be as memory pool for the ioal library. 
Function ioc_set_memory_pool() stores buffer pointer within the iocRoot structure.
If pool is set:

* The pool is dedicated to one iocRoot structure.
* All memory needed by the iocom library is allocated from the pool.
* No dynamic memory allocation is used.
* Necessary pool size can be calculated precisely from maximum number of connections, 
  number of registers, etc, communication detail.
* No fragmentation can occur, blocks are never split.

200120, updated 23.5.2020/pekka

