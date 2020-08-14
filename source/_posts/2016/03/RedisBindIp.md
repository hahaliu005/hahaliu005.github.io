---
title: redis 绑定ip
tags:
  - Redis
date: 2016-03-23
---

使用ifconfig查看各网卡的inet
```
enp0s3: inet 10.72.2.47
enp0s3:1: inet 10.72.2.48
lo: inet 127.0.0.1
```

设置redis.conf
```
bind 10.72.2.47 10.72.2.48 127.0.0.1
```

那些此时可以使用这三个IP连接至redis-server, 使用其它方式的IP连接均会被拒绝
```
redis-cli -h 10.72.2.47
redis-cli -h 10.72.2.48
redis-cli -h 127.0.0.1
```
