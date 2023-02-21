# KernelSU Action

Action for Non-GKI Kernel has some common and requires knowledge of kernel and Android to be used.

## Warning:warning: :warning: :warning:

If you are not a kernel author and use someone else's source code to build KernelSU, please use it for your own use only and do not share it with others, it is respectful to the author.

## Support Kernel

- `4.19`
- `4.14`

## Usage

> After successful build, it will upload AnyKernel3 in `Action`, which has turned off device check, please flash to phone in Twrp.

First fork this repository to your repository and edit config.env as follows, then click `Star` or `Action`, you will see `Build Kernel` option on the left side, click it and you will see `Run workflows` on the top of the big dialog box on the right side, click it and it will start the build.

### Kernel Source

Type your kernel link

e.g. https://github.com/Diva-Room/Miku_kernel_xiaomi_wayne

### Kernel Source Branch

Type your kernel branch

e.g. TDA

### Kernel defconfig

Type your kernel defconfig

e.g. vendor/wayne_defconfig

### Target arch

e.g. arm64

### Kernel file

Type in the image you need, usually the same as BOARD_KERNEL_IMAGE_NAME in your aosp-device tree.

e.g. Image.gz-dtb

### Clang version

Type the version of Clang you need to use
| Clang version | Corresponding Android version | AOSP-Clang version |
| ------------- | ----------------------------- | ------------------ |
| 12.0.5 | Android S | r416183b |
| 14.0.6 | Android T | r450784d |
| 14.0.7 | | r450784e |
| 15.0.1 | | r458507 |

Usually Clang12 will pass most kernel builds of 4.14 and above.
My own MI 6X 4.19 is using r450784d.

### Extra build commands

Some kernels require some build commands to be typed in manually in order to build properly, so please don't type them in if you don't need to.
Separate commands with spaces.

e.g. LLVM=1 LLVM_IAS=1

### Disable LTO

This is used to optimize the kernel, but sometimes it causes errors, so it is provided that it can be disabled, set to true to disable.

### Use KernelSU

For debug kernel or build it separately

### Use Kprobes

If your kernel Kprobes is working properly, changing this to "true" will automatically add the parameter to defconfig.

### Use overlayfs

Please enable this parameter if the kernel does not have it, the module requires

### Need DTBO

If your kernel also needs to be flashed with DTBO image, set it to true.

### Make boot image
> Merge from build_boot_image.yml

Set to true, boot.img will be Make, you need to provide `Source boot image`

### Source boot image

Provide a boot image that will boot successfully, with the same kernel source code and the same device tree as your current system built from aosp, the ramdisk contains the partition table and init, without which it may reboot to fastboot.

e.g. https://raw.githubusercontent.com/xiaoleGun/KernelSU_action/main/boot/boot-wayne-from-Miku-UI-latest.img

## Credits

- [AnyKernel3](https://github.com/osm0sis/AnyKernel3)
- [AOSP](https://android.googlesource.com)
- [KernelSU](https://github.com/tiann/KernelSU)
- [xiaoxindada](https://github.com/xiaoxindada)
