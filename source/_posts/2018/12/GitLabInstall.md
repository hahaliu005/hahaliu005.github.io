---
title: GitLab Install
tag:
  - GitLab
date: 2018-12-26
---

## Run gitlab in docker
```shell
docker run --detach \
    --hostname git-dev \
    --publish 443:443 --publish 8192:80 --publish 8193:22 \
    --name gitlab \
    --restart always \
    --volume /data/gitlab/config:/etc/gitlab \
    --volume /data/gitlab/logs:/var/log/gitlab \
    --volume /data/gitlab/data:/var/opt/gitlab \
    gitlab/gitlab-ce:latest
```

<!-- more -->

> All the gitlabe persist data will be store st /data/gitlab directory, If you want to migrate, just copy this directory, and there maybe some permission problem after copy, check logs you will find the revolution.

## How to change ip address
Edit config file
```
cat /etc/gitlab/gitlab.rb
    external_url 'http://gitlab.example.com'
```
Then run these command in docker
```
gitlab-ctl reconfigure
gitlab-ctl restart
```
