---
title: SSH Issues
tags:
  - SSH
  - Linux
date: 2016-11-15
---

ssh配置中添加如下的配置：
```
cat ~/.ssh/config
Host git-dev
  HostName 192.168.1.1
  Port 1234
  User git
```

重新设置git的remote url 即可：
```
git remote set-url origin git@git-dev:dev/test.git
```
