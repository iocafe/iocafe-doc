eosal initialization and shut down 
==========================================================


eosal initialization
********************************************

The eosal library must be initialized before use (There are a few low level functions, like os_strncpy(), etc which can be used without initialization).
What is done during initialization depends on platform: If we have dynamic memory allocation, multi-threading, TLS encryption,  etc. Platform configuration is 
selected with build time defines.

::
 
    void osal_initialize(
        os_int flags);


.. figure:: pics/210403-attached-thread.png

- :doc:`210404-attached-thread-code-example`



eosal shut down
********************************************



Terminate threads and clean up allocated resources. 

::

    void osal_shutdown(
        void);

Global lock os_lock() MUST NOT be on when this function is called: Worker threads may need os_lock()
to be able to terminate.


Define OSAL_PROCESS_CLEANUP_SUPPORT xxx


Terminating worker threads. thread_count, osal_wait_for_threads_to_exit(); 

Using osal_main entry point
********************************************


Creating platform specific main function using EOSAL_C_MAIN define.
If needed for the operating system, EOSAL_C_MAIN macro generates the actual C main() function.

::

    #include "eosal.h"
    #include "myheader.h"

    EOSAL_C_MAIN

::

    osalStatus osal_main(
        os_int argc,
        os_char *argv[])
        {
            ...
        }



How worker thread knows when to exit?
********************************************

Multithreading only:

osal_exit function calls osal_request_exit(void);


Multithreading, wait for event, create event with OSAL_EVENT_SET_AT_EXIT and check either osal_stop() and osal_go()


Quitting worker threads earlier
********************************************


atexit ?
********************************************
