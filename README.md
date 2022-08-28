# no-bootable-device
Bypass stupid microsoft bug in surface go and surface go2

Recently there have been cases of surface devices reporting "No bootable device" on boot.
Despite attempts to reinstall the operating system (fortunately booting via usb remains functional), there is no way to get the surface to boot from its emmc memory. This is because the efi firmware reports the internal emmc as a non-bootable device (like an sd card).

After numerous attempts, I found a way to boot the tablet directly from internal storage. The only problem is that this guide will only allow you to boot Linux systems (Ubuntu in particular), but it's still better than a bricked tablet!

## about the technique
The technique I have chosen is called chain loading, and consists of first loading a linux kernel via usb, which will contain the drivers necessary to read the emmc (which the efi firmware considers instead not bootable). After that, the kernel contained in the USB will boot the kernel contained in the emmc.
Trust me, it's a lot simpler than it sounds.

## result you will get
You can start the tablet with a USB and then remove it immediately afterwards, and the tablet will load the operating system from the internal memory.
## what you need
- 2 USB drive, one will be used for a live Ubuntu 22.04 distro, the other will be used as a bootloader to actually boot the installed operating system.
- one USB to USB-C adapter (to connect USB to the surface)

## Prepare first USB drive (The bootloader)
- Format the first USB drive as FAT32
- Copy the EFI folder contained in the bootloader.zip (download from this page) in the newly created partition
![alt text](https://github.com/fmorea/no-bootable-device/blob/main/tutorial.png?raw=true)


## Prepare second USB drive (The bootable live enviroment)
- Download ubuntu 22.04 iso 
- write this iso onto the usb with rufus ([download rufus](https://rufus.ie/it/))

## Disable secure boot in the tablet
- power key and volume up button to boot into UEFI bios
- disable secure boot

## System installation
- Boot the Ubuntu USB (do not worry if you see the ubuntu logo for long time)
- Click on "Try Ubuntu"
- Connect to wifi
- Open a terminal (ctrl + alt + t)
- copy and paste one by one these commands and push enter key
```
sudo apt update
sudo apt --yes install git
git clone https://github.com/fmorea/no-bootable-device.git ~/temp
cd ~/temp
chmod +x install_ubuntu.sh
sudo ./install_ubuntu.sh initial
```
- now you have to choose where to install the sistem, select 1, the mmc-DA4064 (our internal emmc)
- wait until installation is finished
- power off the tablet (if necessary by pressing and holding the power button)
- connect the Bootloader usb to the tablet
- boot the tablet, the 10 second countdown should start and then you can disconnect the USB
- follow the instructions on the screen to choose username, password, language, yada yada yada ...
You may need to press the enter key if the "next" button is not displayed. That's all.


## Upgrading the kernel to linux-surface (to install drivers)
```

sudo add-apt-repository ppa:jonathonf/zfs
sudo apt update
sudo apt install zfs-dkms zfs-auto-snapshot zfs-initramfs
sudo update-initramfs -k all -c
```
- reboot the device
```
wget -qO - https://raw.githubusercontent.com/linux-surface/linux-surface/master/pkg/keys/surface.asc \
    | gpg --dearmor | sudo dd of=/etc/apt/trusted.gpg.d/linux-surface.gpg
echo "deb [arch=amd64] https://pkg.surfacelinux.com/debian release main" \
	| sudo tee /etc/apt/sources.list.d/linux-surface.list
sudo apt update
sudo apt install linux-image-surface linux-headers-surface iptsd libwacom-surface
sudo zpool set multihost=on rpool
sudo zpool set multihost=off rpool
sudo rm /etc/hostid
sudo generate-zbm

```
- reboot the device
```
sudo systemctl enable iptsd
```



