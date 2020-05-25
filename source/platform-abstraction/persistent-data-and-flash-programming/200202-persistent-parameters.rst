Persistent storage – osal_persistent
=======================================
Micro-controllers store persistent board configuration parameters in EEPROM or flash. On Windows/Linux  these parameters are usually saved in files within OS file system. Several different implementations are needed because flash and EEPROM use on micro-controllers is usually platform/micro-controller/board dependent.
 
Persistent storage functions
*******************************
Include “eosalx.h” to use persistent storage functions. Define OSAL_OSAL_PERSISTENT_SUPPORT is nonzero if persistent storage is available, value 1, 2.. selects the used implementation for the platform.

Initialize persistent storage access. Call this first. prm can be OS_NULL if not needed.

::

    void os_persistent_initialze(
        osPersistentParams *prm);

Load parameter structure identified by block number from persistent storage. Load all parameters when micro controller starts, not during normal operation. If data cannot be loaded, leaves the block as is. Returned value maxes at block_sz.

::

    os_memsz os_persistent_load(
        osPersistentBlockNr block_nr,
        os_uchar *block,
        os_memsz block_sz);

Save parameter structure to persistent storage and identigy it by block number.

::

    osalStatus os_persistent_save(
        osPersistentBlockNr block_nr,
        os_uchar *block,
        os_memsz block_sz);

Data is saved and loaded as blocks
***********************************
Data is saved and loaded as memory blocks. When storing data in flash or EEPROM, we do not
need to care about portability of the data format from micro-controller architecture to another.
Structure alignment will not change and data will not be copied from big 32 bit MCU to 
16 bit, etc. So we can present a set of parameters as C structure:

::

    typedef 
    {
        os_short version;  // Have this always first, even if you would not need it
        os_char ip[MY_IP_SZ];
        os_char subnet[MY_IP_SZ];
        os_char gateway[MY_IP_SZ];
        os_boolean dhcp;
    }
    MyNetConfParams;

Load parameter to C structure as below. If the data cannot be found or is corrupted in storage, 
the os_persisent_load() leaves the parameter structure unchanged. If it should be zeroed, 
call os_memclear(&prm, sizeof(prm)) before. 

::

    #define MY_PRM_BLOCK_NR OS_PBNR_APP_1
    MyNetConfParams prm;
    os_memsz sz;
    sz = os_persistent_load(MY_PRM_BLOCK_NR, &prm, sizeof(prm));
    if  (sz == sizeof(prm)) {
        os_console_write(“success, exact version match”);
    }
    else if (sz > 0)
    {
        os_console_write(“success, not same version (may work, may not)”);
    }

When parameters have changed, we need to save those. Set version number before saving,
this is likely to make things easier later on.

::

    prm.version = 1;
    if (os_persistent_save(MY_PRM_BLOCK_NR, &prm, sizeof(prm))) {
      os_debug_error(“saving my parameters failed”);
    }

New versions of data structure
*************************************
Often we need to add parameters to existing set, change some array size, etc. We do not want to
reconfigure a board at software update.  Version number should be incremented when saving and
can be checked when loading. 

Implementation
*****************
Flash and EEPROM hardware use is typically micro-controller specific, and common library
functionality can rarely be used. On Arduino there is valuable effort to make this portable,
but even with Arduino we cannot necessarily use the portable code: To be specific we cannot use
Arduino EEPROM emulation relying on flash with secure flash program updates over TLS. 

On raw metal or custom SPI connected EEPROMS we always end up with micro-controller or board specific code.

On Windows or Linux this is usually simple, we can just save board configuration in file system.
 Except if we have read only file system on Linux. 

The "persistent" directory contains persistent storage interface header osal_persistent.h, which declares
how the persistent storage access functions look like. The actual implementations are in subdirectories
for different platforms, boards and setups.

The subdirectories named after platform, like "arduino", "linux" or "windows" contain either the persistent
storage implementations for the platform or include C code from "shared" directory, when a generic
implementation can be used. The "metal" subdirectory is used typically with micro-controller without
any operating system.

Warning: Do not use micro-controller flash to save any data which changes during normal run time operation, 
it will eventually burn out the flash (death of the micro controller).

200202, updated 20.5.2020/pekka
