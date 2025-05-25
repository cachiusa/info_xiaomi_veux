# initramfs: Device won't boot with ramdisk size above certain threshold
## Priority: medium
## Environment
- Redmi Note 11 Pro 5G
- PixelExperience beta, Android 14
- e+ kernel installed
## Steps to reproduce
This is stock vendor_boot.img from AOSP ROM

With no `ignore_builtin_recovery` cmdline, the device can boot into Android system and recovery mode
```
magiskboot unpack stock_vendor_boot.img

Parsing boot image: [vendor_boot.img]
VENDOR_BOOT_HDR
HEADER_VER      [3]
RAMDISK_SZ      [28572764]
DTB_SZ          [275689]
PAGESIZE        [4096]
NAME            []
CMDLINE         [console=ttyMSM0,115200n8 earlycon=msm_geni_serial,0x04C8C000 androidboot.hardware=qcom androidboot.console=ttyMSM0 androidboot.memcg=1 lpm_levels.sleep_disabled=1 video=vfb:640x400,bpp=32,memsize=3072000 msm_rtb.filter=0x237 service_locator.enable=1 androidboot.usbcontroller=4e00000.dwc3 swiotlb=0 loop.max_part=7 cgroup.memory=nokmem,nosocket iptable_raw.raw_before_defrag=1 ip6table_raw.raw_before_defrag=1 firmware_class.path=/vendor/firmware]
RAMDISK_FMT     [lz4_legacy]
VBMETA
```
e+ kernel has `INITRAMFS_FORCE` enabled, and the device should boot normally regardless of the ramdisk passed by bootloader

We verify it by replacing stock ramdisk with a stub of same size
```
dd if=/dev/zero of=ramdisk.cpio bs=28572764 count=1 iflag=fullblock
magiskboot repack -n stock_vendor_boot.img new-boot.img

Repack to boot image: [new-boot.img]
HEADER_VER      [3]
RAMDISK_SZ      [28572764]
DTB_SZ          [275689]
PAGESIZE        [4096]
NAME            []
CMDLINE         [console=ttyMSM0,115200n8 earlycon=msm_geni_serial,0x04C8C000 androidboot.hardware=qcom androidboot.console=ttyMSM0 androidboot.memcg=1 lpm_levels.sleep_disabled=1 video=vfb:640x400,bpp=32,memsize=3072000 msm_rtb.filter=0x237 service_locator.enable=1 androidboot.usbcontroller=4e00000.dwc3 swiotlb=0 loop.max_part=7 cgroup.memory=nokmem,nosocket iptable_raw.raw_before_defrag=1 ip6table_raw.raw_before_defrag=1 firmware_class.path=/vendor/firmware]

fastboot flash vendor_boot new-boot.img
fastboot reboot
```
However, testing that with a larger ramdisk (34MB) causes the device to panic(?) and fall back to bootloader mode

This means that the kernel can't handle a passed ramdisk above certain size(around 33-34MB).
```
dd if=/dev/zero of=ramdisk.cpio bs=34000000 count=1 iflag=fullblock
magiskboot repack -n stock_vendor_boot.img new-boot.img

Repack to boot image: [new-boot.img]
HEADER_VER      [3]
RAMDISK_SZ      [34000000]
DTB_SZ          [275689]
PAGESIZE        [4096]
NAME            []
CMDLINE         [console=ttyMSM0,115200n8 earlycon=msm_geni_serial,0x04C8C000 androidboot.hardware=qcom androidboot.console=ttyMSM0 androidboot.memcg=1 lpm_levels.sleep_disabled=1 video=vfb:640x400,bpp=32,memsize=3072000 msm_rtb.filter=0x237 service_locator.enable=1 androidboot.usbcontroller=4e00000.dwc3 swiotlb=0 loop.max_part=7 cgroup.memory=nokmem,nosocket iptable_raw.raw_before_defrag=1 ip6table_raw.raw_before_defrag=1 firmware_class.path=/vendor/firmware]

fastboot flash vendor_boot new-boot.img
fastboot reboot
```
This issue is not related to ramdisk compression format(as long as kernel supports it)

## Summary
Some custom recoveries have large ramdisk size, even with lz4 compression. If e+ kernel was built without `INITRAMFS_FORCE`, the device would boot normally. Otherwise, it stucks at bootloader mode

While devs can make effort to reduce ramdisk size, doing so will remove certain features from recovery, yet doesn't utilize the size of the [vendor_]boot image
