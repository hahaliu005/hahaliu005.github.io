---
title: UFW setup
tags:
  - Ubuntu
date: 2023-03-13
---

Ref link [here](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-with-ufw-on-ubuntu-20-04)

<!-- more -->

Before enable ufw, Please make sure you enable ssh port that you can  connect to server remotely.

### Allow from ip to port
```
sudo ufw allow from 192.168.0.1 to any port 22
```