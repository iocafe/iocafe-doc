SPI connector A
==================================

SPI interface uses 2x5 FC-10P 2.54mm dual row IDC sockets connectors are. This allows for flat ribbon cable.

Connectors soldered to both microcontroller and device boards are male.
Ends of connecting cable are female. The cable is straight cable, and connects pin 1 to pin 1, pin 2 to pin 2...


.. figure:: pics/spi-connector-and-cable.png

   SPI connector and cable orientation - both controller board end and device end use the same pinout. 


.. list-table:: connecor pins,
  :widths: 20 20 60
  :header-rows: 1

  * - pin number
    - pin name
    - description
  * - 1
    - GND
    - Ground.
  * - 2
    - V
    - 3.3V 
    - SPI operating voltage, typically 3.3V. Controller board powers the device.
  * - 3
    - 5V
    - Controller board powers the device.
  * - 4
    - NC
    - Not connected, used to avoid connecting wrong cable. Box header pin should be cut, and cable plugged for this pin.
  * - 5
    - SCLK
    - SPI clock from controller (master) to device (slave). 
  * - 6
    - MOSI
    - Data from controller to device, master out/slave in. 
  * - 7
    - MISO
    - Data from device to controller, master in/slave out. 
  * - 8
    - SS
    - Chip select, activates SPI device. This signal is inverted, the device is activated when this is 0V and deactivated when 3.3V.

XX


