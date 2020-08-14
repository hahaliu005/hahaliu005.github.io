---
title: VMware fusion share folder to ubuntu
tags:
  - VMware
  - Ubuntu
  - Linux
date: 2016-03-07
---

确保在ubuntu虚拟机中安装了以下两个工具
```
sudo apt-get install open-vm-tools
sudo apt-get install open-vm-tools-dev
```

在vmware菜单栏中设置需要共享的文件夹, 为确保生效可重启一下

以下命令可以看到共享的文件名
```
vmware-hgfsclient
```

使用以下命令设置共享文件夹挂载至何处, 并设置所属用户ID与用户组ID
```
vmhgfs-fuse  ~/Public -o uid=1000 -o gid=1000
```