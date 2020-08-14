---
title: Centos Network Config
tags:
  - Linux
date: 2018-01-03
---

A Centos Network Config Example

<!-- more -->

```
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=dhcp
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NETMASK=255.255.255.0
NAME=enp0s3
UUID=8678d039-5ef8-487f-a127-7def4e8f6b5a
DEVICE=enp0s3
ONBOOT=yes
```

important: NETMASK