---
title: Enable serial console and login with root
tags:
  - Linux
date: 2016-10-16
---

需要在服务器端启动相应的服务
```
systemctl enable getty@ttyS1.service
```

<!-- more -->

在/etc/securetty文件中添加ttyS1，以使root也能够登录
```
systemctl start getty@ttyS1.service
```