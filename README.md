
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
