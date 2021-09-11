Authentication message
=======================

Authentication message is sent by both ends of connection immediately after connection is transferred.
No data is transferred befor authenticatin messages have been exchanged and accepted. 

.. list-table:: Message content
  :widths: 25 15 15 45
  :header-rows: 1

  * - name/address
    - bytes
    - type
    - use
  * - SYSFRAME_TYPE 
    - 1
    - uchar
    - Constant IOC_AUTHENTICATION_DATA (2) indicating an authentication message.s
  * - AUTH_FLAGS
    - 1
    - uchar
    - Flag for some connection attributes and remainder of authentication message fields.
      IOC_AUTH_ADMINISTRATOR (1) = client requires administrator priviliges.
      IOC_AUTH_UNIQUE_ID (2) = authentication message contains unique device ID.s
      IOC_AUTH_CERTIFICATE_REQUEST (4) = client request for a copy of server certificate.
      IOC_AUTH_CLOUD_CON (8) = this may be OBSOLETED.
      IOC_AUTH_CONNECT_UP (16) = connecting upwards in device hierarchy (from device to server, etc).
      IOC_AUTH_DEVICE_NR_2_BYTES (32) - Two bytes needed for device number in this message.
      IOC_AUTH_DEVICE_NR_4_BYTES (64) - Four bytes needed for device number in this message.
      IOC_AUTH_BIDIRECTIONAL_COM (128) - Bidirectional memory blocks supported. This may be OBSOLETED.
  * - USER_NAME
    - N
    - char[N]
    - Device or user name, without serial number. NULL terminated C string.
  * - DEVICE_NR
    - 1/2/4
    - number
    - Device number. Number 0 is unspecified, and device number may be assigned by server.
      1, 2 or 4 bytes, least significant byte first. Flag IOC_AUTH_DEVICE_NR_2_BYTES indicates
      that this has 2 bytes and flag IOC_AUTH_DEVICE_NR_4_BYTES four bytes. If none is specified,
      one byte.
  * - UNIQUE_ID
    - 0/12
    - uchar[12]
    - Unique device ID. This is used by server to assign same device number to reconnecting
      client which doesn't request a specific device number. This field is optional, flag 
      IOC_AUTH_UNIQUE_ID indicates if it is here.
  * - NETWORK_NAME
    - 1/2/4
    - char[N]
    - Network name, without serial number. NULL terminated C string. Client may use this
      to select IO network to connect to and server publishes it's default IO network. 
      Asterisk * or empty value mean unspecified. 
  * - PASSWORD
    - char[N]
    - uchar[]
    - User's or devices password. NULL terminated C string. For unsecured connections
      this is always empty string, just NULL character.


Authentication message is wrapped within iocom message frame.

210910, updated 11.9.2021/pekka
