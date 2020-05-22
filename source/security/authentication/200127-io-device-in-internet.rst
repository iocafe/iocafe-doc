IOT – IO devices in Internet
=============================
What if the the IO domain controller was on a cloud server, connected trough the Internet? Instead of
running within a self contained device, like a robot, or within a building: Our device could be 
distributed around the world.

* This is doable because an IO device can establish TLS connection to controlling computer, which
  in this case would be a server in cloud:
* The IOCOM library already transmits minimum about of data, and only when it changes.
* We are able to control maximum transfer frequency simply by calling ioc_send() synchronization
  as needed (This may be necessary to keep AWS, etc, billing in check).
* Security scheme based on security scheme, works in this setting. When IO board connects to
  the server, the secure channel is established by key exchange and AES. Server identifies itself
  to IO device by presenting server certificate, and IO board itself by IO board name and password.
  Fine graining to user rights doesn’t really concern IO devices, it is task of the cloud server application. 
* The same IO device configuration and firmware update methods work as do ones within a robot.
  Basically the firmware updates and IO device configuration is done trough the controlling computer.
  No other connection (potential security hazard) is allowed.

There are details which must be accounted for: 

* Outbound TLS from the IO device to the Internet needs to be open.
* The IO device needs to be able to use DNS to resolve IP address from name. 
* Keep alive message (to know that socket connection is broken) frequency must be set as needed. 

What this achieves is set of IO devices, physically located anywhere, connected to cloud server
application. The cloud server application would be The IOCOM library runs the communication layer
in both the IO device and in cloud server application. The real wold input data from IO devices
is moved to the cloud server immediately and efficiently, and the cloud server can control
the IO device outputs and functions. 

200127, updated 31.5.2019/pekka
