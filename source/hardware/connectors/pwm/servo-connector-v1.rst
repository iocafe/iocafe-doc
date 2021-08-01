Servo connector A
====================

The connector provides PWM position control amd operating voltage around 5V to a servo or other device.
Servo control uses 1x3 2.54mm pin strip.

Microcontroller PCB board has male 3 pin connector, and the cable from servo the female.

.. figure:: pics/210801-servo.jpeg

   Servo and cable.

.. list-table:: Connector pinout
  :widths: 20 20 60
  :header-rows: 1

  * - pin nr and color
    - pin names
    - description
  * - 1 black
    - GND
    - Ground.
  * - pin 2 red
    - VDC
    - Power for the servo, nominally 5V (4.8V - 6.0 V), but can be as low as 3.3 V.
  * - pin 3 white
    - SIGNAL
    - PWM signal to control servo position. Pulse amplitude may be less than operating voltage in pin 2, down to 3V.

For servos, the pulse frequency is typically 50 Hz, but the exact frequency does not matter. 
The servo position is controlled by changing pulse width. 1.5 ms corresponds to center point of the servo.
Shortening or lengthening the pulse (1.0 ms to 2.0 ms) will turn the servo.

Higher pulse frequency is used to control LED brightness and especially audio by PWM. 

1.8.2021/pekka
