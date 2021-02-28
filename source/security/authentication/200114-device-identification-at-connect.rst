Authentication 
============================
TLS library takes care of encryption key exchange when a connection is established.
So we have a "secure line". Identifying who is at "who we are speaking to?" is a the challenge. Default authentication used by IOCOM is summarized below. 
Pairing is simple way how an IO device gets the trusted certificate, and how to make it simple to create user account for the new IO device on server.

Client verifies server certificate to know that that the server is legitimate

* server presents a server certificate (and proves it holds matching private key).
* client knows it can connect to a server if server certificate matches to list of accepted certificates stored in client.

Server identifies client by user name, device name + number, or process namme and number, and password

* These must match to valid user accounts in server.
* Password can be transferred to server since connection is secure and the server has been already identified.

200114, updated 28.2.2021/pekka
