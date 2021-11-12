---
title: GitLab Install
tag:
  - GitLab
date: 2018-12-26
---

## Run gitlab in docker
```shell
docker run --detach \
    --hostname git-dev.prod \
    --publish 8443:443 --publish 8080:80 --publish 8022:22 \
    --name gitlab \
    --restart always \
    --volume /data/gitlab/config:/etc/gitlab \
    --volume /data/gitlab/logs:/var/log/gitlab \
    --volume /data/gitlab/data:/var/opt/gitlab \
    gitlab/gitlab-ce:latest
```

<!-- more -->

Remap port 22 to 8022 because sshd always use port 22

> All the gitlabe persist data will be store st /data/gitlab directory, If you want to migrate, just copy this directory, and there maybe some permission problem after copy, check logs you will find the revolution.

## How to change hostname
Edit config file in docker
/etc/gitlab/gitlab.rb
```
external_url 'https://gitlab.example.com'
```

Then run command in docker
```
gitlab-ctl reconfigure
```

## How to check initial password
```
docker exec -it gitlab grep 'Password:' /etc/gitlab/initial_root_password
```

## How to clone when ports are remaped
The git clone format just like below
```sh
# git clone ssh://git@[your_domain]:[your_port]/[your_user]/[your_project].git

git clone ssh://git@git-dev.prod:8022/root/test.git
```