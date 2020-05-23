IO device configuration
=========================

IO device configuration includes:

* Wifi configuration. Wifi network name and password. Gazerbeam, serial port for development testing, PC computers use OS Wifi setup.
  If there can be multiple WiFi networks, these for each network.

* NIC configuration. Either DDCP or static. If static, the IP, subnet, gateway and DNS of the NIC. 
  If there are multiple network adapters, then these for each adapter. 

* Device name. Typicaly hard coded.

* Device number. There can be several same kind of devices in the IO device network and it is necessary
  to identify these. is nice to enumerate, lets say my house thermostats,
  like 1, 2, 3… If nothing better is available, MAC address of one of IO devices network interfaces
  can be used. These are or should always by unique. The device name and number must form unique
  identification within the domain of the control computer. 

* IO device network name, "*" for automatic.

* IO boards, server IP address to connect to.

200127, updated 21.5.2020/pekka
