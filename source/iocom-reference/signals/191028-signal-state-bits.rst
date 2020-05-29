Signal state bits
=================
It is not enough to know the value of a temperature sensor signal (from IO device), speed setting
for a motor (to IO device) or if some binary input is ON or OFF when the device. 

Along the temperature value, let’s say 30 degrees Celsius, we need to pass information that signal 
“has value/is connected” and the sensor is not broken. This type of information is called signal 
state byte, and we should have one for every signal.

Bits in signal state byte
*************************
Signal byte contains signal state bits as “----YOCX”.

* bit Y, OSAL_STATE_YELLOW = 8
* bit O, OSAL_STATE_ORANGE = 4
* bit C, OSAL_STATE_CONNECTED = 2
* bit X,  OSAL_STATE_BOOLEAN_VALUE = 1
* bits marked with ‘-’ are reserved for future uses (redundancy, etc).

OSAL_STATE_CONNECTED. There are times when controller doesn’t know state of input on IO board or
IO board doesn’t know weather controller wants some output to be set ON or OFF. This happens for
example when IO device connects to controller, or either IO device or controller is restarted,
network is temporary disabled… Also connections can be chained, so that signal needs to jump trough 
a few hoops, before it is known somewhere else. The connected bit is set to 1 along with signal value. 

OSAL_STATE_ORANGE and OSAL_STATE_YELLOW bits. These are for reporting broken or untrusted hardware. 
Total failure is indicated by OSAL_STATE_RED = 12, which is simply both OSAL_STATE_YELLOW and 
OSAL_STATE_ORANGE bits set. For example if temperature sensor input has value 0 or 4095 (for 12 bit A/D)
it can be quite safely assumed that it is not connected and IO device can set OSAL_STATE_ORANGE for it.
Mask OSAL_STATE_ERROR_MASK is also defined with both error/warning bits set.

Signal state with data byte(s)
******************************
When we transfer for example 16 bit temperature  signal, we need 3 bytes: State byte followed by 
two data bytes. Same goes for motor speed control to other direction. 
Boolean ON/OFF values are embedded in OSAL_STATE_BOOLEAN_VALUE bit of the state byte.
To summarize, every signal except boolean values start with state byte followed by data bytes, 
in small endian byte order. Boolean value with signal bits is transferred as one byte.

Elaborating what IO device should do when it doesn’t receive control
********************************************************************
When a signal controlling to IO board has no OSAL_STATE_CONNECTED bit, what should IO board do? 

* Let’s assume that signal controls chain saw spinning speed. If no speed control is received, 
  IO board should gradually (quickly, but not so suddenly that the saw spins our of control) power down the motor. 
* If the signal controls toilet tank fill valve, the output should be turned off to avoid flooding.
* If the signal controls pneumatic cylinder to position a mechanical arm, the arm might be left at position
  it is. We do not want loss of connection to cause sudden unexpected movements, these could be a safety hazard.

Same goes also for controller. What it should do if it doesn’t receive input signal from an IO device.

Applicability
*************
Signal bits are not part of lowest raw IOCOM layer. It only handles transfer of memory blocks, and 
doesn’t care about content. Anyhow if we deal with real world IO devices, I recommend to use JSON 
configuration and signal state as convention to enable device interoperability.


191028, updated 21.5.2020/pekka

