---
title: Run Local Services With Docker
tags:
  - Docker
date: 2019-05-07
---

- Run local mysql
```
docker run -p 3306:3306 --name local-mysql -v /Volumes/Data/database/local-mysql:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=111 --restart always -d mysql:5.7
```

<!-- more -->

- Run local redis
```
docker run -p 6379:6379 --name local-redis -v /Volumes/Data/database/local-redis:/data --restart always -d redis:5
```