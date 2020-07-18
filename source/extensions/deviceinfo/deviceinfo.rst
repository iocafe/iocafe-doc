deviceinfo - publish device information
========================================
deviceinfo is iocom extension library to publish device's network configuration and state, software versions,
used operating system and hardware, and resource/performance counters.

* Network configuration and state
* System specification
* Resource monitor


Resource monitor
*****************

Compile time EOSAL define OSAL_RESOURCE_MONITOR=1 enables EOSAL code to keep track of resource use.
See osal_resource_monitor.h. 

Merge default resource monitor signals into application's signal map. For example candy camera's
/coderoot/iocom/examples/candy/config/signals/merge.json file includes resource_monitor.json:

::

    {"merge": ["signals.json", "resource_monitor.json", "device_conf_signals_1k.json", "camera_flat24k_signals.json"]}


Allocate static structrue for resource monitor.

::
    
    static dinfoResMonState dinfo_rm;

Initialize resource monitor structure. This stores signal pointers to dinfo_rm structure. This can be called
during application initialization, from osal_main(), etc.

:: 

    dinfoResMonSignals rm_sigs;
    DINFO_SET_COMMON_RESOURCE_MONITOR_SIGNALS(rm_sigs, candy);
    dinfo_initialize_resource_monitor(&dinfo_rm, &rm_sigs);

Call dinfo_run_resource_monitor() from main loop. This keeps the resource monitor signals alive.  

::

    os_get_timer(&ti);
    dinfo_run_resource_monitor(&dinfo_rm, &ti);

|

.. list-table:: Resource monitor signals
  :widths: 25 75
  :header-rows: 1

  * - signal name
    - description
  * - rm_malloc
    - Operating system memory allocated by EOSAL, bytes. 
  * - rm_muse
    - How much of memory allocated through/by EOSAL is in use. The os_malloc() adds to used memory and os_free() releases it. 
  * - rm_threads
    - Number of threads created trough EOSAL.
  * - rm_events
    - Number of events created tough/by EOSAL.
  * - rm_mutexes
    - Number of mutexes created tough/by EOSAL.
  * - rm_sockets
    - Number of sockets currently allocated trough EOSAL, including connected sockets, listening sockets and UDP sockets.
  * - rm_connects
    - Number of TCP socket connects. This number is incremented at every connection and never decremented. Listening sockets
      and UDP sockets do not affect to this. 
  * - rm_txbytes
    - Number of bytes sent to a connected TCP socket
  * - rm_rxbytes
    - Number of bytes received from a connected TCP socket
  * - rm_aveloop
    - Average main loop period, ms. For example if main loop runs 1000 times per second, this is 1 ms.
  * - rm_maxloop
    - How long the slowest single round of main loop took within testing period.
  * - rm_boottime
    - How many minutes ago the software started (device booted). Useful to ensure that there are not
      unintended device reboots.

|


Build with deviceinfo library
********************************
Directory /coderoot/iocom/extensions/deviceinfo need to be in included path, then include deviceinfo header.

::

    #include "deviceinfo.h"

Link with deviceinfo deviceinfod library, these are under /coderoot/lib.