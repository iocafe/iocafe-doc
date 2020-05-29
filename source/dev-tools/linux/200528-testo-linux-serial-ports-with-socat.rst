Testing serial communication with socat
========================================
I is useful to test new IO device software in PC without actual hardware. Serial communication of 
a simulated IO device can be tested either in linux or Windows. In Linux one can use socat to create 
virtual serial ports connected together. For example 

::

    sudo apt-get install socat
    sudo socat PTY,link=/dev/ttyS30 PTY,link=/dev/ttyS31
    cd /dev/pts
    sudo chmod 0666 2
    sudo chmod 0666 3

Would install socat and create and connect ports “ttyS30” and “ttyS31” together.  Initially this gives only
root access to the simulated ports. The /dev/ttyS30 and /dev/ttyS31 are links to /dev/pts/2 and /dev/pts/3 (in
my test, for you these may be different. 

::

    cd /dev
    ls -la ttyS3* 
    output: lrwxrwxrwx 1 root root       10 Jun  3 14:02 ttyS30 -> /dev/pts/2
    output: lrwxrwxrwx 1 root root       10 Jun  3 14:02 ttyS31 -> /dev/pts/3
    cd /dev/pts
    ls -la
    sudo chmod 0666 2
    sudo chmod 0666 3

