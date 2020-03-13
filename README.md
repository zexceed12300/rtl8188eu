# RTL8188EUS Driver
[![Monitor mode](https://img.shields.io/badge/monitor%20mode-supported-brightgreen.svg)](#) [![Frame injection](https://img.shields.io/badge/frame%20injection-supported-brightgreen)](#) [![MESH Mode](https://img.shields.io/badge/mesh%20mode-supported-brightgreen.svg)](#) [![aircrack-ng](https://img.shields.io/badge/aircrack--ng-supported-blue.svg)](#) [![android](https://img.shields.io/badge/android-supported-blue.svg)](#) [![NetHUNTER](https://img.shields.io/badge/NetHUNTER-supported-red.svg)](#) [![LICENSE-GPL](https://img.shields.io/badge/license-GPL--v3.0-orange)](https://github.com/zexceed12300/rtl8188eus/blob/master/LICENSE)

This is Forked From https://github.com/aircrack-ng/rtl8188eus, no changes only bug fixes, this must be the newest, most stable and effective one.

# How To Build For Android/Nethunter
## Preparing
1. Your kernel should enabled below. check in /proc/config.gz
```
CONFIG_MODULES (loadable module support)
CONFIG_MODULE_FORCE_LOAD (forced module loading)
CONFIG_MODULE_UNLOAD (module unloading)
CONFIG_MODULE_FORCE_UNLOAD (forced module loading)
CONFIG_CFG80211_WEXT (wireless extension compability)
CONFIG_MAC80211 (IEEE 802.11 Networking Stack)
```
2. Download your source kernel. (example for Redmi 5: https://github.com/aragon12/android_kernel_xiaomi_msm8953)
## Build Kernel Headers
if you have built the kernel and followed the steps, Let's Start!. 
```
cd kernel-source
mkdir ../kernel-headers
make O=../kernel-headers someone_defconfig
make O=../kernel-headers modules_prepare
```
You can get your kernel defconfig in /proc/config.gz (extracted) and place in [your kernel source directory]/arch/[your device architecture]/configs/ and rename as someone_defconfig.
## Build RTL8188EUS driver/modules
clone this driver repository:
```
cd ../
git clone https://github.com/zexceed12300/rtl8188eus
cd rtl8188eus
```

now follow this command:
```
export ARCH=arm64
export SUBARCH=arm64
export CROSS_COMPILE=aarch64-linux-gnu-
export KLIB_BUILD=../kernel-headers
```
arm64 is my Redmi 5 device architecture. CROSS_COMPILE is toolchain(aarch64-linux-gnu- for arm64, arm-linux-gnueabihf for arm)

and type this command to build this driver: 
```
make
```
if there is no error or success you will see a file named 8188eu.ko in this driver directory. 
## Load the 8188eu.ko
```
su
insmod 8188eu.ko
```
Enjoy!! the driver has been loaded in your device, now you can plugged your wifi adapter(like TP-LINK TL-WN722N v2/v3) now.
## HowTo Monitor mode
Use these steps to enter monitor mode.
```
$ sudo airmon-ng check-kill
$ sudo ip link set <interface> down
$ sudo iw dev <interface> set type monitor
```
Frame injection test may be performed with
(after kernel v5.2 scanning is slow, run a scan or simply an airodump-ng first!)
```
$ aireplay -9 <interface>
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
