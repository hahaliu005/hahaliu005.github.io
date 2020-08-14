---
title: WireGuard Install
tags:
  - WireGuard
  - Linux
date: 2020-02-27
---

### Article [here](https://ssr.tools/1086)

<!-- more -->

## For Ubuntu
```
wget https://raw.githubusercontent.com/atrandys/wireguard/master/wireguard_install_ubuntu.sh && chmod +x wireguard_install_ubuntu.sh && ./wireguard_install_ubuntu.sh
```
## For Debian
```
wget --no-check-certificate -qO- 'https://moeclub.org/attachment/LinuxShell/wireguard.sh'| bash
```

## For Centos
```
yum install -y wget && wget https://raw.githubusercontent.com/atrandys/wireguard/master/wireguard_install.sh && chmod +x wireguard_install.sh && ./wireguard_install.sh
```

## Add new client
Run shell script
Select add user
Input client config file name, that's ok

## Get QR code
qrencode -t ansiutf8 < /etc/wireguard/client.conf

Issues
---
## For install failed, maybe you need to reboot ubunt system 