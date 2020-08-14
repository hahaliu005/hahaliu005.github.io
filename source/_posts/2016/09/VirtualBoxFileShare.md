---
title: VirtualBox 设置共享文件夹 (从mac共享至ubuntu/centos)
tags:
  - MacOS
  - VirtualBox
date: 2016-09-08
---

虚拟机安装好后, 在此地址上可以找到对应版本的, VBoxGuestAdditions 的ISO文件, http://download.virtualbox.org/virtualbox/, 如, 当前使用的版本为5.1.4版本, 则下载地址为: http://download.virtualbox.org/virtualbox/5.1.4/VBoxGuestAdditions_5.1.4.iso, 将此iso文件下载至虚拟机中
wget http://download.virtualbox.org/virtualbox/5.1.4/VBoxGuestAdditions_5.1.4.iso

<!-- more -->

将ISO文件挂载至CDROM中:
mkdir /mnt/cdrom
sudo mount -o loop /usr/share/virtualbox/VBoxGuestAdditions.iso  /mnt/cdrom

安装一此依赖项:
yum groupinstall -y 'Development Tools'
yum install -y kernel-devel-`uname -r`


运行其中的安装文件:
sudo /mnt/cdrom/VBoxLinuxAdditions.run


Settings -> Shared Folders -> add user shared folder
配置好共享文件夹相关配置: 其中 Auto-mount, Make Perment打上勾

重启虚拟机

df -h  命令应该就能看到挂载的盘符了
应该使用mount配置正确的用户与用户组权限, 如下:
sudo mount -t vboxsf -o uid=[user_id],gid=[group_id] [shared name] [mount to directory]

sudo mount -t vboxsf -o uid=1000,gid=1000 code ~/Public/code


