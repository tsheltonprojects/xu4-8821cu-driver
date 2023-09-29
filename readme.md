#Compiling a driver for android (lineage 14.1 - voodik image)

1) Get the main android repo Example: https://github.com/tsheltonprojects/android/tree/driver_compiler the most important things are the kernel source code and the prebuilts, in  this case 32 bit arm with eabi gcc compile. delete all non relavent projects

2) run repo init, repo sync like the readme.md says

3) the kernal source tree will exist in `kernel/hardkernel/odroidxu3/` the compiler exits in `prebuilts/gcc/linux-x86/arm/` take not of these two directories....

4) copy the kernel config to `kernel/hardkernel/odroidxu3/arch/arm/configs/odroidxu3_defconfig` to    `kernel/hardkernel/odroidxu3/.config`

5) Modify .config to match the EXACT version on the target system. e.g.
CONFIG_LOCALVERSION="-g41ed8ac57acb-dirty"
CONFIG_LOCALVERSION_AUTO=n
discover the exact version string using `sudo modinfo [kernel module]` you'll have to copy a module off the compiled image to find the string

5) run
LOCALVERSION="" CROSS_COMPILE=/build/prebuilts/gcc/linux-x86/arm/arm-linux-androideabi-4.9/bin/arm-linux-androideabi-  ARCH=arm TARGET=arm-linux-androideabi PATH="/build/prebuilts/gcc/linux-x86/arm/arm-linux-androideabi-4.9/bin:$PATH" make
 allow it run for a couple minutes, it doesn't need to finish. after a while cancel the build

6) clone the wifi driver repo `git clone https://github.com/brektrou/rtl8821CU` modify the Makefile to force it to compile 32 bit arm (or whatever it is you need, tell it where the kernal source is. example: https://github.com/tsheltonprojects/rtl8821CU/tree/xu4

7) run
LOCALVERSION="" CONFIG_TRAVIS=y  PREFIX=/build/rtl8821CU_build/ CROSS_COMPILE=/build/prebuilts/gcc/linux-x86/arm/arm-linux-androideabi-4.9/bin/arm-linux-androideabi-  ARCH=arm TARGET=arm-linux-androideabi PATH="/build/prebuilts/gcc/linux-x86/arm/arm-linux-androideabi-4.9/bin:$PATH" make   -j 8
NOTE:if needed removed soft-float from `kernel/hardkernel/odroidxu/arch/arm/Makefile`

USB MODE SWITCH
in order to change usb modes, these commnads can be used

#manual std-eject command to USB for a usb connected cdrom
usb_modeswitch -v 0bda -p 1a2b  -M "5553424387654321000000000000061e000000000000000000000000000000"  -2 "5553424397654321000000000000061b000000020000000000000000000000"
#usb_modeswitch -v 0bda -p 1a2b  -M "5553424387654321000000000000061e000000000000000000000000000000" -2 "5553424397654321000000000000061b000000020000000000000000000000" -3 "5553424387654321000000000001061e000000000000000000000000000000" #-4 "5553424397654321000000000001061b000000020000000000000000000000"
usb_modeswitch -v 0bda -p 1a2b  -M "5553424387654321000000000000061e000000000000000000000000000000"
usb_modeswitch -v 0bda -p 1a2b  -M "5553424397654321000000000000061b000000020000000000000000000000"
usb_modeswitch -v 0bda -p 1a2b  -M "5553424387654321000000000001061e000000000000000000000000000000"
usb_modeswitch -v 0bda -p 1a2b  -M "5553424397654321000000000001061b000000020000000000000000000000"


