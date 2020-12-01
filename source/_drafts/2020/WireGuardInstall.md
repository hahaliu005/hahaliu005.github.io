---
title: WireGuard Install
tags:
  - WireGuard
  - Linux
date: 2020-02-27
---

### Article [here](https://ssr.tools/1086) (Deprecated)

<!-- more -->

## For Ubuntu
Update kernel version 5.6.19 use script from https://github.com/pimlie/ubuntu-mainline-kernel.sh
```
apt install wget
wget https://raw.githubusercontent.com/pimlie/ubuntu-mainline-kernel.sh/master/ubuntu-mainline-kernel.sh
chmod +x ubuntu-mainline-kernel.sh
./ubuntu-mainline-kernel.sh -i v5.6.19
```

After Upgrade kernel version, reboot system

Install wireguard use script from https://github.com/angristan/wireguard-install
```
curl -O https://raw.githubusercontent.com/angristan/wireguard-install/master/wireguard-install.sh
chmod +x wireguard-install.sh
./wireguard-install.sh
```

## Get QR code
qrencode -t ansiutf8 < /etc/wireguard/client.conf

Issues
---
## For install failed, maybe you need to reboot ubunt system 