---
title: Logrotate日志滚动配置
tags:
  - Logrotate
date: 2016-01-12
---

滚动日志配置文件存放在/etc/logrotate.d文件夹下, 如nginx的滚动日志配置

<!-- more -->

vim /etc/logrotate.d/nginx
```
/var/log/nginx/*.log {
  daily
  dateext
  missingok
  rotate 7
  compress
  delaycompress
  notifempty
  create
  sharedscripts
  postrotate
  [ ! -f /opt/nginx/logs/nginx.pid ] || kill -USR1 `cat /opt/nginx/logs/nginx.pid`
  endscript
}
```

```
/var/log/php-fpm.log {
  daily
  dateext
  missingok
  rotate 7
  compress
  delaycompress
  notifempty
  create
  sharedscripts
  postrotate
  [ ! -f /run/php/php-fpm.pid ] || kill -USR1 `cat /run/php/php-fpm.pid`
  endscript
}
```

kill -USR1 [pid]  :  为程序发送一个重新载入配置的信号, 让程序重新打开日志文件

可以使用man logrotate来查看各配置参数的说明