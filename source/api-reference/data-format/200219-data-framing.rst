Data framing
==============

DATA FORMAT HAS BEEN MODIFIED AFTER WRITING THIS, THIS NEEDS TO BE REVIEWED

.. list-table:: Serial frame
   :widths: 25 15 15 45
   :header-rows: 1

  * - name/address
    - bytes
    - type
    - use
  * - FRAME_NR/0
    - 1
    - uchar
    - Frame count. Every time new frame is sent, this number is incremented, value 0 if used to 
      indicate the first frame and skipped from that point on. Used to check for missing or duplicated 
      frames, first frame, etc. Value 255 is used as ACKNOWLEDGE/KEEP ALIVE. Also values greater 
      than 220 are reserved for control characters.
  * - CHECKSUM/1
    - 2
    - ushort
    - Checksum. 16 bit CRC. Used to verify frame integrity.
  * - FLAGS/3
    - 1
    - uchar
    - Flags: IOC_DELTA_ENCODED,  IOC_COMPRESESSED, IOC_ADDR_HAS_TWO_BYTES, IOC_ADDR_HAS_FOUR_BYTES,
      IOC_MBLK_HAS_TWO_BYTES, IOC_SYNC_COMPLETE
  * - BYTES/4
    - 1
    - uchar
    - Data size, bytes. Compressed or uncompressed. Data size zero indicates special message. 
      In this case start address is parameter for special message.
  * - MBLK/5
    - 1/2
    - uchar/ushort
    - Memory block identifier.
  * - ADDR/6-7
    - 1/2/4
    - uchar/ushort/uint
    - Start address within memory block. Or for special messages like keep alive or request to bind, 
      this is 32 bit value containing both special message and argument for it.
  * - DATA/7-11
    - ?
    - uchar[]
    - Data. Can be delta encoded and/or compressed.


.. list-table:: Socket frame
   :widths: 25 15 15 45
   :header-rows: 1

  * - name/address
    - bytes
    - type
    - use
  * - FLAGS/0
    - 1
    - uchar
    - Flags: IOC_DELTA_ENCODED,  IOC_COMPRESESSED, IOC_ADDR_HAS_TWO_BYTES, IOC_ADDR_HAS_FOUR_BYTES, 
      IOC_MBLK_HAS_TWO_BYTES, IOC_SYNC_COMPLETE.  Value 255 reserved for ACKNOWLEDGE/KEEP ALIVE.
  * - BYTES/1
    - 2
    - ushort
    - Data size, compressed or uncompressed, bytes. Data size zero indicates special message. 
      In this case start address is parameter for special message.
  * - MBLK/3
    - 1/2
    - uchar/ushort
    - Memory block identifier.
  * - ADDR/4-5
    - 1/2/4
    - uchar/ushort/uint
    - Start address within memory block. Or for special messages like keep alive or request to bind, 
      this is 32 bit value containing both special message and argument for it.
  * - DATA/5-9
    - ?
    - uchar[]
    - Data. Can be delta encoded and/or compressed.

All numeric data is transferred in small endian form, lest significant byte first and most significant byte last.

Maximum frame sizes for different types of communication are fixed.  For all serial communication maximum message 
frame size is always 96 bytes (IOC_SERIAL_FRAME_SZ), which includes headers. For sockets maximum message frame size 
is set 464  bytes (IOC_SOCKET_FRAME_SZ). Maximum frame sizes (especially for serial communication) are small to work 
with limited memory in micro-controllers. Notice that multiple message frames can be packed in single TCP frame.


200219, updated 27.5.2020/pekka
