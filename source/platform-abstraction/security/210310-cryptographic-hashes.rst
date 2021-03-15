Cryptographic hashes
=========================================
Passwords stored in server are cryptographically hashed. Cryptographic hashes of two passwords can be used to
check if passwords do match, but are not secret. One cannot get original password from it's cryptographic hash.
 
Calculate SHA-256 cryptographic hash (as binary) of buffer given as argument:

::

    void osal_sha256(
        const os_uchar *d,
        os_memsz n,
        os_uchar *md);

See tls/mbedtls/osal_mbedtls_hash.c, etc. for details.

210310, 21.3.2021/pekka

