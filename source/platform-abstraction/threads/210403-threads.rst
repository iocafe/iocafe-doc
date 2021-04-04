threads
==================================

The eosal thread API is wrapper for operating system threads. It provides unified interface to OS thread functions.

create and terminate attached thread
********************************************

Attached threads are bound to parent thread which start then, and must exit and be joined to parent thread before it can exit.

.. figure:: pics/210403-attached-thread.png

::

    osalThread *osal_thread_create(
        osal_thread_func *func,
        void *prm,
        osalThreadOptParams *opt,
        os_int flags);

    void osal_thread_join(
        osalThread *handle);

- :doc:`210404-attached-thread-code-example`

create and terminate detached thread
*******************************************

Detached threads run completely independently from thread that started them, and do continue executing after parent thread has exited. 
The parent thread is not responsible for any clean up tasks.

.. figure:: pics/210403-detached-thread.png

- :doc:`210404-detached-thread-code-example`

terminating threads when process exits
****************************************

Terminating individual threads is not part of this API, but on application's responsibility. Typically request_exist, thread_event, etc.
are passed in application's parameter structure to the new thread. 

Process exit is triggered by osal_request_exit() function. 
The thread events which may be waited for and prevent the process from existing need to be flagged as osal_event_create(OSAL_EVENT_SET_AT_EXIT),
which adds those event to list of events to set when the osal_request_exit() is called.

thread priority
******************

The osal_thread_set_priority() function sets current thread priority. Thread priority will set how operating system scheduler should share
time between threads. Most systems we work on implement preemptive multitasking. This mean that threads running on higher priority get 
processor time first, and only when no higher priority thread needs time then the lower priority threads will receive it. 
Multiprocessor or multi-core environment will change this, since one processor or core can be executing only one task.

:: 

    osalStatus osal_thread_set_priority(
        osalThreadPriority priority);


The OSAL_THREAD_PRIORITY_LOW, OSAL_THREAD_PRIORITY_NORMAL and OSAL_THREAD_PRIORITY_HIGH are used to prioritize execution of normal 
threads. The OSAL_THREAD_PRIORITY_TIME_CRITICAL is reserved for real time tasks only, and using this priority will put special 
requirements on the thread.

Linux specific note: Linux thread scheduler does amazingly good job without application specific thread priority settings, so these are not supported for nor.
While it is possible to use real time scheduling and set priorities and we may add support for this in esoal, I have found this often counterproductive. 
It requires serious knowledge and effort to get better performance than what linux scheduler provides easily.

sleep functions
******************

The os_timeslice() function sleep for very short time, like one time slice. Real sleep period is operating system/HW dependent, in order of 1 ms.
To sleep for a specific period, use either os_sleep(milliseconds) or  os_microsleep(microseconds).  
The sleep function precision varies by operating system, and may be longer if high priority threads eat the processor time. 

:: 

    void os_timeslice(void);

    void os_sleep(
        os_long time_ms);

    void os_microsleep(
        os_long time_us);


