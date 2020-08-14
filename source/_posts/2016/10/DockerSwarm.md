---
title: Docker swarm
tags:
  - Docker
date: 2016-10-02
---

以特定机器启动服务:
为机器添加label
cat /lib/systemd/system/docker.service
ExecStart=/usr/bin/dockerd --label node=k1
create时添加labels过滤 constraint:
docker service create --constraint engine.labels.node==k1 --name redis-slave --publish 6380:6380 app_redis-slave