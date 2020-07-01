Handling signal changes in communcation callback
=================================================
When an IO device received signal (change) trough communication it is often necessary
to reach to it. If signal is connected to HW digital output, we need to to call forward 
the signal change to pins library. If signal is parameter setting, we need to modify 
parameter structure in memory and trigger saving parameters to persistent storage.
In addition we may have application specific signals to react to. These are handled
by switch() in communication callback.

TO DO
********

- START BY WRITING DOC "HANDLING SIGNAL CHANGES IN COMMUNICATION CALLBACK"
- Write script parameters_to_signals.py to convert parameter JSON to signal JSON which
  can be merger with other signals.
- MODIFY signals_to_c.py so that signals are sorted by address (enables quick searching), Ok
- MODIFY signals_to_c.py so that flags for parameters (much like IOC_PFLAG_IS_PRM are set), ok
- Set iocMblkSignalHdr signal header pointers in memory block.
- Write script parameters_to_c.py Which generates parmeter structure for both persistent
  and volatile structures.

Memory block and signal information 
************************************
Memory structure includes pointer to signal header structure struct iocMblkSignalHdr *signal_hdr.
If memory block's signal information is linked to C code, the signal_hdr pointer can
be used to get get pointer of memory block's signal list (iocSignal). The signal list is sorted
by address to allow fast searching.

If a signal is created from parameter JSON, it will have the "pflag" attribute. The
"pflag" value is bit values combined by OR operator: 

- IOC_PFLAG_IS_PRM (64): Set for all parameters.
- IOC_PFLAG_IS_PERSISTENT (128): This is persistent parameter.

The "pflag" value is stored in signal flags (along with type and IOC_PIN_PTR bit). 
If IOC_PFLAG_IS_PRM is set: The offset address within parameters structure (and within
presistant block, if applicable) is stored in signal structure's "ptr" member (by signals_to_c.py)


Set up for IO board (using ioc_ioboard)
****************************************
Pointers to memory blocks's signal headers need to be set. This allows communication
callback to access signal information by memory block handle.

::

    /* Set up parameters for the IO board.
     */
    os_memclear(&prm, sizeof(prm));
  
    prm.exp_signal_hdr = &candy.exp.hdr;
    prm.imp_signal_hdr = &candy.imp.hdr;
    prm.conf_exp_signal_hdr = &candy.conf_exp.hdr;
    prm.conf_imp_signal_hdr = &candy.conf_imp.hdr;

    /* Start communication.
     */
    ioboard_start_communication(&prm);

 
Communication callback function
********************************
callbak functionality

find index of signal which is touched by start addr
find last signal which is touched by start addr

loop though touched signals
   if pins pointer
   if set parameter flag
   handle custom stuff with switch ()

then callback has directly signal header pointer.
signal header is directly followed by IO signal data (iocSignal)

::

    void ioboard_communication_callback(
        struct iocHandle *handle,
        os_int start_addr,
        os_int end_addr,
        os_ushort flags,
        void *context)
    {
        const iocSignal *sig;
        os_int n_signals;

        /* If this memory block is not written by communication, no need to do anything.
        */
        if ((handle->flags & IOC_MBLK_DOWN) == 0 ||
            (flags & IOC_MBLK_CALLBACK_RECEIVE) == 0)
        {
            return;
        }

        sig = ioc_get_signal_range(handle, start_addr, end_addr, &n_signals);
        while (n_signals-- > 0)
        {
            if (sig->flags & IOC_PIN_PTR) {
                forward_signal_change_to_io_pin(sig, 0);
            }
            else if (sig->flags & IOC_PFLAG_IS_PERSISTENT) {
                changed ioc_set_parameter_by_sig(sig);
            }
            sig++;
        }
    }



200615, updated 15.6.2020/pekka

