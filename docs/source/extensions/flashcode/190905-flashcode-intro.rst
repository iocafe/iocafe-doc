Programming micro-controller flash over network
===============================================

THIS TEXT IS FROM PROTOTYPE AND NOT APPLICABLE FOR NOW

The code executed by a micro-controller is typically stored in on-chip flash memory. There are alternatives, but this is definitely the mainstay. It is important that the code is executed “directly” from flash, thus one cannot overwrite the code being executed.
Code upload while IO device is running
Upload code when IO device is running normally. Since we are using a secure communication protocol where the IO device has been identified by the control computer, we must have the normal IO device code running when transferring the program to the IO device. It is unrealistic that we would have a boot loader which could do TLS negotiation and identify the IO device.

A word about flash memory
*************************
Just a couple of words about how flash memory works. When executing code or reading data. the flash memory is like RAM, consequent memory addresses to read from. Same almost goes for writing, except one can write only 0 bits to flash, never ones. To write meaningful data to flash, an area of flash needs first to be erased. Here is when it gets tricky and chip-specific.
Erasing is filling a block of flash memory with ones. Typically these blocks are quite large, for example, 8kB. On some micro-controllers are erasable blocks have the same size, on others beginning of flash address space has smaller blocks which get bigger towards higher addresses.
There are micro-controller specific “registers” for controlling flash write protection, erasing the blocks and for selecting boot bank.
Dual bank feature is used, if it is available
Some micro-controller have something called dual bank feature. It allows flash memory to be divided into two banks, which can be swapped. Dual bank feature makes thing simple. If it is not available, we need to add some complexity.

Flash programming and data using “dual bank”
********************************************
The “dual bank” feature allows flash memory to be divided into two banks, which can be swapped. For example, by default, active bank 0 is in address 0x000000 and bank 1 in address 0x100000, and at boot, we start executing software from address 0. If we set bank 1 to be the active bank, then at next boot bank 0 is in address 0x100000 and bank 1 in address 0x000000, and at boot, we start executing software from address 0. Effectively booting from bank 1. Note: Be careful when programming the flash, often erasing doesn’t care which bank is active, but writing to flash does. The flash partitions are:

* Bank 0 code
* Bank 0 data
* Bank 1 code
* Bank 1 data

Software update procedure:

* If bank 0 is active, erase “bank 1 code”. Otherwise, if bank 1 is active, erase “bank 0 code”.
* While the software is received (trough SSL socket) write it to address 0x100000 (this is the higher address bank, code is being executed from the bank with lower address).
* Once the whole program has been transferred verify that the program was written correctly.
* If all is good, and only if, switch active bank to another one.
* We may reboot the micro-controller automatically at this point. Or we may want to leave the controller process running and take updates to use in next restart.

This should prevent corruption of the program, even the power or connection would be lost at any time during the IO device software update.

We also need to write data to flash. This includes network configuration, device’s user name, and password and possibly client certificate, etc. In addition, there may be IO device application data to write. The procedure for writing is:

* Erase “bank 0 data” partition as needed, write data and checksum for the data.
* Verify that this is written correctly. If not, try again.
* Erase “bank 1 data” partition as needed, write data and checksum for the data.
* Verify that this is written correctly. If not, try again.

Thus bank 0 and bank 1 should contain exactly the same data, except if power is lost during saving. Validity check relies on checksum. Reading procedure is.

* Read data and checksum from “bank 0 data”. If the checksum matches to data, we are done.
* If getting data from bank 0 failed, try bank 1. Read data and checksum from “bank 1 data”. If the checksum matches to data, we are done. If both banks fail, we need to initialize data with defaults.

Without dual bank feature
*************************
We divide the available flash memory into five “partitions”. I borrow here hard drive term, here it simply means a range of flash memory reserved for a specific use.

* boot update check code
* active code
* downloaded code
* active data
* backup data

Software update procedure:

* Erase “downloaded code”.
* While updated IO device program is received (trough SSL socket) write it to “downloaded code”.
* Once the whole program has been received, verify it. If no good, report an error and quit the update.
* If all fine, calculate a checksum for the new version of the program and mark that “we got new software”. 
  The “mark” and checksum can be stored at the very beginning of “downloaded code”.
* We may reboot the micro-controller automatically at this point. Or we may want to leave the controller process running and take updates to use in next restart.

Boot procedure.

* Start executing code within “boot update check code” partition.
* Check if we have a new program in “downloaded code” with “mark” and matching checksum.
* If not, jump to the beginning of “active code” partition and start running IO. Done with this procedure.
* Copy code from “downloaded code” to “active code” partition.
* Verify that copying went correctly. If not, reboot to try again.
* Clear “mark” in “downloaded code”. This prevents copying at next boot. Alternatively, the whole “downloaded code” can be erased.

Writing and reading data is quite similar to operation with dual bank feature. When writing, the data with checksum is written to both “active data” and “backup data” partitions. When reading any of the two with good checksum is accepted. If neither has good checksum, we default to initial values.

190905, updated 5.9.2019/pekka
