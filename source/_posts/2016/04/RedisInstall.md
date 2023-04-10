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

## For Ubuntu
Tutorial [link](https://www.digitalocean.com/community/tutorials/how-to-install-and-secure-redis-on-ubuntu-22-04)

And official [link](https://redis.io/docs/getting-started/installation/install-redis-on-linux/) to install newest version
```
sudo apt install lsb-release

curl -fsSL https://packages.redis.io/gpg | sudo gpg --dearmor -o /usr/share/keyrings/redis-archive-keyring.gpg

echo "deb [signed-by=/usr/share/keyrings/redis-archive-keyring.gpg] https://packages.redis.io/deb $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/redis.list

sudo apt-get update
sudo apt-get install redis
```

Remember to set requirepass in /etc/redis/redis.conf