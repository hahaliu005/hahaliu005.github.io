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

Check wireguard status
```
systemctl status wg-quick@wg0
```

# TCP Mode

## Server side
Edit /etc/wireguard/wg0.conf, Only list updated field
```
[Interface]
# No need ipv6, If set ipv6, MTU=1280 will occur error
Address = 10.66.11.1/24
MTU=1280
[Peer]
# No need ipv6, If set ipv6, MTU=1280 will occur error
AllowedIPs = 10.66.11.2/32
```

Download server bin & unzip & run: https://github.com/wangyu-/udp2raw-tunnel/releases
```
./udp2raw_amd64 -s -l0.0.0.0:4096 -r 127.0.0.1:7777  -a -k "passwd" --raw-mode faketcp
```


## Client side
Install pcap and libnet
```
brew install libpcap libnet
```

Edit Client config file, Only list updated field. remove ipv6 field add MTU etc.
```
[Interface]
Address = 10.66.11.2/32
MTU = 1280

[Peer]
AllowedIPs = 0.0.0.0/0
Endpoint = 127.0.0.1:3333
```

Download client bin & unzip & run: https://github.com/wangyu-/udp2raw-multiplatform/releases
```
## Need sudo
sudo ./udp2raw_mp_mac -c -l0.0.0.0:3333  -r104.233.224.112:4096 -k "passwd" --raw-mode easy-faketcp
```