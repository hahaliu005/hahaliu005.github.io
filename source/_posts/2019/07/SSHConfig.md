---
title: SSH config
tags: 
  - SSH
  - Linux
date: 2019-07-29
---

Config SSH with proxy in ~/.ssh/config
```
Host dev
  ProxyCommand nc -X 5 -x 127.0.0.1:1080 %h %p
  HostName 127.0.0.1
  Port 22
  User root
```

<!-- more -->