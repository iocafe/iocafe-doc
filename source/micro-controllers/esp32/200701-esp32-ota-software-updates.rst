ESP32 OTA - software updates
==============================

The OTA update mechanism allows a device to update itself over IOCOM connection while the normal firmware is running.

OTA requires configuring the Partition Table of the device with at least two “OTA app slot” partitions (ieota_0 and 
ota_1) and an “OTA Data Partition”.
The OTA operation functions write a new app firmware image to whichever OTA app slot is not currently being used for
booting. Once the image is verified, the OTA Data partition is updated to specify that this image should be used for 
the next boot.


Note 3.7.2020: To enable rollback staging version of arduino-esp32.git needs to be installed. 

::

    platform_packages =
        framework-arduinoespressif32 @ https://github.com/espressif/arduino-esp32.git

The staging bestion needs that it needs "SimpleBLE" listen in platformio.ini lib_deps.

::  
    lib_deps =
        eosal_jpeg
        WiFi
        EEPROM
        SimpleBLE

Links

* https://docs.espressif.com/projects/esp-idf/en/latest/api-reference/system/ota.html
* https://github.com/scottchiefbaker/ESP-WebOTA/tree/master/src
* https://docs.platformio.org/en/latest/platforms/espressif32.html#using-arduino-framework-with-staging-version
