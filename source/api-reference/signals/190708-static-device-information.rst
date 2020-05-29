Publish static IO device information
=======================================
There are scenarios, where hard coding memory block map in both ends is not the best thing. 
For example if we wish user interface to display the state of any unknown IO board, or to use 
names for inputs/outputs, etc, instead of addresses within memory block. This map for example 
state that “HEATER_POWER” is a 4 byte floating point number at address 121 within memory block “OUT”. 

Here, we work at low abstraction level, thus we do not yet define what this information is nor 
how it is formatted. For now it is enough that this is “const static char mydevice_info[] = x…. ”, 
basically statically allocated block of memory  whose contents do not change, or at least changes
are not synchronized. 
Recommendation is that device information would start with the name of the device information 
format as ‘\n’ terminated string. This could be used by controller to check what format the device
information is.

To set up a static memory block, we allocate space for it statically. For micro-controllers 
it is beneficial to use “const static” C declaration, so that no RAM copy of the static information
 is needed, it can be used directly from flash. Publish device information using ioboard.c API

If the we use ioboard.c API to set up the, we store device information pointer and size in parameter
structure before calling ioboard_start_communication:

::

    const static os_uchar mydevice_info[]
     = "di-1\n"
       "HEAT_POWER,OUT,121,float\n"
       "TEMPERATURE,IN,18,float\n";

    ioboardParams prm;
    os_memclear(&prm, sizeof(prm));
    ...
    prm.device_info = mydevice_info;
    prm.device_info_sz = sizeof(mydevice_info);

Here is some very basic device information as text example. Since plain text takes kind of a lot
of space, this is not optimal way to present device information. It is beneficial to compress device 
information as much as possible, since it is usually transferred every time when a connection is established.

The static memory pool needs to be slightly larger, by sizeof(iocMemoryBlock) + IOBOARD_MAX_CONNECTIONS * sizeof(iocSourceBuffer) bytes.
We need space for memory block structure and source buffer structure for each connection.  We do not need 
to allocate memory for memory block data nor for source buffer content, since this will be IOC_STATIC memory block. 

::

    static os_uchar
        ioboard_pool[IOBOARD_POOL_SIZE(IOBOARD_CTRL_CON,
            IOBOARD_MAX_CONNECTIONS,
            IOBOARD_TC_BLOCK_SZ, IOBOARD_FC_BLOCK_SZ)
            + sizeof(iocMemoryBlock)
            + IOBOARD_MAX_CONNECTIONS * sizeof(iocSourceBuffer)];

Or call basic IOCOM functions to set up

When a device information memory block is allocated, it needs pointer to and size of static data.
Flag IOC_STATIC will tell that the buffer is static and source synchronization buffering is not needed.

::

    os_memclear(&blockprm, sizeof(blockprm));
    blockprm.mblk_nr = IOC_DEVICE_INFO_MBLK;
    blockprm.mblk_name = "INFO";
    blockprm.buf = (os_uchar*)prm->device_info;
    blockprm.nbytes = prm->device_info_sz;
    blockprm.flags = IOC_SOURCE|IOC_STATIC;
    ioc_initialize_memory_block(OS_NULL, &ioboard_communication, &blockprm);

Dynamic access to device information in control computer
This is code in control computer’s end. A “root callback” function can be set at startup. 
It gets called when a new memory block is created (among other things). 

::
    osal_socket_initialize();
    ioc_initialize_root(&root);
    ioc_set_root_callback(&root, root_callback, OS_NULL);

Creating dynamic memory blocks is be enabled by IOC_DYNAMIC_MBLKS flag in either ioc_listen() or ioc_connect() call. 

::

    ep = ioc_initialize_end_point(OS_NULL, &root);
    os_memclear(&epprm, sizeof(epprm));
    epprm.flags = IOC_SOCKET|IOC_CREATE_THREAD|IOC_DYNAMIC_MBLKS;
    ioc_listen(ep, &epprm);

Thus application’s root_callback() function gets called with IOC_NEW_DYNAMIC_MBLK event, once 
connection is established and memory block information is received from the IO device.

::

    static void root_callback(
        struct iocRoot *root,
        struct iocConnection *con,
        struct iocMemoryBlock *mblk,
        iocRootCallbackEvent event,
        void *context)
    {
        os_char text[128], mblk_name[IOC_NAME_SZ];
        switch (event)
        {
            case IOC_NEW_DYNAMIC_MBLK:
                ioc_get_memory_block_param(mblk, IOC_MBLK_NAME,
			    mblk_name, sizeof(mblk_name));
                os_strncpy(text, "Memory block ", sizeof(text));
                os_strncat(text, mblk_name, sizeof(text));
                os_strncat(text, " dynamically allocated\n", sizeof(text));
                osal_console_write(text);
                if (!os_strcmp(mblk_name, "INFO"))
                {
                    ioc_add_callback(mblk, info_callback, OS_NULL);
                    ioc_set_flag(mblk, IOC_AUTO_RECEIVE, OS_TRUE);
                }
                break;
            default:
                break;
        }
    }

This function again checks if this is device information memory block “INFO”. 
Info callback will be called when device information data has been transferred. 
This example just prints the device information (string assumed).

::

    static void info_callback(
        struct iocMemoryBlock *mblk,
        int start_addr,
        int end_addr,
        os_ushort flags,
        void *context)
    {
        os_char buf[128];
        if (end_addr >= 0)
        {
            ioc_getstring(mblk, 0, buf, sizeof(buf));
            osal_console_write(buf);
            osal_console_write("\n");
        }
    }


190708, updated 22.5.2020/pekka
