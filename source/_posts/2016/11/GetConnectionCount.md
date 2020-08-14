---
title: 获取端口的连接数量
tags:
  - Linux
date: 2016-11-17
---

ss -tan state established '( sport = :80 or dport = :80 )' | wc -l