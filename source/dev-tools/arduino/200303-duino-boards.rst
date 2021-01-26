"Duino" variants and boards
===================================

* Original Arduino boards generally work well, and software is polished. Downside often is very limited resources and capability. Performance on more powerful versions of actual Arduino is being tested. Even it is possible to load iocom in very minimal serial communication configuration to UNO with 2kB of RAM and 32kB flash, something a bit more powerful is preferrable. The iocom alone eats up most of UNO's RAM and flash, leaving little for anything else.
* ESP32 is the low cost IoT device of today. It features inbuilt WiFi, two cores and plentiful resources. In practice the chip is so complex, that it cannot be used as metal: The Espressif operating system including FreeRTOS is a must. Low ADC top speed of below 100kHz limits use for some applications.
* Teensy 3.6 has really good performance and format that it can be plugged directly into custom PCB. Unfortunately has two minuses: lack of debugging support and no real Ethernet support. Teensy 4.1 has Ethernet (to be tested).
* STM32F429, etc, Nucleo, on the other hand shine where Teensy lacks, but have a downside when it comes to development boards: Nucleo cannot legally be reused “as is” as part of commercial project: You have to draw your own PCB and put the STM chip on it. STM gives you two good choices for development tools, TrueStudio or STMduino + PlatformIO.

200303, updated 26.1.2021/pekka
