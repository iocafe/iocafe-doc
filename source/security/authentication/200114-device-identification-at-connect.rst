Authentication 
============================
TLS library takes care of encryption key exchange when connection is established.
So we have a "secure line". Identifying who is at "the other end or the line, who we are speaking to?"
before we send any data,is a the challenge. Default authentication used by IOCOM is summarized below. 
Pairing is popular way how an IO device gets the list of accepted server certificates and how user
account on server can be created for a new IO device.

Client verifies server certificate to know that that the server is legitimate

* server presents a valid certificate (and proves it holds matching private key).
* client knows it can connect to a server if server certificate matches to list of accepted certificates stored in client.

Server identifies client by user name (device name + number) and password

* These must match to valid user accounts in server.
* Password can be transferred to server since connection is secure and server has been already identified.

200114, updated 20.5.2020/pekka
