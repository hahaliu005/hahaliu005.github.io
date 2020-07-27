---
title: Install VirtualBox
tags:
  - VirtualBox
date: 2018-10-20
---

```
wget https://download.virtualbox.org/virtualbox/rpm/el/virtualbox.repo -P /etc/yum.repos.d/
```

<!-- more -->

Install Development Tools
```
yum install -y 'Development Tools'
```

Maybe need to install kernel-devel
```
yum install -y kernel-devel
```

If the kernel is updated , we need reboot so that vbox can get right kernel header version
```
yum update -y
reboot
```

```
yum search virtualbox -y
yum install VirtualBox-5.2 -y
```