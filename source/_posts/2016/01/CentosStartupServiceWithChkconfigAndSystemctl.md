---
title: Centos startup service with chkconfig and systemctl
tags:
  - CentOS
  - Linux
date: 2016-10-13
---

将下面的模板文件放入/etc/init.d/ 中按需修改: 

<!-- more -->

```bash
#!/bin/sh

# chkconfig: - 99 99
# description: php startup script

EXEC=/usr/sbin/php-fpm
CONF="/etc/php-fpm.conf"
PIDFILE=$(sed -n 's/^pid[ =]*//p' $CONF)

case "$1" in
  start)
  if [ -f $PIDFILE ]
  then
  echo "$PIDFILE exists, process is already running or crashed"
  else
  echo "Starting PHP-FPM server..."
  $EXEC -c $CONF
  fi
  ;;
  stop)
  if [ ! -f $PIDFILE ]
  then
  echo "$PIDFILE does not exist, process is not running"
  else
  PID=$(cat $PIDFILE)
  echo "Stopping ..."
kill $PID
  while [ -x /proc/${PID} ]
  do
  echo "Waiting for PHP-FPM to shutdown ..."
  sleep 1
  done
  echo "PHP-FPM stopped"
  fi
  ;;
  status)
  PID=$(cat $PIDFILE)
  if [ ! -x /proc/${PID} ]
  then
  echo 'PHP-FPM is not running'
  else
  echo "PHP-FPM is running ($PID)"
  fi
  ;;
  restart)
  $0 stop
  $0 start
  ;;
  *)
  echo "Please use start, stop, restart or status as first argument"
  ;;
esac
```

为文件分配可执行的权限
chmod u+x /etc/init.d/servicename

添加进chkconfig
sudo chkconfig --add servicename

开机启动执行
sudo chkconfig --level 2345 servicename on

使用systemctl设置开机启动
sudo systemctl enable servicename.service

禁止开机启动
sudo systemctl disable servicename.service