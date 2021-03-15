Platform security functionality
=================================
IOCOM's communication and device security is largely based on ready TLS library, typically MbedTLS. We have used OpenSLL as well, but OpenSLL wrapper API is not up to date (15.3.2021).

TLS and server certificates, user accounts, device secrets and user name/password concept are used to secure the communication. 

Device level security is not part of iocom, it relies on operating system security or on hardware design. Practical implementation depends on what hardware or operating system the end product runs on and what are it's security needs. Security adds to difficulty and cost, thus maximum security is not the best security for every use.

Besides of communication, the TLS library is used for cryptographic hashes and for AES encryption. User password and device secrets are stored on server as cryptographic hash. AES is used to provide fallback, if operating system decurity fails: Device secret, private server key and saved user password (to avoid retyping) are stored as AES encrypted format. Hardware identification (CPUID, etc) can be merged to hard coded AES encryption key.


.. toctree::
   :maxdepth: 2
   :caption: Contents:

   210310-cryptographic-hashes
   210310-aes-encryption-wrapper-api
   210310-unique-computer-identifier

210310, 23.3.2021/pekka

