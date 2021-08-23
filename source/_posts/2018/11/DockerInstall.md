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
### Uninstall old versions
```shell
yum remove docker \
  docker-client \
  docker-client-latest \
  docker-common \
  docker-latest \
  docker-latest-logrotate \
  docker-logrotate \
  docker-engine
```

### Set up the repository
```shell
yum install -y yum-utils
yum-config-manager \
  --add-repo \
  https://download.docker.com/linux/centos/docker-ce.repo
```

### Install latest docker
```shell
yum install -y docker-ce docker-ce-cli containerd.io
```

## Install docker-compose
```shell
curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
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