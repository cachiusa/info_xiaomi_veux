# Restore boot only
```
fastboot flash boot boot.img
fastboot flash vendor_boot vendor_boot.img
fastboot flash dtbo dtbo.img
fastboot reboot recovery
```
You can use this method to:
- repair a soft-brick
- uninstall custom recovery, custom kernel, root
- revert to original ROM state
- install a different ROM

## Requirements
`boot.img, vendor_boot.img, dtbo.img` must be extracted from your current ROM, or the ROM you are about to install
- For MIUI/HyperOS (fastboot installer only), look inside `images` directory
- For AOSP, your maintainer should provide them.
    - If not, use [payload-dumper-go](https://github.com/ssut/payload-dumper-go) to extract these from the OTA package
- For ported/modded ROM, your maintainer should provide them
- Only use [LineageOS Recovery](https://sourceforge.net/projects/recovery-veux/files/lineage-recovery-20250112-veux.zip/download) as a last resort
