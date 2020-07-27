---
title: Linux About Set Time And TimeZone
tags:
  - Linux
date: 2018-12-26
---

### Set system timezone
```
# set timezone
$ timedatectl set-timezone Asia/Shanghai
# write to hardware clock
$ timedatectl set-local-rtc 0
```

<!-- more -->

### install ntpdate to sync the time
```
$ apt install ntpdate
$ ntpdate cn.pool.ntp.org
```

### For addition , may be you want to get the unix timestamp :
```
$ date +%s
```

### Or , maybe you want to get the unix timestamp based the date string:
```
$ date --date '20130808 16:32:45' +%s
#it will echo the unix timestamp base the date
```