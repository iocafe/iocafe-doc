Pairing IO device and server
========================================
Whe IO device connects the first time, it generates it a random secret. From this point on the
generated secret will be used to identify this particular IO device.
The first time an IO device connects to server (controller) for the very first time, a valid
certificate bundle is copied to IO device. Thus an IO device cannot be paired again to another
server unless it is "reseted" to forget all security related information.

On control computer UI, a "New IO device" appears. It shows what kind of board.
If user with admin right clicks "accept it" button -> The new IO device will be added to
valid user accounts. 

Security risks:

* Someone pretends to be the control computer when IO device is being paired for the very first time.
  IO board will be paired to attackers computer, and cannot connect to other server until it is "factory reset".
  One must make sure that IO board is connected to correct server (seen there as a new device) before allowing
  IO board to use heavy/dangerous machinery. Once it has been correctly paired, attacker can not change pairing
  later.
* Someone takes the IO board, hacks access to it's micro controller's flash memory and takes private key from
  there. In this case intruder would be able to pretend to be the IO board and supply control computer 
  with erroneous inputs. To prevent this, USB and serial ports which can be used to debug or install programs
  to IO device must be physically disabled.

200127, updated 20.5.2020/pekka
