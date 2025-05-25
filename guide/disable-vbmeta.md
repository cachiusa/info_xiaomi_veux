# Disable VBMeta
- Get vbmeta.img from [here](https://sourceforge.net/projects/recovery-veux/files/vbmeta.img/download)
- Run this command
```
fastboot flash vbmeta --disable-verity --disable-verification vbmeta.img
```