Streaming data
==============
The IOCOM uses flat memory blocks as basic abstraction, not streams. Still, configuration data,
flash program updloads and video from camera are logically stream type transfers. Ring buffer
setup using IOCOM signals is used to transfer streamed data.

THIS PAGE IS OUTDATED, REVIEW!

Signals for camera stream transfer from IO device (<- IO device)
::

    exp

      cam_state
      cam_buf
      cam_head

    imp

      cam_cmd
      cam_select
      cam_tail
  

Signals for configuration file transfer to IO device (-> IO device, tod = to device)
Used also for IO device flash program transfer .
::

    conf_exp

      tod_state
      tod_tail

    conf_imp

      tod_cmd
      tod_select
      tod_buf

      tod_head


Signals for configuration file transfer from IO device (<- IO device, frd = from device)
::

    conf_exp

      frd_state
      frd_buf
      frd_head

    conf_imp

      frd_cmd
      frd_select
      frd_tail


** Transfer from the IO device
controller: 
Controller can be in IOC_STREAM_IDLE or IOC_STREAM_RUNNING command (in "cmd")
::

    If "cmd" is IOSTREAM_IDLE:
        If state is IOSTREAM_RUNNING, do nothing. We are waiting for IO device to finish.
        Otherwise if controller wants to start the transfer, set "select" to ? (app specific), "tail" = 0 and "cmd" = IOSTREAM_RUNNING

    if "cmd" is IOSTREAM_RUNNING:
        if "state" is IOC_STREAM_RUNNING or IOSTREAM_COMPLETED: Read all available data and move "tail".
        If "state" is IOSTREAM_COMPLETED, the transfer has successfully finished. Set "cmd" = IOSTREAM_IDLE.
        If controller wants to interrupt the transfer, set "cmd" = IOSTREAM_IDLE.

IO device: 
Stream transfer has three states, IOC_STREAM_IDLE, IOC_STREAM_RUNNING and IOSTREAM_COMPLETED. This is in "state" signal.
::

    If in IOC_STREAM_COMPLETED state:
        If "cmd" is not IOC_STREAM_RUNNING, set "state" = IOSTREAM_IDLE.

    If in IOC_STREAM_IDLE state:
        If "cmd" is not IOC_STREAM_RUNNING, do nothing
        Otherwise set "head" to zero and switch to "state" = IOC_STREAM_RUNNING

    If in IOC_STREAM_RUNNING state:
        If "cmd" is no longer IOSTREAM_RUNNING, interrupt the transfer and switch to "state" = IOC_STREAM_IDLE. Same if "cmd" signal is disconnected?
        Otherwise move as much data as we can to head position. Limits are how much data we have and how much space there is for data between tail and head.
        Update head position. "select" can choose what to transfer. If all data has been transferred, switch to "state" = IOC_STREAM_COMPLETED.
 

** Transfer to the IO device

controller: 
Controller can be in IOC_STREAM_IDLE, IOC_STREAM_RUNNING or IOC_STREAM_COMPLETED command (in "cmd")
::

    If "cmd" is IOC_STREAM_IDLE:
        If "state" is not IOC_STREAM_IDLE, do nothing. We are waiting for IO device to finish.
        Otherwise if controller wants to start the transfer, set "select" to ? (app specific), "head" = 0 and "cmd" = IOSTREAM_RUNNING

    if "cmd" is IOC_STREAM_RUNNING:
        if "state" is IOC_STREAM_RUNNING: Write as much data to buffer as available and fits between tail and head and move head.
        If all data has been written and "state" is IOC_STREAM_RUNNING, set "cmd" to IOC_STREAM_COMPLETED.
        If controller want to interrupt the transfer, it sets "cmd" to IOC_STREAM_IDLE.

    Is cmd is IOC_STREAM_COMPLETED:
        If "state" is IOC_STREAM_IDLE, set cmd = IOC_STREAM_IDLE. All done.

IO device: 
Stream transfer has two states, IOC_STREAM_IDLE and IOC_STREAM_RUNNING. This is in "state" signal.
::

    If in IOC_STREAM_IDLE state:
        If "cmd" is IOC_STREAM_IDLE, do nothing
        Otherwise set "tail" to zero and switch to "state" = IOC_STREAM_RUNNING

    If in IOC_STREAM_RUNNING state:
        If "cmd" is IOC_STREAM_IDLE, the transfer has been interrupted: set "state" = IOC_STREAM_IDLE. Same if "cmd" signal is disconnected?
        Otherwise move as much data as we can from tail position, and update tail.
        if "cmd" is IOC_STREAM_COMPLETED, set "state" = IOC_STREAM_IDLE.

** Error handling note
This stream transfer doesn't include any error handling, like if writing to flash fails. Use separate signals for error handling or
embed error information into stream. This is to keep stream transfer as simple as possible.

"state"
::

    0 = IOC_STREAM_IDLE
    1 = IOC_STREAM_RUNNING
    2 = IOC_STREAM_COMPLETED


