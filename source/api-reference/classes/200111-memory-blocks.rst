Memory blocks
======================
A memory block is byte array which is copied from a device to another. 

A memory block in device A is copied to identical memory block in device B. If the memory blocks have different size, the extra data is ignored.

Communication between two devices typically involves several memory blocks, for example IO board blocks could be "binary inputs", "analog inputs" and "binary outputs".  When deciding how to divide data into memory blocks, consider also synchronization and how often input data changes. If some data changes quickly and some is static, these should be in different groups. Data which need to be synchronized (transferred in specific order), like "my data array" and "data array ready" bit, need to be in the same memory block.

Initializing and deleting memory blocks
*****************************************
A memory block is presented by iocMemoryBlock structure. The structure stores pointers to buffers, flags and state information.  A memory block is initialized or allocated by:
 
osalStatus ioc_initialize_memory_block(
    iocHandle *handle,
    iocMemoryBlock *static_mblk,
    iocRoot *root,
    iocMemoryBlockParams *prm);

The root object is root of iocom instance’s object hierarchy. It kind of binds memory blocks, connections and source/target buffers together. The root object must be initialized before it can be used, and releasing root object will free all resources allocated within the root. 

If static_mblk argument is OS_NULL, the function allocates memory for the memory block structure. Otherwise it initializes a memory block in application allocated memory block structure. Memory block data size is nbytes. 

Structure iocMemoryBlockParams contains parameters for creating the new memory block, and can be discarded immediately after ioc_initialize_memory_block() function call.  The parameter structure should be cleared and only parameters needed set.

    iocMemoryBlockParams prm;
    os_memclear(prm, sizeof(prm));
    … set parameters for the memory block
    ioc_initialize_memory_block(…, prm);

Once a memory block is no longer needed, it should be released by calling ioc_release_memory_block function. If the memory block and/or the buffer were allocated by ioc_initialize_memory_block function, the allocated memory is released by this function. 

void ioc_release_memory_block(
    iocHandle *handle);


Memory block parameters
*****************************************
Memory block parameters are enumerated:
typedef enum
{
   IOC_NETWORK_NAME = 1,
   IOC_DEVICE_NAME = 2,
   IOC_DEVICE_NR = 3,
   IOC_MBLK_NR = 4,
   IOC_MBLK_NAME = 5,
   IOC_MBLK_AUTO_SYNC_FLAG = 6
}
iocMemoryBlockParamIx;

Set memory block parameter.
void ioc_memory_block_set_int_param(
    iocHandle *handle,
    iocMemoryBlockParamIx param_ix,
    os_int value);

Get memory block parameter value as integer.

os_int ioc_memory_block_get_int_param(
    iocHandle *handle,
    iocMemoryBlockParamIx param_ix);

Get memory block parameter as string.

void ioc_memory_block_get_string_param(
    iocHandle *handle,
    iocMemoryBlockParamIx param_ix,
    os_char *buf,
    os_memsz buf_sz);

Signal structure
*****************************************
X…

Accessing memory block content using signals
*********************************************
Signal abstraction of joins connected and error information with data. This is necessary in practise 

Low level memory block content access
*****************************************
X

Synchronizing data transfers
*****************************************

Send data synchronously:

void ioc_send(
    iocHandle *handle);

Receive data synchronously:

void ioc_receive(
    iocHandle *handle);
Handling callbacks related to memory block
Add a callback function:

void ioc_add_callback(
    iocHandle *handle,
    ioc_callback func,
    void *context);

Function to check if callback relates to specific signal:

os_boolean ioc_is_my_address(
    iocSignal *signal,
    int start_addr,
    int end_addr);

200111, updated 19.5.2020/pekka
