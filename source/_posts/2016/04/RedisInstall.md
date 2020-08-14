---
title: Redis Install
tags:
  - Redis
date: 2016-04-25
---

```
wget http://download.redis.io/releases/redis-3.2.3.tar.gz
tar zxvf redis-3.2.3.tar.gz
cd redis-3.2.3
make
sudo make install
```

<!-- more -->

配置redis-server开机启动, redis-server可执行命令路径 => /usr/local/bin/redis-server
```
sudo ./utils/install_server.sh
```