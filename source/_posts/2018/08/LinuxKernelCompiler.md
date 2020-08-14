---
title: Linux Kernel Compiler
tag:
  - Linux
date: 2018-08-05
---

To compiler linux kernel , we can get and download full source from [www.kernel.org](www.kernel.org), choose the same version kernel with your linux .

<!-- more -->

Check you own linux kernel version:
```
$ uname -r
3.0.XX..
```

In the commond line , can use curl to download the resource
```
$ cd ~
$ curl www.kernel.org/pub/linux/kernel/v3.0/linux-3.0.56.tar.bz2 > linux3.0.tar.bz2
```
Then uncompress the resource file:
```
$ tar -jxvf linux3.0.tar.bz
$ cd linux3.0.tar.bz
```
Delete the *.o file use commond below
```
$ make mrproper
```
Then we can choose which content or module can be compiler freely:
```
$ make menuconfig
```
If can't use the 'make menuconfig' it need to install a library ncruses-devel
```
$ apt-get install ncruses-dev
```
After config the content and module which will be compilered , it's time to compiler it:
```
$ make clean
$ make bzImage
$ make modules
```
The 'make bzImage' and 'make modules' will spend lots of time , please pay attention and wait for a minute .

Finish compiler kernel , move file to boot directory:
```
$ cp arch/i386/boot/bzImage /boot/linux3.0-56
$ cp System.map /boot/System.map3.0-56
```
Then add grub config to grub.cfg or menu.lst file
```
menuentry 'Ubuntu, with Linux 3.0.0-56-generic' --class ubuntu --class gnu-l    inux --class gnu --class os {
         recordfail
         set gfxpayload=$linux_gfx_mode
         insmod gzio
         insmod part_msdos
         insmod ext2
         set root='(hd0,msdos10)'
         search --no-floppy --fs-uuid --set=root c3a2f48c-75b5-45c8-aabe-1566    a89e1cb3
         linux   /boot/linux3.0-56 root=UUID=c3a2f48c-75b5-45c8-aabe-1566a89e    1cb3 ro   quiet splash vt.handoff=7
 }
```
reboot the system