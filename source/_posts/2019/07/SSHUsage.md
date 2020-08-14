---
title: SSH Usage
tags: 
  - SSH
  - Linux
date: 2019-07-29
---

### Config SSH with proxy
~/.ssh/config
```
Host dev
  ProxyCommand nc -X 5 -x 127.0.0.1:1080 %h %p
  HostName 127.0.0.1
  Port 22
  User root
```
<!-- more -->

### 快速添加Ssh-key
```
ssh-copy-id node01
```
按照提示操作后, 就可以直接方便地连接到node01了