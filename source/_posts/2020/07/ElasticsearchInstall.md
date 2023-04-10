---
title: Elasticsearch Install
tags:
  - Elasticsearch
date: 2020-07-28
---

## For Centos7
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

## For MacOS
Tap the Elastic Homebrew repository:
```
brew tap elastic/tap
```
Install
```
brew install elastic/tap/elasticsearch-full
```

## For Ubuntu

Follow this [link](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-elasticsearch-on-ubuntu-22-04) to install Elasticsearch

Follow this [link](https://www.elastic.co/guide/en/elasticsearch/reference/7.16/security-minimal-setup.html) to set minimail security for Elasticsearch

Check with username and password
```
curl -X GET -u elastic 'http://localhost:9200'
```