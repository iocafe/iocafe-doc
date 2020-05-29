Memory block information
===========================
Each memory block must inform all connected devices about it’s existence. This is needed so that memory
blocks in different devices can be bound, resized or dynamically created. Following information is 
maintained for a memory block.

|

.. list-table:: Socket frame
  :widths: 14 14 14 14 14 15 15
  :header-rows: 1

  * - device name
    - device number
    - memory block number
    - Memory block id
    - Memory block name
    - Memory block size/bytes
    - Flags (includes direction)
  * - gina
    - 1
    - 1
    - 1
    - exp
    - 128
    - 0

|

This information is static type. Let’s say, even if memory block is automatically resized,
the information here reflects original set values. 

When connection is established, memory block information is sent for every existing memory
block is sent to new connection. If new memory is added, block information is sent trough 
every connection.

I connection contains pointer to memory block, whose information is to be sent. When new connection
is established, this pointer is set to first memory block. When information about the pointed memory 
block has been sent trough the connection, pointer is moved to next memory block. When all are ready,
pointer becomes zero.

When new memory block if added, we set the pointer within connection object to point it (unless not 
null, which means that the connection is already working on transferring memory block data).

Thus control computer end is responsible of subscribing data it wants, and informing what data it
will send. This generates source and target buffer objects also in device end.

190624, updated 24.5.2020/pekka

