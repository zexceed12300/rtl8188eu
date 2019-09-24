
# RTL8188EUS Driver
[![Monitor mode](https://img.shields.io/badge/monitor%20mode-supported-brightgreen.svg)](#) [![Frame Injection](https://img.shields.io/badge/monitor%20mode-supported-brightgreen.svg)](#) [![MESH Mode](https://img.shields.io/badge/mesh%20mode-supported-brightgreen.svg)](#) [![aircrack-ng](https://img.shields.io/badge/aircrack--ng-supported-blue.svg)](#) [![android](https://img.shields.io/badge/android-supported-blue.svg)](#) [![NetHUNTER](https://img.shields.io/badge/NetHUNTER-supported-red.svg)](#)
This is Forked From https://github.com/aircrack-ng/rtl8188eus, no changes only bug fixes, this must be the newest, most stable and effective one.

# How To Build For Android/Nethunter
## Build Kernel
You must first build the kernel, follow this step https://github.com/offensive-security/kali-nethunter/wiki/Modifying-the-Kernel
(as we know there they built a kernel for Galaxy Note 3. but here we will apply it to Redmi 5)

## Build Kernel Headers
if you have built the kernel and followed the steps, Let's Start!. 
```
cd [your kernel source directory]
make module_prepare
make modules_install INSTALL_MOD_PATH=../
```
## Build RTL8188EUS driver/modules
clone this driver repository:
```
cd ../
git clone https://github.com/zexceed12300/rtl8188eus
cd
```
That command places this driver behind your kernel source directory (RECOMMENDED). if you put it anywhere you might need to set the Makefile in this driver, but i won't explain it.

now follow this command:
```
export ARCH=arm64
export SUBARCH=arm64
export CROSS_COMPILE=../toolchain/toolchain64/bin/aarch64-linux-android-
export KBUILD_KVER=3.10.20-KALI-NetHUNTER-moor_n-release
```
arm64 is my Redmi 5 device architecture. CROSS_COMPILE is your toolchain directory(This should have been explained when you built the kernel). KBUILD_KVER is your kernel build version, you can search for it in ../lib/modules (the place of your modules_install when you build kernel headers).


and type this command to build this driver: 
```
make
```
if there is no error or success you will see a file named 8188eu.ko in this driver directory. 
## Create Flashable zip 
Create flashable zip for flash it the kernel with module to your device via twrp

clone this repository for create flashable zip
```
cd /root
git clone https://github.com/nathanchance/AnyKernel2
cd Anykernel2
```
place your kernel image file(zImage or zImage-dtb. You can find it in [your kernel source]/arch/[your device architecture]/boot/ . for my redmi 5, I got it on arch/arm64/boot/) on this Anykernel2 folder. and place 8188eu.ko in Anykernel2/modules/system/lib/modules

open the anykernel2.sh and Change..
```
do.modules=0
do.cleanup=1
do.cleanuponabort=0
device.name1=maguro
device.name2=toro
device.name3=toroplus
```
TO
```
do.modules=1
do.cleanup=1
do.cleanuponabort=0
device.name1=rosy
device.name2=
device.name3=
```
rosy is the code name of my Redmi 5 device, you can search google for your code name. do.modules changes to 1. device.name2= and device.name3= are empty because there is only one codename.

type the following command:
```
zip -r9 kernel-nethunter.zip * -x .git README.md *placeholder
```
now there will be a file with the name kernel-nethunter.zip 
You can flash it now via TWRP or other custom recovery
## Load the 8188eu.ko
if it has been flashed and rebooted. Open the terminal emulator or nethunter terminal and type the following command
```
su
cd /system/lib/modules
insmod 8188eu.ko
```
Enjoy!! the driver has been loaded in your device, now you can plugged your wifi adapter(like TP-LINK TL-WN722N v2/v3) now.
## HowTo Monitor mode
Use these steps to enter monitor mode.
```
airmon-ng check-kill
ip link set <interface> down
iw dev <interface> set type monitor
```
Frame injection test may be performed with
```
aireplay -9 <interface>
```
## Network Manager Configuration
Add these lines below to "NetworkManager.conf" and ADD YOUR ADAPTER MAC below [keyfile] This will make the Network-Manager ignore the device, and therefor don't cause problems.
```
[device]
wifi.scan-rand-mac-address=no

[ifupdown]
managed=false

[connection]
wifi.powersave=0

[main]
plugins=keyfile

[keyfile]
unmanaged-devices=mac:A7:A7:A7:A7:A7
```
