"Duino" variants and boards
===================================

* Original Arduino boards generally work well, and software is polished. Downside often is very limited resources and capability.
* ESP32 is The low cost IoT device of today. It features inbuilt WiFi, two cores and plentiful resources.   In practice the chip is so complex, that it cannot be used as metal: The Espressif operating system   including FreeRTOS is a must. Noticing low ADC top speed of approximately 100kHz limits use for some applications.
* Teensy 3.6 has really good performance and format that it can be plugged directly into my own PCB.  Unfortunately has two big minuses: lack of debugging support and incomplete LWIP library port.
* STM32F429, etc, Nucleo, on the other hand shines where Teensy lack, but it also has a downside when it comes to development boards: Nucleo cannot legally be reused “as is” as part of commercial project:
  You have to draw your own PCB and put the STM chip on it, starting already from first customer tests alpha version. STM gives you two good coices for development tools, TrueStudio or Arduino based platformio.
* Performance on more powerful versions of actual Arduino is being tested. Even it is possible to load iocom in very minimal serial communication configuration to UNO with 2kB of RAM and 32kB flash, this is recommended. The iocom alone eats up practically all RAM and flash, leaving very little for anything else.

200303, updated 25.1.2021/pekka
