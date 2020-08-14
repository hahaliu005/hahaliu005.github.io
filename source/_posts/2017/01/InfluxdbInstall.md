---
title: Influxdb Install
tags:
  - Influxdb
  - Linux
date: 2017-01-12
---

```
cat <<EOF | tee /etc/yum.repos.d/influxdb.repo
[influxdb]
name = InfluxDB Repository - RHEL \$releasever
baseurl = https://repos.influxdata.com/rhel/\$releasever/\$basearch/stable
enabled = 1
gpgcheck = 1
gpgkey = https://repos.influxdata.com/influxdb.key
EOF
```

<!-- more -->

```
yum install influxdb
systemctl start influxdb
```