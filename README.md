# RTL8188EUS Driver
[![Monitor mode](https://img.shields.io/badge/monitor%20mode-supported-brightgreen.svg)](#) [![Frame injection](https://img.shields.io/badge/frame%20injection-supported-brightgreen)](#) [![MESH Mode](https://img.shields.io/badge/mesh%20mode-supported-brightgreen.svg)](#) [![aircrack-ng](https://img.shields.io/badge/aircrack--ng-supported-blue.svg)](#) [![android](https://img.shields.io/badge/android-supported-blue.svg)](#) [![NetHUNTER](https://img.shields.io/badge/NetHUNTER-supported-red.svg)](#) [![LICENSE-GPL](https://img.shields.io/badge/license-GPL--v3.0-orange)](https://github.com/zexceed12300/rtl8188eus/blob/master/LICENSE)

This is forked From https://github.com/aircrack-ng/rtl8188eus, this including bug fixes, error fix and other patch. This must be the newest, most stable and effective one.

# How To Build For Android/Nethunter
## 1. Compile your own kernel
Download your device source kernel. (example for Redmi 5: https://github.com/zexceed12300/android_kernel_xiaomi_rosy-3.18) & Download GCC toolchain (example for Google GCC 4.9).
```
git clone https://github.com/zexceed12300/android_kernel_xiaomi_rosy-3.18
git clone https://android.googlesource.com/platform/prebuilts/gcc/linux-x86/aarch64/aarch64-linux-android-4.9 -b ndk-release-r16
```
### Prepare for compiling kernel
```
cd android_kernel_xiaomi_rosy-3.18
export ARCH=arm64
export CROSS_COMPILE=../aarch64-linux-android-4.9/bin/aarch64-linux-android-
mkdir ../out
make O=../out someone_defconfig
```
edit ../out/.config and make sure these config has already enabled :
```
CONFIG_MODULES=y
CONFIG_MODULE_FORCE_LOAD=y
CONFIG_MODULE_UNLOAD=y
CONFIG_MODULE_FORCE_UNLOAD=y
CONFIG_CFG80211_WEXT=y
CONFIG_MAC80211=y

If not enabled already, add to your defconfig.
```
### Compiling kernel
```
make O=../out -j$(nproc)
```
now your Kernel file(Image.gz-dtb or Image) is in android_kernel_xiaomi_rosy-3.18/arch/arm64/boot/ and flash it to your device.
## 2. Build RTL8188EUS driver/modules
#### Download driver RTL8188EUS
```
cd ../
git clone https://github.com/zexceed12300/rtl8188eus
cd rtl8188eus
```
#### Compile all module 
```
export KLIB_BUILD=../out
make
```
If there is no error or success, you will see a file named 8188eu.ko in this driver directory. 
## 3. Load the Driver
move 8188eu.ko to your device. open terminal and type
```
su
insmod 8188eu.ko
```
Enjoy!! the driver has been loaded in your device, now you can plug your wifi adapter(like TP-LINK TL-WN722N v2/v3) in now.
## How To Enable Monitor mode
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
