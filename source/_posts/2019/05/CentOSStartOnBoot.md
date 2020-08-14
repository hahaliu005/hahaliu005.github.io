---
title: Centos 开机启动服务
tags:
  - Linux
  - CentOS
date: 2016-05-23
---

为rc.loca添加可执行权限,  这是最重要的, 注意添加可执行权限一定要是原文件, 而不是链接文件:
```
chmod u+x /etc/rc.d/rc.local
```

<!-- more -->

编辑rc.local文件, 添加可执行命令, 注意环境变量, 一定要使用绝对路径, 如:
```
/usr/local/openresty/nginx/sbin/nginx -c conf/nginx.conf
```
