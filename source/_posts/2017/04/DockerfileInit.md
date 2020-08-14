---
title: Dockerfile Init
tags:
  - Docker
date: 2017-04-19
---

A Dockerfile about centos init

<!-- more -->

```
FROM centos:7

# update system
RUN rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm && \
    rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm && \
    yum update -y

# set the timezone
RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai  /etc/localtime

# install development tools
RUN yum groupinstall 'Development Tools' -y

# install base tools
RUN yum install -y wget vim

# install supervisor, this can be used to run multi server
RUN yum install supervisor -y

# install crond, this can be used to run cron task
RUN yum install cronie -y

# create server user and group
RUN useradd -u 1050 -U -m docker-user
```