Hardware IO abstraction
================================

PINS library - Generic IO wrapper
**********************************************
The “pins” library separates IO application from hardware level IO functions, so the same IO application can be
run on different micro-controllers or as Linux/Windows simulation. This library is optional: Any other IO library
can be used as well. Typically IO hardware accessed tough the “pins” interface are GPIO, ADC/DAC, PWM, timers, etc.

Function of the "pins" library:

* Provide an application with hardware independent access to GPIO, analogs, PWM, timers, etc.
* Connect IO to communication without minimal application code.
* Hardware configuration is written in JSON only once and converted to C code, connected to IOCOM and documented by scripts.

Key points:

* Hardware specific IO headers are not included in application (except when there is need to bypass "pins").

.. toctree::
   :maxdepth: 2
   :caption: General

   general/190918-using-pins-chapter
   general/190918-implementing-pins-for-new-hardware
   general/spi/spi-devices

.. toctree::
   :maxdepth: 2
   :caption: API Reference

   api-reference/260319-gpio

.. toctree::
   :maxdepth: 2
   :caption: Examples

   examples/190918-pins-jane-example

260319, updated 19.3.2026/pekka
