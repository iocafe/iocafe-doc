Resizing virtual machine hard drive
=====================================
It is not unusal to run out of virtual machine disc space. The virtual machine can be hd resized, but it is not exactly simple. To do list:

* Use vmware virtual machine settings to expand the hard drive. This will not change linux partitions.
* Download gparted live ISO image.
* Set this ISO image as cdrom in virtual machine settings.
* Go to virtual machine BIOS settings: Power on the virtual machine and try to click virtual machine window and press and hold F2 key to get to BIOS. This can get real tricky, it has to be real fast just when vmware virtual machine pops up, to get the window focused and F2 recognized. Any warnings or error messages by vmware at this point are a challenge. Some player version may have "power to firmware", etc, choice to make this easier.
* Change CD to be before HD in boot order.
* Let virtual machine boot and now you should be in live CD. Hit enter to all terminal questions until gparted GUI opens.
* Move swap partition forward to end of disc. Apply changes.
* Resize the root partition to take all possible space left before swap. Apply changes.
* Close gparted, restart virtual machine to BIOS and restore HD boot to be before CD.


210309, updated 23.9.2020/pekka

