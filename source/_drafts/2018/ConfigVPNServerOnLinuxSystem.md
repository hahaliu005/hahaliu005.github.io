---
title: Config VPN Server On Linux System
tags:
  - VPN
  - Linux
date: 2018-08-05
---

There are many methods to set up a VPN server in linux , I use PPTP for simple . And set up the VPN on UBUNTU system . for the privilege to every user , please put 'sudo' in front of every commond

<!-- more -->

First , install the PPTPD package:
```
$ apt-get install pptpd
```

Finish install it , then edit the /etc/pptpd.conf
```
$ vim /etc/pptpd.conf
```

uncommond the two commond below
```
localip 192.168.0.1
remoteip 192.168.0.234-238,192.168.0.245
```

set the dns server in /etc/options
```
ms-dns 8.8.8.8
ms-dns 8.8.4.4
```
Then change the /etc/ppp/chap-secrets , add a line like this:
```
username pptpd userpassword *
```
The first col is user name , The second col is protocol , maybe also can try '*' instead .

To let the config work , we restart the pptpd server .
```
$ /etc/init.d/pptpd restart
or
$ service pptpd restart
```

On finished these config , we find a computer , create a VPN connect , use the server ip , and username , password we set before ,

basicly , you can connect to server now , but can't connect internet .

Next step , to open the ipv4 forward , change /etc/sysctl.conf , uncommond the below line
```
net.ipv4.ip_forward=1
```
set up the config
```
$ sysctl -p
```
Sometimes it can connect to internet now . and then config the iptables
```
$ iptables -t nat -A POSTROUTING -s 192.168.1.0/24 -o eth0 -j MASQUERADE
```
The ip are the server ip piece and can tap ifconfig to check whether the network are eth0

save it !
```
$ iptables-save > /etc/iptables-rules
```
change /etc/network/interfaces , find the piece for eth0 add some word after it
```
$ pre-up iptables-restore</etc/iptables-rules
```
For this it will set up iptable automaticly .

So , a basic VPN server finished .
