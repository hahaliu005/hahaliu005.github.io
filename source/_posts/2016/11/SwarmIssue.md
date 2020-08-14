---
title: Swarm issues
tags:
  - Docker
date: 2016-11-20
---

### 网络无法互朕
很有可能是因为时钟不同步造成的, 在所有的服务器中添加时钟同步

<!-- more -->

```
yum install ntp -y
# 启动ntp服务, 配置开机启动
systemctl start ntpd
systemctl enable ntpd
```

列出你需要的时区:
```
timedatectl list-timezones | grep Shanghai
```

设置时区
```
sudo timedatectl set-timezone Asia/Shanghai
```

如果看到有下面这一行警告, 请按照警告上的说明设置本地时间为UTC时间:
```
#: timedatectl
Warning: The system is configured to read the RTC time in the local time zone.
  This mode can not be fully supported. It will create various problems
  with time zone changes and daylight saving time adjustments. The RTC
  time is never updated, it relies on external facilities to maintain it.
  If at all possible, use RTC in UTC by calling
  'timedatectl set-local-rtc 0'.
```

同步本地硬件(RTC)时钟与网络UTC时钟
```
timedatectl set-local-rtc 0
```