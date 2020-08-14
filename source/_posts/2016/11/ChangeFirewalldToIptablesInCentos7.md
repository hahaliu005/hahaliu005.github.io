---
title: Change firewalld to iptables in centos7
tags:
  - Linux
date: 2016-11-15
---

```
yum install -y iptables-services
systemctl disable firewalld
systemctl mask firewalld
systemctl enable iptables
systemctl stop firewalld
systemctl start iptables
```