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

The "pflag" value is stored in signal flags. If IOC_PFLAG_IS_PRM is set: The offset
address within parameters structure (and within presistant block, if applicable) is 
stored in signal structure's "ptr" member (by signals_to_c.py)

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



200615, updated 15.6.2020/pekka

