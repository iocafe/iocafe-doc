Using STM32 on-chip integrated Ethernet is easy!
=================================================
I tested using LwIP (lightweight IP library) on the STM32 Nucleo F429ZI development board. This board has ARM chip with integrated Ethernet, and the Nucleo board has PHY layer.

-------------- Requirements
This library can only be used with STM32 chips/boards with Ethernet compatibility.
Example dev boards which work (I used F429Zi):

NUCLEO-F429ZI
STM32F746G-DISCOVERY

-------------- Installing
Two stm32duino libraries "LwIP" and "STM32Ethernet" are needed. So first you need to have Arduino IDE and STM32duino installed (these two must be versions which work together).

Both libraries are available through the Arduino Libraries manager [Sketch][Include library][Library manager].
Click on "Sketch" menu and then "Include Library > Manage Libraries..." Search "LwIP" then install "LwIP" and "STM32Ethernet"

-------------- Testing
Open and run sketch [File][Examples][STM32duino STM32Ethernet] and select "Web Server'. This should start a simple WEB server on 192.168.1.177 port 80.


-------------- PHY
http://andybrown.me.uk/2012/09/01/ethernet-phy-stm32f107/

hobby project notes 20.9.2018/pekka
