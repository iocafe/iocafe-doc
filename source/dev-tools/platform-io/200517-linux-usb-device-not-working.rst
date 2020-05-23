Issue: Platform IO cannot use USB device
========================================

Symphtoms
*********
ESP-prog or other USB connected device cannot be used from linux trough USB

Solution
********
Linux users have to install udev rules for PlatformIO supported boards/devices. The latest version of rules may be found at https://raw.githubusercontent.com/platformio/platformio-core/master/scripts/99-platformio-udev.rules

Note:
Please check that your board’s PID and VID are listed in the rules. You can list connected devices and their PID/VID using platformio device list command.
This file must be placed at /etc/udev/rules.d/99-platformio-udev.rules (preferred location) or /lib/udev/rules.d/99-platformio-udev.rules (required on some systems).

