# Fix stuck in bootloader mode (screen showing `FASTBOOT`) (veux only)

You need a PC to with [latest](https://developer.android.com/tools/releases/platform-tools) `fastboot` and `adb` installed

Your bootloader must be unlocked. If not, it's bricked

Try each of these, from top to bottom:

## 1 - Run `fastboot reboot`
If this didn't fix, try (2)

## 2 - Reset active slot
1. Get your current slot
```
fastboot getvar active-slot
```
2. Run this command. Replace `<active-slot>` with your current slot name
```
fastboot set_active <active-slot>
fastboot reboot
```
If this didn't fix, try (3)

## 3 - [Restore boot](./restore-boot.md)
If this didn't fix, try (4)

## 4 - [Restore to MIUI/HyperOS](./flash-stock.md)
If this didn't fix, your phone is bricked.