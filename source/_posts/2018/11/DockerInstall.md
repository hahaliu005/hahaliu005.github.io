---
title: Docker Install
tags:
  - Docker
date: 2018-11-19
---

## Install for fast simple test
```shell
curl -sSL https://get.docker.com/ | sh
```

<!-- more -->

## Install for production environment
```shell
# install requirements
yum install -y yum-utils device-mapper-persistent-data lvm2
# add docker-ce repo
yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
# index repo
yum makecache fast
# list the docker-ce version
yum list docker-ce.x86_64  --showduplicates | sort -r
# instal latest version with yum install -y docker-ce
# or install the specified version by list above
yum install -y docker-ce-<VERSION>
# add user to docker group
usermod -aG docker <username>
```
### One command to install, latest stable docker
```
yum install -y yum-utils device-mapper-persistent-data lvm2 && \
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo && \
yum makecache fast && \
yum install -y docker-ce
```


> If some of your log redirect to stdout, want to rotate log, you can config daemon.json
```
# cat /etc/docker/daemon.json
{
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "512m",
    "max-file": "5"
  }
}
# After change config, need to restart docker, and kubelet, and kube-proxy, or other pod
```

> For the error 'net/http: TLS handshake timeout' when pull image in China , [Here have a guid doc](https://www.daocloud.io/mirror#accelerator-doc), or add `"registry-mirrors": ["https://registry.docker-cn.com"]` to daemon.json