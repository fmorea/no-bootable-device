# no-bootable-device
Bypass stupid microsoft bug in surface go and surface go2

Recently there have been cases of surface devices reporting "No bootable device" on boot.
Despite attempts to reinstall the operating system (fortunately booting via usb remains functional), there is no way to get the surface to boot from its emmc memory. This is because the efi firmware reports the internal emmc as a non-bootable device (like an sd card).

After numerous attempts, I found a way to boot the tablet directly from internal storage. The only problem is that this guide will only allow you to boot Linux systems (Ubuntu in particular), but it's still better than a bricked tablet!
