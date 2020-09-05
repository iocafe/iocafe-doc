SPI devices
==================================

.. figure:: pics/spi-bus-multiple-devices.png

   SPI bus wires, each device has own CS pin.

* SCLK: Clock signal
* MOSI: data from master to slave (master out slave in)
* MISO: data from slave to master
* CS1, CS2, CS3: Chip select for SPI devices. Active low.

.. toctree::
   :maxdepth: 1
   :caption: Contents:

   mcp3208

