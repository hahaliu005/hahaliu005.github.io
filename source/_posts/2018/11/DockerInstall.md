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


## Install For Ubuntu
Reference this [link](https://docs.docker.com/engine/install/ubuntu/)

Run the following command to uninstall all conflicting packages:
```
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done
```

Set up Docker's apt repository.
```
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

Install the Docker packages. (Latest version)
```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```