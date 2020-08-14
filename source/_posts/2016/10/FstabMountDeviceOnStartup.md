---
title: Fstab mount device on startup
tags:
  - Linux
date: 2016-10-31
---

Set device to ext4
mkfs.ext4 /dev/sda1

mount device
mount /dev/sda1 /mnt/test

mount device on startup
cat /etc/fstab
/dev/sda1 /mnt/test ext4 defaults 0 0