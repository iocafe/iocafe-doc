SPI connector A
==================================

SPI interface uses 2x5 FC-10P 2.54mm dual row IDC sockets connectors.

Connectors soldered to both microcontroller and device PCB boards are male.
Ends of connecting cable are female. The cable is straight cable, and connects pin 1 to pin 1, pin 2 to pin 2...
Flat ribbon cable can be used.

.. figure:: pics/spi-connector-and-cable.png

   SPI connector and cable orientation - both controller board end and device end use the same pinout.

.. list-table:: Connector pinout
  :widths: 20 20 60
  :header-rows: 1

  * - pin number
    - pin name
    - description
  * - pin 1
    - GND
    - Ground.
  * - pin 2
    - 3.3V
    - SPI operating voltage, typically 3.3V. Controller board powers the device.
  * - pin 3
    - 5V
    - Controller board powers the device.
  * - pin 4
    - NC
    - Not connected, used to avoid connecting wrong cable. Box header pin should be cut, and cable plugged for this pin.
  * - pin 5
    - SCLK
    - SPI clock from controller (master) to device (slave).
  * - pin 6
    - MOSI
    - Data from controller to device, master out/slave in.
  * - pin 7
    - MISO
    - Data from device to controller, master in/slave out.
  * - pin 8
    - SS
    - Chip select, activates SPI device. This signal is inverted, the device is activated when this is 0V and deactivated when 3.3V.
  * - pin 9
    - GPIO A
    - Application specific GPIO pin A, optional. Specific use like display backlight control, etc.
  * - pin 10
    - GPIO B
    - Application specific GPIO pin B.


Cable to connect SPI from 2x5 IDC to breadboard
**************************************************

Straight 1 x 10 pin strip (pin numbers 1 - 10) which can be presssed into breadboard.
Pin use as described in "connector pinout" above.
