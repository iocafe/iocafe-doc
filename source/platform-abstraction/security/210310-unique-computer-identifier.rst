CPUID Unique CPU or computer identifier
=========================================
This is most useful for saving user login and password on PC computer or android device
running user interface. Merging CPU id with  hard coded application key prevents snooping
login information for discarded SSD drive, etc.

This could be used to protect server's private key and other secret information. But this
has a major down side: One cannot any more copy the server and assume that it will start
working.

CPUID is not necessarily used for microcontrollers, since flash is integrated to microcontroller:
The attacker has access to both flash and microcontroller/board identification. 

Possible security improvement for servers and microcontrollers is to save additional random
key code to USB flash, etc, removable media, or on a separate device in network.
  Thus removable media or network device would be the an key required to make server or device
  part of IO network. This approach has reliability and other downsides, which most often
  outweigh the added security. Thus taking care that decommissioned server SSD drives are wiped
  or destroyed, and security reset is done on decommissioned devices.

  This function can be criticized that it has limited entropy. This is true, an educated attacker
  could guess the possible combinations and simply try them all. We may improve entropy later on
  by adding mother board serial number, NIC address, etc to this function.

  
 Merge CPU identifier to buffer with XOR:

::

    osalStatus osal_xor_cpuid(
        os_uchar *buf,
        os_memsz buf_sz);

See cpuid/linux/osal_cpuid.c, etc. for details.

210310, 21.3.2021/pekka

