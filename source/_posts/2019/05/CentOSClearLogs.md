---
title: Centos 记录清除
tags:
  - CentOS
  - Linux
date: 2016-05-04
---

清除/var/log路径下所有的日志记录文件
```
find /var/log -type f | xargs rm
```

清除命令行历史
```
cat /dev/null > ~/.bash_history && history -c
```