eosal initialization and shut down
==========================================================
The eosal library needs to be initialized before use, and shut down to release allocated resources. It is possible to do this
void

Initialization
********************************************
The eosal library must be initialized before use (There are a few low level functions, like os_strncpy(), etc which can be used without initialization).
What is done during initialization depends on platform: Do we have dynamic memory allocation, multi-threading, TLS encryption,  etc. Platform support
is configured with build time defines, and limited to operating system/HW capability.

::

    void osal_initialize(
        os_int flags);

Warning: Many micro-controllers do not clear memory at soft reboot, so programmer cannot fully trust that all static variables are
filled with zeroes when the software starts. It is proper to clear these programmatically in some initialization function.


Shut down
********************************************
On Windows, Linux, Android, etc, terminating a process is orderly process to complete writes, terminate threads, and release allocated
resources. This is different from typical micro-controller, which is shut down by disconnecting the power, and even rebooted trough soft
reset without cleaning up all resources.

Define OSAL_PROCESS_CLEANUP_SUPPORT as 0 or 1 selects wether the graceful shut down code is included in eosal and related libraries.
By default this is 1 for Windows, Linux, etc and 0 for micro-controllers.

Call osal_shutdown() explicitly always when process exits, eben if compiling for a micro-controller.
If OSAL_PROCESS_CLEANUP_SUPPORT is zero, osal_shutdown is just empty macro and generates no code:

::

    void osal_shutdown(
        void);

If  OSAL_MULTITHREAD_SUPPORT is 1, the osal_shutdown terminates worker threads created by osal_thread_create()
and cleans up allocated resources. The function wait for worker threads to exit.
Global lock os_lock() MUST NOT be on when this function is called: Worker threads may need os_lock()
to be able to terminate.

If  OSAL_MULTITHREAD_SUPPORT is 0, the osal_shutdown releases allocated resources.

Transport support
********************************************
Support for sockets, TLS and serial port is not initialized by osal_initialize(). If used, it need to be initialized
separately, typically at beginning of osal_main(). Network interface card and WiFi configuration arguments are for
micro-controllers, and ignored in PC environment.

::

    osal_socket_initialize(nics->nic, nics->n_nics, wifis->wifi, wifis->n_wifi);
    osal_tls_initialize(nics->nic, nics->n_nics, wifis->wifi, wifis->n_wifi, security);

Do not call both osal_socket_initialize() and osal_tls_initialize(), The osal_tls_initialize also initializes sockets.

Similarly serial port code is initialized by:

::

    osal_serial_initialize();

The clean up for socket, TLS, serial, and bluetooth libraries is done by osal_shutdown() and must not be called by application.

Using osal_main entry point
********************************************
Program entry point varies by operating system and platform: in PC we have main, _wmain, etc, with different character encoding, etc.
In Arduino and we have setup() and loop(), and so on. If writing code to run on many platforms: Creating platform specific entry
point using EOSAL_C_MAIN macro which call's application's osal_main() may make sense. This way the application code is the same
regardless of the platform. Ther

Creating platform specific main function using EOSAL_C_MAIN define.
If needed for the operating system, EOSAL_C_MAIN macro generates the actual C main() function.

::

    #include "eosal.h"
    #include "myheader.h"

    EOSAL_C_MAIN

Then the application's entry point function will be as below:

::

    osalStatus osal_main(
        os_int argc,
        os_char *argv[])
        {
            ... application code, rval = ? ...
            return rval;
        }

In PC computer, the EOSAL_C_MAIN macro use to generate entry point is roughly same as code below.
In micro-controllers, code what the EOSAL_C_MAIN macro creates varies.

::

    int main(int argc,char *argv[])
    {
        osal_initialize(OSAL_INIT_DEFAULT);

        ... application code, rval = ? ...

        osal_shutdown();
        return rval;
    }

In micro-controller applications, we preserve the traditional setup/loop thinking. The osal_main()
function can be taught as setup() and osal_loop() as loop().
Application's loop function for micro-controller to run the main loop. In PC simulation, This
is called by osal_simulated_loop(), not by platform framework.

::

    osalStatus osal_loop(
        void *app_context);

Prototype for application defined cleanup function to release resources allocated by osal_main().
This is often just

::

    void osal_main_cleanup(
        void *app_context);

The osal_simulated_loop() function is used to create repeated osal_loop function calls in PC.
On micro-controller this is only saves context pointer.

::

    void osal_simulated_loop(
        void *app_context);


Code for single threaded micro-controller IO application could look like below, but written so that it can
be run as PC simulation.

::

    osalStatus osal_main(
        os_int argc,
        os_char *argv[])
    {
        ... application initialization code ....

        osal_simulated_loop(OS_NULL);
        return OSAL_SUCCESS;
    }

    osalStatus osal_loop(
        void *app_context)
    {
        ... application main loop code ...
        return OSAL_SUCCESS;
    }

    void osal_main_cleanup(
        void *app_context)
    {
        ... cleanup code, if we need any  ...
    }

How worker thread knows when to exit?
********************************************
Forget threads need to monitor global "exit request" flag using either osal_go() or osal_stop() macro.
Only difference of the two that osal_go returns TRUE if we need to keep on running, and osal_stop
returns OS_TRUE if exit has been requested, osal_go() == !osal_stop().

Worker threads often wait for an OS event, select, etc. While waiting, the thread cannot't check for
global "exit request" flag. This is solved so that events which are used this way are created
with OSAL_EVENT_SET_AT_EXIT flag. This flag causes osal_event_create to place the event into "at exit"
event list, which are set when process is exiting, so the threads regain control and can check
the "exit request" flag.

When thread is created by osal_thread_create, global thread count is incremented. When the
thread exists (and is joined, if it is attached thread), the thread count is decremented.

The osal_shutdown calls osal_request_exit() and osal_wait_for_threads_to_exit functions.
The osal_request_exit() function sets global exit_process flag and sets all thread events listed
in "at exit" event list so that threads can start shutting themselves down. Main thread which does
eosal, etc, clean up should wait until child thread count reaches zero before final clean up.

.. figure:: pics/210403-attached-thread.png


Quitting worker threads earlier
********************************************
When working with multiple libraries, it may be useful to first terminate all worker threads from all libraries.
Then, once we have only single thread of execution left, do the clean up. The code below would request worker
threads to stop and wait until these exit.

::

    osal_request_exit();
    osal_wait_for_threads_to_exit();
