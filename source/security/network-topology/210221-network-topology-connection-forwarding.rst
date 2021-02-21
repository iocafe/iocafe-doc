IOCOM secure network topology - take 2
========================================
The network topology of distributed system is closely related to security: 

* All data is TLS Data.
* Client (IO device, user interface, etc) identify a service by server certiticate. Trust on server can be explicitely configured: User interface may have multiple roots of trusts. Or trust can be establised automatically when an IO device connects to a server for very first time. 
* Client IO devices are identified by device ID and secret, or by user name and password.
* We need to keep IO device networks of separate people strictly apart. Even we would use the same cloud server 
  to access data from two IO device networks, data can newer be passed from one IO device network to another. 

.. figure:: pics/210221-connection-forwarding.png

   A switchbox process can be used to forward connections trough cloud server.

210221, updated 21.2.2021/pekka
