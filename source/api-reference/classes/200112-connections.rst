Connections
============
A connection object represents logical connection between two devices. Both ends of communication have a connection object dedicated for that link, serialized data is transferred from a connection object to another.

::

    ioc_initialize_connection(iocConnection *con, iocRoot *root);
    ioc_release_connection(iocConnection *con);
    ioc_attach_memory_block(iocConnection *con, iocMemoryBlock *mblk, iocTransferBuffer *tbuf);
    ioc_detach_memory_block(iocConnection *con, iocMemoryBlock *mblk);

    ioc_connect(iocConnection *con, char *constr, flags)
    ioc_listen(iocConnection *con, flags)
    ioc_accept(iocConnection *con, flags)
    ioc_disconnect(iocConnection *con)
    ioc_run(iocConnection *con);

200112, updated 22.5.2020/pekka

