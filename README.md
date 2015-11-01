debian-mini-odroid-c1
=====================

Script to build a minimal Debian sd/eMMC card image.

## Features:
* Supports building Wheezy or Jessie (default) images (specify using the DIST variable)
* SSH login: odroid/odroid
* Host name: odroidc1-MACADDRESS (e.g. odroidc1-1a2b3c4d5e6f)
* SSH host keys are generated and saved permanently on first boot
* Automatic mounting of USB storage devices using usbmount
* Automatic resize on first boot (It will reboot 2 times to make sure)

## Prerequisites:
On a x86 based Debian system, make sure the following packages are installed:
```
sudo apt-get install build-essential wget git lzop u-boot-tools binfmt-support \
                     qemu qemu-user-static debootstrap parted dosfstools \
                     binutils-arm-none-eabi gcc-arm-none-eabi \
                     binutils-arm-linux-gnueabihf gcc-arm-linux-gnueabihf \
                     g++-arm-linux-gnueabihf
```

If you are running 64 bit Ubuntu, you might need to run the following commands to be able to launch the 32 bit toolchain:
```
sudo dpkg --add-architecture i386
sudo apt-get update
sudo apt-get install libc6:i386 libncurses5:i386 libstdc++6:i386 lib32z1
```

## Build the image:
Just use the make utility to build e.g. an debian-jessie-odroid-c1.img.  Be sure to run this with sudo, as root privileges are required to mount the image.
```
sudo make DIST=jessie IMAGE_MB=1024
```

This will install the toolchains, compile u-boot, the kernel, bootstrap Debian and create a 1024mb sdcard-jessie.img file, which then can be transferred to a sd card (e.g. using dd):
```
sudo dd bs=1M if=debian-jessie-odroid-c1.img of=/dev/YOUR_SD_CARD && sync
```

## Customize your image:
It should be fairly easy to customize your image for your own needs.  You can add package names to `packages.txt`, drop scripts into the `postinst` folder and add patches to the `patches` folder, as well as add any files you want as part of the root file system into the `files` folder.  This should allow you install extra packages (e.g. using apt-get) and modify configurations to your needs.  Of course, you can do all this manually after booting the device, but the goal of this project is to be able to generate re-usable images that can be deployed on any number of ODROID-C1 devices (think of it as "firmware" of a consumer device).
