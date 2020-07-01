ESP32 OTA - software updates
==============================

The OTA update mechanism allows a device to update itself over IOCOM connection while the normal firmware is running.

OTA requires configuring the Partition Table of the device with at least two “OTA app slot” partitions (ieota_0 and 
ota_1) and an “OTA Data Partition”.
The OTA operation functions write a new app firmware image to whichever OTA app slot is not currently being used for
booting. Once the image is verified, the OTA Data partition is updated to specify that this image should be used for 
the next boot.

Links

* https://docs.espressif.com/projects/esp-idf/en/latest/api-reference/system/ota.html
* https://github.com/scottchiefbaker/ESP-WebOTA/tree/master/src

