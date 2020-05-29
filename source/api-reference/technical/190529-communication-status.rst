Communication status
======================
THIS TEXT IS OBSOLETED -> REWRITE NEEDED

Communication status refers to general communication information and settings. For example number
of connections (sockets, etc) connected to a memory block. In future this could indicate which input
data is selected in redundant communication, etc. Communication status may include also settings.

From application's view communication status appears the same as data memory and is accessed using 
the same ioc_read(), ioc_get16(), ioc_write(), ioc_set16(), etc. functions. For data memory, 
the address is positive or zero, status memory addresses are negative.

Number of connected streams at this moment. 
How many times socket connection has been dropped.

For example IOC_NRO_CONNECTED_STREAMS is defined as address -2 and has two bytes, thus less significant 
byte is is in address -2 and more significant byte in address -1.

Example, print communication status without callbacks
The example function below: Every time a socket connects or disconnects to this "IO board", this function
prints number of connected sockets and how many times a socket has been dropped (global count).

::

    static int
        prev_nro_connections,
        prev_drop_count;

    static void ioboard_show_communication_status(void)
    {
        int
            nro_connections,
            drop_count;
        char
            nbuf[32];
        nro_connections = ioc_get16(&ioboard_fc, IOC_NRO_CONNECTED_STREAMS);
        drop_count = ioc_get32(&ioboard_fc, IOC_CONNECTION_DROP_COUNT);
        if (nro_connections != prev_nro_connections ||
            drop_count != prev_drop_count)
        {
            osal_console_write("nro connections = ");
            osal_int_to_string(nbuf, sizeof(nbuf), nro_connections);
            osal_console_write(nbuf);
            osal_console_write(", drop count = ");
            osal_int_to_string(nbuf, sizeof(nbuf), drop_count);
            osal_console_write(nbuf);
            osal_console_write("\n");
            prev_nro_connections = nro_connections;
            prev_drop_count = drop_count;
        }
    }

Example, get communication status from callback
The iocontroller_callback() example function is called when changed data is received from connection or when connection status changes. Here we are interested in case when connections status, number of connected sockets, etc. or connection drop count changes. We check by callback start and end addressed if either of the two has changed. 

::

    static volatile int
        nro_connections,
        drop_count;

    static void iocontroller_callback(
        struct iocMemoryBlock *mblk,
        int start_addr,
        int end_addr,
        os_ushort flags,
        void *context)
    {
        if (end_addr >= IOC_NRO_CONNECTED_STREAMS && start_addr < IOC_NRO_CONNECTED_STREAMS + 2)
        {
            nro_connections = ioc_get16(mblk, IOC_NRO_CONNECTED_STREAMS);
        }
        if (end_addr >= IOC_CONNECTION_DROP_COUNT && start_addr < IOC_CONNECTION_DROP_COUNT + 4)
        {
            drop_count = ioc_get32(mblk, IOC_CONNECTION_DROP_COUNT);
        }
    }

No heavy processing or printing data should be placed in callback. The callback should return quickly, the communication must be able to process all data it receives. Thus here we just copy the status into global variables.


190529, updated 29.5.2019/pekka
