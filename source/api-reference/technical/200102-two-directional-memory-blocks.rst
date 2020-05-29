Two directional memory blocks
===========================

* supporting bidirectional transfers on low level IO devices takes considerable amount of resources and is mostly 
  unnecessary. It also slows down the communication.
* use only for testers, configuration tools, etc. Do not use for normal IO communication, reliable recovery from 
  exception situations gets complicated. Stick with one directional memory blocks. 
* Both source and target buffers exist memory block in higher and in lower level.
* Source and target buffer tracks invalidated addresses at byte precision. This is done by using extra memory for mark bits.
* Invalidated data is transferred, again at byte precision.
* Received changes coming from lower hierarchy level are not echoed back down to same memory block. 


