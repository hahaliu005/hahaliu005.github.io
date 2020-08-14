---
title: DNS Server Setup For Mac
tags:
  - MacOS
date: 2017-06-08
---

```
brew install dnsmasq
```

<!-- more -->

Edit /usr/local/etc/dnsmasq.conf
```
strict-order
listen-address=192.168.0.100,127.0.0.1  
# Resolve all the '*.test' domain to 192.168.0.100
address=/.test/192.168.0.100
```

Run as root
```
sudo brew services start dnsmasq
# or
sudo /usr/local/sbin/dnsmasq
```
> Remember use sudo to start dnsmasq

> Change hosts in /etc/hosts of macos

Then you can set dns ip in the phone or other device.
