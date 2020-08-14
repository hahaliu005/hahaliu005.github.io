---
title: Virtualbox安装centos7.2问题汇总
tags:
  - Virtualbox
  - Centos
date: 2016-09-01
---

### virtualbox虚拟机安装centos后无法联网的问题
一. 查看是否有设置网络
二. 查看/etc/sysconfig/network-scripts/中对应网卡中的ONBOOT=yes
三. 检查无误后可重启network
systemctl restart network