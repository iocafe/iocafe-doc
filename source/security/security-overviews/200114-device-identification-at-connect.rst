Device identification at connect
================================

Client
**********
Client verifies that server is legimate by checking server ceriticate:
* server has server certificate, +public & private keys 
* cliset knows who it can trust by "chain bundle" 


Server
******
Server identifies client by user (device) name and password

* These must match to user accounts in server
* Password can be transferred as clear text since connection is secure and server has been proven legimate.


200114, updated 20.5.2020/pekka
