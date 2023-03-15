---
title: Centos7 Set Static Ip
tags:
  - Linux
date: 2017-05-06
---

- This is a standard network eth static ip config

<!-- more -->

```
# cat /etc/sysconfig/network-scripts/ifcfg-enp0s3
TYPE=Ethernet
BOOTPROTO=static
DEFROUTE=yes
PEERDNS=yes
PEERROUTES=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_PEERDNS=yes
IPV6_PEERROUTES=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=enp0s3
UUID=f1407dbc-b248-4c56-ad40-27ce64febc44
DEVICE=enp0s3
ONBOOT=yes
NM_CONTROLLED=no
IPADDR=192.168.1.105
NETMASK=255.255.255.0
GATEWAY=192.168.1.1
DNS1=192.168.1.1
```

- The most import elem is below 
```
BOOTPROTO=static
ONBOOT=yes
# for use this config file's config, not other
NM_CONTROLLED=no
IPADDR=192.168.1.105
NETMASK=255.255.255.0
GATEWAY=192.168.1.1
# For domain resolve
DNS1=192.168.1.1
```

Restart network after change config
```
service network restart
```

Maybe can not resolve domain after config, can edit /etc/resolv.conf
```
nameserver 8.8.8.8
nameserver 8.8.4.4
```

You can use for multi ip
```
IPADDR1=192.168.1.101
IPADDR2=192.168.1.102
IPADDR3=192.168.1.103
IPADDR4=192.168.1.104
NETMASK=255.255.255.0
```

# For Ubuntu
Ubuntu use netplan to set network.
Change /etc/netplan/xxxx.yaml file
```
# This is the network config written by 'subiquity'
network:
  ethernets:
    enp0s5:
      dhcp4: no
      addresses:
        - 10.211.55.112/24
      gateway4: 10.211.55.1
      nameservers:
          addresses: [10.211.55.1]
  version: 2
```
enp0s5 is the interface name, can check use `ip addr`

Then apply change `sudo netplan apply`