IO device firmware updates
=============================
The software updates to IO device are loaded trough IOCOM, just the same way as the data is loaded. 
Thus the control computer needs to participate in updating IO devices.

IO device software is distributed as installation package:

* The control computer copies this to the IO device. This is transfer erred trough memory block N bytes
  at the time. The IO device writes this to flash, etc, as it receives it.
* The control computer doesn’t care about installation package content, memory addresses, etc. 
  This information is in IO device or in installation package. 
* The control computer cares about IO device name, usually it makes sense to update software to device
  named “hollythermostat”  only from installation package for “hollythermostat”. 
* The control computer also cares about version number, like “190531”. Most often it makes sense
  to load newer software versions to replace old ones. 
* Installation package file name has specific format, that device name, version number and OS/HW type
  can picked up by installer program.

On micro controllers, the software binary is written into flash. Trick here is that program (iocom
library and rest of software) need to run to receive the binary, and we cannot overwrite flash at same
place as we are running from. Solutions:

* Most elegant solution is “dual bank”. Micro controllers like STM32F429 and STM32L467 support this.
  It means that flash is split into two halves. One can be written while program runs from the other one. 
  Once programming is complete, a register controlling which bank is used to run code is toggled and
  the IO board rebooted. A what, we are up an running. 
* Many micro controllers do not support dual bank. We still divide the flash into two halves, and write
  the other when running. But our installation package must have binary code built twice for loading
  it to lower half and to upper half. And when program is updated, we only write one of those, the one
  which doesn’t go on top of running program. Then we need external bit (preferably in EEPROM, not in flash),
  which we toggle after programming flash is complete. Then among first things our application binary does
  is: Am I running in boot address (low one) and is external boot high bit set. If yes, we are running old
  version, jump to start up code from high end. THIS NEEDS TO BE TESTED, IT WOULD BE NICE NOT NEED TO DO
  THIS IN APPLICATION CODE, BUT DECIDE BEFORE APPLICATION CODE STARTS. 


200127, updated 20.5.2020/pekka
