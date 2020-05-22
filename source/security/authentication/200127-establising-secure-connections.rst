Establishing secure connection
===============================
Secure connections means a connection which cannot be listened in and identity of both ends is
verified. This needs to be kept as simple and clear as possible, complexity enables human error
which is most common cause for security breach. Basically we want to:

* Setting up secure transport (key exchange, AES)
* Verify control computer identity (server certificate) . 
* Present IO device identity (device name, number and password).

When an IO device is set up, it must be loaded with information:

* Which certificate authority is trusted (public key of the certificate authority). Only a server
  presenting certificate signed by the trusted certificate authority can be connected to.
* Name of the IO device can be stored here, but it is practical to hard code this in IO device software. 
  This should be a shortish name, which describes the device and it’s maker.  
* Device number (up to 64 bits). Usually there are many same kind of devices in the network 
  and it is necessary to identify these. is nice to enumerate, lets say my house thermostats,
  like 1, 2, 3… If nothing better is available, MAC address of one of IO devices network interfaces
  can be used. These are or should always by unique. The device name and number must form unique
  identification within the domain of the control computer. 
* Password together with device name/number is used to prove the identity of the IO board to
  the control computer.
* Network setup. The IP, subnet, gateway and DNS of IO board. Or information to get these using DHCP.
  If there are multiple network adapters, then these for each adapter. In rare case we may also need
  to specify which network adapter is used for the outgoing connection. 
* Control computer URL or IP and “same segment” check box. The control computer’s IP address can be
  stored as plain number, like “192.168.1.229”, or if “same segment” is selected, then like “X.X.X.254”,
  which would mean three first numbers same as in the board’s IP, but end with 254. If this IO device
  is to connect to a second control computer (for redundancy) same information is needed. 

Procedure for establishing a connection

* The IO device initiates TCP socket connection to server port 443, using TLS. Both ends generate
  random keys for encrypting the communication. These keys are exchanged using Diffie-Hellman,
  or other key exchange. After this communication moves to symmetric AES256, etc, encryption.
  At this point we have a secure transport, but we cannot be sure who is there in the other end.
* Server presents it’s server certificate. When IO device receives this, it checks if it is
  signed by certificate authority specifies in IO device setup. If so, all fine. If not, “Intruder”
  alarm blinks and whistles on the device and the connection is dropped. 
* IO device sends it’s name, number and password. If these match to the control computer’s (server)
  database of “good IO devices”, we are fine and can proceed with actual communication. 
  If not, then “Intruder alarm!” goes off in server end.

It is very possible to make a configuration error, and get stuck with configuration which makes 
the IO device completely inaccessible. For that we need a physical “reset to defaults” jumper, etc.
on the IO device. This restores the information above to such defaults that it is possible to 
regain access to the board and start over.

Certificate authority here can be the IO device manufacturer, the owner of IO device, etc. 
Chain of trust can also be longer, like server certificate signed by corporate division, signed
by corporate head quarter. In this IO board could be loaded with public key of the head quarters.
The certificate authority doesn’t need to have anything to do with Verisign, Thawte or Comodo.
One could use also those to as base domain of trust. But these cost and provide weaker security
since these are widely used and they end up authorizing people who should not be authorized,
and these authorization rights occasionally get stolen. 

Security breaches: 

* If a board is stolen, it is possible that it’s name, number and password are used by a hacker
  who pretends to be that specific IO board and sends misleading input information. 
  If possible breach of board password is detected, the real board’s password must be changed. 
* It is recommendable to black list device name, number, password combinations of devices which
  have been removed from use. Black list can be simply stored in control computer which checks
  it out when a connection is established, and sounds possible “Intruder alert”.
* If control computer (server) credentials are stolen (it’s private key or there is break above
  it in security chain), all IO device passwords must be changed once server has been fixed.
  Might be big work, take care of server. Our server certificates never expire and are never
  automatically renewed. Last thing we want is that our robot, machine or whatever system does
  down on it’s own if there is some problem in certificate renewal chain. That would be as bad
  or worse than a hacker.

200127, updated 20.5.2020/pekka
