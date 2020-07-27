---
title: 一键安装最新内核并开启 BBR 脚本
tags:
  - BBR
  - Linux
---

Reference from this [link](https://teddysun.com/489.html)
## Run this on click script to install new kernel and bbr
```
wget --no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh && chmod +x bbr.sh && ./bbr.sh
```

<!-- more -->

## Check new kernel
```
uname -r
```
## Check bbr
```
sysctl net.ipv4.tcp_available_congestion_control
# May be 'net.ipv4.tcp_available_congestion_control = bbr cubic reno'
# Or 'net.ipv4.tcp_available_congestion_control = reno cubic bbr'

sysctl net.ipv4.tcp_congestion_control
# May be 'net.ipv4.tcp_congestion_control = bbr'

sysctl net.core.default_qdisc
# May be 'net.core.default_qdisc = fq'

lsmod | grep bbr
# May be tcp_bbr
```