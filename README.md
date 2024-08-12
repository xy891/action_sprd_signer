# Action SPRD image sign tool

Action Workflows to sign image for Unisoc/SPRD device. **WORK IN PROGRESS!**

# Introduce

As I know, two sign method are used by Unisoc. Please choose different signature methods based on your SoC model。

## AVBTOOL METHOD

> [!Caution]
> Avbtool method isn't yet completed. Please sign manually by watching this two tutorial: [1](https://www.hovatek.com/forum/thread-32664.html), [2](https://www.hovatek.com/forum/thread-32674.html)

Avbtool is a tool to read AVB2.0 (or avb1.0 support?) signed image and sign an image. Avbtool method means using avbtool to sign the image. Check [here](https://www.hovatek.com/forum/thread-32664.html) and [here](https://www.hovatek.com/forum/thread-32674.html) if you want to know how does it work.

For example, **SC9832E/SL8541E** uses Android Verified Boot 2.0 to sign and verify the image. It should use avbtool method.

Usually, if your device has vbmeta partition and it was not empty, you should use this method.

If you want to further confirm, you can check the header of your boot image and vbmeta image. The boot image uses common header (`ANDROID!` in the first 8 bytes) but vbmeta uses a different header (`DHTB` in the first 4 bytes) that avbtool can not read it correctly. 

## LEGACY METHOD

This method seems to be used only in old SoCs. Unisoc has their own tool to sign the image.

For example, **SC9820E/SL8521E** uses this method to sign the image. Most of SC9820E devices using Android 4.4. But there're still some devices using Android 8.1.

I ain't sure, but if your device doesn't have vbmeta pertition or it was empty, you may need to use this method.

If you want to further confirm, you can check the header of your boot image. The boot image uses `DHTB` for it's header instead of `ANDROID!` so that bootimg unpacker can't read it correctly. Actually, `ANDROID!` has been moved backwards by 512 bytes.
