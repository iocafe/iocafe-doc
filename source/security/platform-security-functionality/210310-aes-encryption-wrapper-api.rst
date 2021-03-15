AES encryption wrapper API
=========================================

AES encryption is used saving server's private key, device secrets and user login and password.

The AES encryption key is a combination of simple fixed key, application hard coded key (from
define) and optionally CPUID, which intends to uniquely identifiy the individual computer.

Application hard coded key is set by define OSAL_AES_KEY at build time. 
OSAL_AES_KEY="myseacretkey" can be defined in Cmakelists.txt, platformio.ini,
in /coderoot/eosal/eosal_linux_config.h, etc.

eosal_linux_config.h to set hard coded key and to force using CPUID even on PC:

::

    #define OSAL_AES_KEY "KebabMakkaraKioski"
    #define OSAL_AES_CRYPTO_WITH_CPUID 1

The unique hardware identification is normally used only for user login for PC computers, 
since we want to be able to make a working backup copy of a server computer. For microcontrollers
we use this, if CPUID functionality support is available (OSAL_CPUID_SUPPORT define).

This is not bullet proof. Serious microcontroller security should be done so that debugging
ports, like JTAG and UART are permanently disabled at production version. On Windows and
linux we should primarily depend on operating system security. But since we live in real
world with real people, and errors happen with these, we want at least to make it 
hard to get the device secret, user login or server's private key.
  
Initialize AES encryption key:

::

    void osal_initialize_aes_crypt_key(
        os_uchar secret_crypt_key[OSAL_AES_KEY_SZ],
        os_int use_cpuid);

Encrypt or decrypt data, AES 256:

::

    void osal_aes_crypt(
        const os_uchar *data,
        os_memsz data_sz,
        os_uchar *buf,
        os_memsz buf_sz,
        const os_uchar key[OSAL_AES_KEY_SZ],
        osalAesOperation operation);

See tls/common/osal_aes_crypt.h, tls/common/osal_aes_crypt.c and tls/mbedtls/osal_mbedtls_aes_crypt.c for details.

210310, 21.3.2021/pekka

