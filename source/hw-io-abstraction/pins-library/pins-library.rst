Generic interface to hardware specific IO
******************************************
The “pins” library separates IO application from hardware level IO functions, so the same IO application can be run on different micro-controllers or as Linux/Windows simulation. This library is optional: Any other IO library can be used as well. Typically IO hardware accessed tough the “pins” interface are GPIO, ADC/DAC, PWM, timers, etc. 
About “pins” library

Function of the "pins" library is to:

* Provide an application with hardware independent access to GPIO, analogs, PWM, timers, etc.
* Connect IO to communication without minimal application code.
* Hardware configuration is written in JSON only once and converted to C code, connected to IOCOM and documented by scripts.

Key points

* Hardware specific IO headers are not included in application (except when there is need to bypass "pins").
* Somewhere we need to map hardware pin numbers, etc. need to the application. This could be simply written as one C file. 
  Here approach is to write it as JSON and then generate the C files automatically by Python script. This enable using the
  PIN information to automate mapping IO pins to IOCOM signals, generate documentation by scripts, etc, and not write same
  information twice. 

.. toctree::
   :maxdepth: 2
   :caption: Contents:

   190918-using-pins-chapter.rst


190916, updated 21.5.2020/pekka
