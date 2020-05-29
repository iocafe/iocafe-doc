Integer serialization
=========================
Serialization format is packed  by integer value. Packed format is type and architecture 
independent, number of  bytes generated depends only on value. Maximum integer size is 64 bits.

Bits in first byte byte NNNNSxxx:
    • NNNN number of follower bytes.
    • S sign, 0 for positive, 1 for negative.
    • xxx Least significant 3 data bits.

Follower bytes hold the remaining data bits.

Required defines:

::
    OSAL_SERIALIZE_SUPPORT = 1

Serializing an integer 
*****************************************************
The osal_intser_writer() function packs C integer x into buffer using serialization format.
The function returns number of bytes written to the buffer.

::

    os_int osal_intser_writer(
        os_char *buf,
        os_long x);

Reading an integer
*****************************************************
The osal_intser_reader() function converts integer from serialization format into 64 bit C integer x.
The function returns number of bytes read from the buffer.

::

    os_int osal_intser_reader(
        os_char *buf,
        os_long *x);

191026, 23.5.2020/pekka

