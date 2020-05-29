Test your new application on the target hardware
================================================

Building
********
Before compiling, run script to convert JSON configuration to C code and to binary compressed JSON data. 
Refer to development tool documentation for build instructions, extra notes will be in "development tools" 
section of this document. 

Uploading
*********
See notes in "Set up and test development tools". 

Testing & debugging
********************
The latest version should be used if you participate in development of IOCOM or another related library. 
If your goal is to create IO device application, latest code from "master" is likely to bring you unwelcome issues.

Test communication
******************
Both socket and serial communication can be tested by running other end of communication in PC and other in 
micro-controller. Use putty to see that you that you have correct serial or socket port.

TLS and security
****************
This is where it gets quite complicated, just to get main idea:

* Test first with un-encrypted sockets, and move on to TLS only when that is working. 
* Server side is identified by client from server certificate. 
  Client must have accepted certificate bundle loaded.
* Client (IO device) is identified by user name/password over TLS connection. 
  Server needs to have account for the device which is allowed to connect.
* Defining macro EOSAL_RELAX_SECURITY=1 disables most of security. If you compile both ends of communication 
  with this define, no username/password nor server certificate checks are done.

