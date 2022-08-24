# no-bootable-device
Bypass stupid microsoft bug in surface go and surface go2

Recently there have been cases of surface devices reporting "No bootable device" on boot.
Despite attempts to reinstall the operating system (fortunately booting via usb remains functional), there is no way to get the surface to boot from its emmc memory. This is because the efi firmware reports the internal emmc as a non-bootable device (like an sd card).

After numerous attempts, I found a way to boot the tablet directly from internal storage. The only problem is that this guide will only allow you to boot Linux systems (Ubuntu in particular), but it's still better than a bricked tablet!

## about the technique
The technique I have chosen is called chain loading, and consists of first loading a linux kernel via usb, which will contain the drivers necessary to read the emmc (which the efi firmware considers instead not bootable). After that, the kernel contained in the USB will boot the kernel contained in the emmc.
Trust me, it's a lot simpler than it sounds.

## result you will get
You can start the tablet with a key and then remove it immediately afterwards

## what you need
- 2 USB drive, one will be used for a live Ubuntu 22.04 distro, the other will be used as a bootloader to actually boot the installed operating system.
- one USB to USB-C adapter (to connect USB to the surface)

## what you need
- 2 USB drive, one will be used for a live Ubuntu 22.04 distro, the other will be used as a bootloader to actually boot the installed operating system.
- one USB to USB-C adapter (to connect USB to the surface)

# Prepare first USB drive (The bootloader)
- Format the first drive as FAT32
- Copy the BOOT folder in the newly created partition

## Prepare second USB drive (The bootable live enviroment)
- Download ubuntu 22.04 iso 
- write this iso onto the usb with rufus ([download rufus](https://rufus.ie/it/))

## Disable secure boot in the tablet
- power key and volume up button to boot into UEFI bios
- disable secure boot

## System installation
- Boot the Ubuntu USB (do not worry if you see the ubuntu logo for long time)
- Click on "Try Ubuntu"
- Open a terminal (ctrl + alt + t)
-//todo


