Serial communication
=======================
Serial communication is differs from socket. Integrity of transferred data must be ensured at protocol level, broken connections nee to be handled, etc. There is also a need to detect beginning of connection, etc. Serial communication bandwidth is usually more limited than in network, and latency may be less of an issue than in large scale network. We treat serial communication as “almost reliable”: we pass only initial state and later on just changes. Communicatin errors are detected, and connection is restarted on error. 
imilar to socket, communication transmits changes to IO state immediately without waiting for a query, token passing, etc. Flow control is kept to minimum. This approach gives good troughoutput and low latency. But it puts a few requirements on design.

* Full duplex connection is required. 
* Device should be able to process received data at serial baud rate. Primitive flow control is provide to avoid collapse of communication on overload, bu if we hit this cealing communication may slow down significantly.
* In sigle thread mode, the iocom_run() functin should be called repeatedly without long pauses. Maximum tolerable pause can be calculated: Assume line speed 38400bps and serial receive buffer size 256 bytes. 38400 bps is about 3840 bytes per second (8 data bits, one stop bit, parity bit).  In numbers 1000ms * 256/3840 = 67ms. Thus in this example case maximum acceptable pause between iocom_run() calls is 67ms. To make it safe we add 10% margin and say 60ms. To work reliably also under full load, the function must be called at least 17 times per second.  

Processing capacity of an embedded device is hard to calculate, thus it may also be challenging to be sure of iocom_run() call frequency by reading code. But these are easy to test. For example we are testing IO board we just run controller software in linux/windows computer which writes random data to IO device at maximum line rate. If IO board cannot process it or call iocom_run() often enough, the connection will be limiteed by flow control.
Error detection, handling and keep alive messages
Transmission errors are detected in two main ways. First every frame transmitted over serial connection is enumerated. If these are not received in precise order, the connection is dropped on error. Secondly every frame contains check sum, which must match to rest of frame content. First frame has FRAME_NR zero. From then on frame number is incremented until IOC_MAX_FRAME_NR, after IOC_MAX_FRAME_NR frame number jumps to 1 (number 0 is never repeated). In code the frame number is maintained in “frame_nr” for both outgoing and incoming data. 

If error is detected, a the connection swiches back to “initiate connection” mode. This starts either BY CONNECT or DISCONNECT character. Thus the connection is dropped off and reinitialized. All key frames are retransmitted. This is somewhat time consuming and the connection will pause for a period.  This way of handling errors is well suited for serial links which do not normally have errors, like adequately designed wired serial connections. It is not well suited when communication breaks are frequent, for blue tooth, etc. 

If no other data is sent, a connected connection sends periodic “keep alive” message to keep other end of connection that all is still working. If no keep alive no other data is received for a period, connection is terminated and moves back to “connection initialization state”. So silent line and communication errors are treated exactly the same way.
Check sums
The check sum is calculated from whole transmitted frame, including header. The two bytes used to store the check sum are zeros when the check sum is calculated. Used algorithm is Modbus CRC-16. 
See ioc_checksum.c for details.
Establishing connection - client
When client end of connection is in disconnected mode, it periodically sends “CONNECT” character to serial line. Once server end receives the “CONNECT” character, it sends “CONNECT_REPLY” character back... Serial communication connect state machine for client end:


Establishing connection - server
When disconnected, the server end listens for “CONNECT” character from client and initiation goes on from there. Serial communication connect state machine for the server end:


Timing
*************
Timing is defined in milliseconds regardless of connection speed. Reasoning for this is that we use relatively long timeouts which can be easily timed using typical OS time slice. Notice that timeouts effect to communication speed/latency only when there are transmission errors in serial line. 

* There may not be long pauses within a message frame. Maximum allowed pause between two characters 
  is 100ms. If there is longer pause than 100 ms, receiver will decide connection is broken and clear
  receive buffer and restart establishing connection.
* There must be at least 200ms pause between “connect” messages sent by client. 

Testing serial communication
*****************************
THIS NEEDS TO BE MOVED TO DEV TOOLS
I is useful to test new IO device software in PC without actual hardware. Serial communication of a simulated IO device can be tested either in linux or Windows. In Linux one can use socat to create virtual serial ports connected together. For example 

::

    sudo apt-get install socat
    sudo socat PTY,link=/dev/ttyS30 PTY,link=/dev/ttyS31
    cd /dev/pts
    sudo chmod 0666 2
    sudo chmod 0666 3

Would install socat and create and connect ports “ttyS30” and “ttyS31” together.  Initially this gives only root access to the simulated ports. The /dev/ttyS30 and /dev/ttyS31 are links to /dev/pts/2 and /dev/pts/3 (in my test, for you these may be different. 

::

    cd /dev
    ls -la ttyS3* 
    output: lrwxrwxrwx 1 root root       10 Jun  3 14:02 ttyS30 -> /dev/pts/2
    output: lrwxrwxrwx 1 root root       10 Jun  3 14:02 ttyS31 -> /dev/pts/3
    cd /dev/pts
    ls -la
    sudo chmod 0666 2
    sudo chmod 0666 3

I have used com0com on Windows. It works nicely, if one gets it set up. But setting it to run on Windows 10 is a hassle. It needs to load unsigned device driver(s). 

200112 – updated 22.5.2020/pekka

