IO device configuration
=========================
IO device is configured trough IOCOM connection. The control computer provides user access
to configure the IO device.

IO device configuration includes:

* Network setup. The IP, subnet, gateway and DNS of IO board. Or information to get these
  using DHCP. If there are multiple network adapters, then these for each adapter. 
* Device number (up to 64 bits). Usually there are many same kind of devices in the network
  and it is necessary to identify these. is nice to enumerate, lets say my house thermostats,
  like 1, 2, 3… If nothing better is available, MAC address of one of IO devices network interfaces
  can be used. These are or should always by unique. The device name and number must form unique
  identification within the domain of the control computer. 
* Control computer URL or IP and “same segment” check box. The control computer’s IP address can
  be stored as plain number, like “192.168.1.229”, or if “same segment” is selected, then like
  “X.X.X.254”, which would mean three first numbers same as in the board’s IP, but end with 254.
  If this IO device is to connect to a second control computer (for redundancy) same information is needed. 
* Which certificate authority is trusted (certificate chain).
* Password for secure connections.

200127, updated 21.5.2020/pekka
