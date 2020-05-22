Implementing pins library stub for a new platform
***************************************************

Example: Implement Raspberry PI IO trough pins library

* Copy pins/code/arduino/pins_basics.c to code/linux/pins_basics_pi.c
* If there is no pins_hw_defs.h in linux folder, copy it also.

Example: Run Jane on Raspberry pi.
Jane was originally tested with hardware named “carol”, which is ESP32 with Arduino libraries wired to control specific set of inputs and outputs. I name the new hardware “david”.

* Copy jane-io.json from jane/pins/carol to  jane/pins/david.
* Modify the jane-io.json to reflect Raspberry PI IO wiring.
* Add call to pins-to-c.py script for “david” thto jane/scripts/pin-config-to-c-code.sh.
      

190918, updated 21.5.2020/pekka
