---
title: Elasticsearch Install
tags:
  - Elasticsearch
date: 2020-07-28
---

Import the Elasticsearch PGP Key
```
rpm --import https://artifacts.elastic.co/GPG-KEY-elasticsearch
```

<!-- more -->

Create a file called elasticsearch.repo in the /etc/yum.repos.d/
```
[elasticsearch]
name=Elasticsearch repository for 7.x packages
baseurl=https://artifacts.elastic.co/packages/7.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=0
autorefresh=1
type=rpm-md
```

Install
```
yum install --enablerepo=elasticsearch elasticsearch
```

Auto start 
```
systemctl enable elasticsearch
systemctl start elasticsearch
systemctl status elasticsearch
```

