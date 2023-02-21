**中文** | [English](README_EN.md)

# KernelSU Action
用于Non-GKI Kernel的Action，具有一定的普遍性，需要了解内核及Android的相关知识得以运用。

## 支持内核
- `4.19`
- `4.14`
## 使用
Fork本仓库到你的储存库然后点击`Action`，在左侧可看见`Build Kernel Common`/`Build boot image`选项，点击选项会看见右边的大对话框的上面会有`Run workflows`，里面有需要你填写的配置，看下面的部分，了解如何填写。

或者使用config.env，编辑config.env然后提交，按star或者run workflows，这个功能是方便手机修改参数。
### Build Kernel
编译成功后，会在`Action`上传AnyKernel3，已经关闭设备检查，请在Twrp刷入
#### Kernel Source
填写你的内核仓库地址

例如: https://github.com/Diva-Room/Miku_kernel_xiaomi_wayne
#### Branch
填写你的内核分支

例如: TDA
#### Kernel defconfig
填写你的内核配置文件名

例如: vendor/wayne_defconfig
#### Kernel file
填写需要刷写的image，一般与你的aosp-device tree里的BOARD_KERNEL_IMAGE_NAME是一致的

例如: Image.gz-dtb
#### Clang version
填写需要使用的Clang版本
| Clang 版本 | 对应 Android 版本 | AOSP-Clang 版本 |
| ---------- | ----------------- | --------------- |
| 12.0.5     | Android S         | r416183b        |
| 14.0.6     | Android T         | r450784d        |
| 14.0.7     |                   | r450784e        |
| 15.0.1     |                   | r458507         |

一般Clang12就能通过大部分4.14及以上的内核的编译
我自己的MI 6X 4.19使用的是r450784d
#### Extra build commands
有的内核需要手动加入一些编译命令，才能正常编译，不需要的话不填写即可
请在命令与命令之间用空格隔开

例如: LLVM=1 LLVM_IAS=1
#### Kprobes
如果你的内核Kprobes工作正常这项改成true即可自动在defconfig注入参数
### Build boot image
编译成功后，会在`Action`上传boot-su.img，使用fastboot刷入到手机
#### Kernel Source
填写你的内核仓库地址

例如: https://github.com/Diva-Room/Miku_kernel_xiaomi_wayne
#### Branch
填写你的内核分支

例如: TDA
#### Kernel Build Config
填写你的内核构建配置文件，需要直链

例如: https://raw.githubusercontent.com/xiaoleGun/KernelSU_action/main/configs/build.config.wayne

里面要改的不多，下面是build config和Device Tree(设备树)里的BoardConfig/BoardConfigCommon的对照
| build config              | BoardConfig/BoardConfigCommon |
| ------------------------- | ----------------------------- |
| DEFCONFIG                 | TARGET_KERNEL_CONFIG          |
| BOOT_IMAGE_HEADER_VERSION | BOARD_BOOT_HEADER_VERSION     |
| BASE_ADDRESS              | BOARD_KERNEL_BASE             |
| PAGE_SIZE                 | BOARD_KERNEL_PAGESIZE         |
| KERNEL_CMDLINE            | BOARD_KERNEL_CMDLINE          |
| MKBOOTIMG_EXTRA_ARGS      | BOARD_MKBOOTIMG_ARGS          |
| KERNEL_BINARY             | BOARD_KERNEL_IMAGE_NAME       |

下面是一些build config里面的选项的用途
| build config          | 作用                               |
| --------------------- | -----------------------------------|
| VENDOR_RAMDISK_BINARY | ramdisk 路径                        |
| ARCH                  | 架构 arm/arm64/x86_64               |
| BUILD_BOOT_IMG        | 为 1 时创建 boot.img                 |
| SKIP_VENDOR_BOOT      | 为 1 时跳过创建 vendor_boot          |
| FILES                 | 需要输出的文件                       |
| CLANG_VERSION         | 我自定义的选项，用于定义 clang 版本     |

剩下的就是杂七杂八的编译器需要，更多请参见[build/build.sh](https://android.googlesource.com/kernel/build/+/refs/heads/master-kernel-build-2022/build.sh)的注释
#### Boot image to get ramdisk
故名思义，提供一个可以正常开机的boot镜像，需要直链，最好是同一套内核源码以及与你当前系统同一套设备树从aosp构建出来的。ramdisk里面包含分区表以及init，没有的话可能会重启到fastboot。

例如: https://raw.githubusercontent.com/xiaoleGun/KernelSU_action/main/boot/boot-wayne-from-Miku-UI-latest.img

## 感谢
- [AnyKernel3](https://github.com/osm0sis/AnyKernel3)
- [AOSP](https://android.googlesource.com)
- [KernelSU](https://github.com/tiann/KernelSU)
