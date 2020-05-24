Transfer buffers
==================
Transfer buffer binds a memory block and connection object together. It buffers changes to be sent through the connection.

::

    ioc_initialize_source_buffer(iocSourceBuffer *sbuf, con, mblk);
    ioc_remove_source_buffer(iocSourceBuffer *sbuf);


180727, updated 22.5.2020/pekka

