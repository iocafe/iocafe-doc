Certificates and chain of trust
================================

This present simple yet useful chain of trust. 

Organization - root certificate and private key
************************************************

We use a self signed root certificate. OpenSSL can generate this, see /coderoot/eosal/extensions/tls/keys-and-certs directory for current readme, file names and scripts.
This results two files: 

* Public root certificate file "rootca.crt", which contains the public key.
* Private confidential key file "rootca.key".

Root certificate is not a secret, these can and should be shared with everyone who needs it.
It is good idea to place this files in git repository to make it accessible to whole development team.

Root's private key is a secret, and needs to be kept confidential and safe (if this is compromised, whole 
security collapses. If this is lost, no new servers can be added).

All certificate related files are in PEM format. This means that they are text files, but contain encoded
binary data, thus do not make much sense when opened in editor. 

What root certificate actually is and what it does: 

* It is a file, I use name "rootca.crt". 
* It represents the organization, so for iocafe it contains "O (Organization): iocafe". 
* This file can be manually copied to client, be part of software installation, or a client IO device can obtain a copy of it the very first time it connects to server (pairing/automatic trust). 
* When TLS connection is established, client can verify that the certificate presented by server at that time has been
  signed by holder of root's private key file. In my case, this means it will trust all server certificates signed 
  with iocafe's private key. 

Certifying a server - server certificate and private key
**********************************************************

A server is identified by server certificate. This, similar to organization's root certificate, has matching private
key which is to be kept secret and only within the server itself. 

* The person who sets up a new server, first generates key pair and certificate request with server information and sends it to holder of root certificate.
* The root send back the signed server certificate, which is stored within server. 
* The server and it presents this certificate at client connect.
* The server's private key is stored only within server (you can have backup in safe place). 

If cloud server is maintained by different organization?
************************************************************

A switchbox process running on cloud server may belong to different organization than the network
which it routes to. For example:

* My home server is called "myserver.pekkahome", and I use "pekkahome" as my home "organization" name.
* Organization "iocafe" is maintaining the cloud server.

Following is needed so that my home server is able to share it's "end point" in switchbox:

* My home server must list "iocafe" at trusted list (and have copy of iocafe's certificate).
* IOcafe's switchbox must have user account for "myserver.pekkahome" to be allowed to connect and publish end point.

For client to connect to "myserver.pekkahome" trough iocafe's cloud switchbox:

* it must list "iocafe" on trusted list.
* myserver.pekkahome must have user account for the client.

Certificate expiration 
***********************

Certificate expiration check is disabled: I do not want my IO to go down at specific date, if the certificate
renewal process is neglected. There is currently no automatic system to renew server certificates in field
devices, often without internet connection. 

This means that devices that are no longer used, need to be wiped clean, or hammered unconscious before recycling
those. Otherwise attacker can simply pick up old server or IO device from junk pile and get authentication information 
from it. 

One future option would be to add support for "certificate revocation list", so devices/processes/users 
which may be compromised could be specifically blocked.

Client certificates
***********************

We do not use client certificates, even they in some cases would add a measure of security. 
Creating client certificates automatically in micro-controller doesn't really work, and copying a certificate in
manually when a new IO device is installed, is unreasonable burden to end user. Since good security can be 
accomplished without client certificates, these are not used. If you want to use client certificates, it is
possible, but requires modifying TLS wrapper code. 

Restorable server backup copy vs. security
**************************************************

This is choice between security and minimizing down time on HW failure. We gain in one, we lose in other. If we keep all files so that 
a copy of server works, we can backup and restore a server without problems. We may even have ready back up server as replacement. 
But if server backup gets in wrong hands, security is compromised. 

It is possible to incorporate some HW specific data, like MAC addressess, SSD serial numbers, etc, in encryption of confidential
information. This makes the backup data much safer to handle, but a restored backup will not start back up easily on new hardware. 
One is forced to reconfigure much of security. By default, iocom, etc, are written so that a backup can be restored and system will just start up. 

updated 28.2.2021/pekka
