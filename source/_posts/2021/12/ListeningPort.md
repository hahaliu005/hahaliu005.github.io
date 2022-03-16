---
title: Get Application Listening Port
tags:
  - Linux
date: 2021-12-22
---

Run any one of the following command on Linux to see open ports:
```
lsof -i -P -n | grep LISTEN
netstat -tulpn | grep LISTEN
ss -tulpn | grep LISTEN
lsof -i:22 ## see a specific port such as 22 ##
```