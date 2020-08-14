---
title: Deploy A Docker Registry Server
tags:
  - Docker
date: 2017-03-20
---

* Generate ca files
```shell
mkdir -p /etc/docker/certs
openssl req \
  -newkey rsa:4096 -nodes -sha256 -keyout /etc/docker/certs/registry.key \
  -x509 -days 3650 -out /etc/docker/certs/registry.crt
```

<!-- more -->

> It will notice you to enter serveal info, it up to you, except the domain which we will use it afterward, eg: registry.prod

* Startup the registry
```shell
docker run -d -p 5000:5000 --restart=always --name registry \
  -v /etc/docker/certs:/certs \
  -v /data/registry:/var/lib/registry \
  -e REGISTRY_HTTP_TLS_CERTIFICATE=/certs/registry.crt \
  -e REGISTRY_HTTP_TLS_KEY=/certs/registry.key \
  registry:2
```

* For the client server which want to access registy, copy the registry.crt and put it to the correct path.
```shell
mkdir -p /etc/docker/certs.d/registry.prod:5000
cp /etc/docker/certs/registry.crt /etc/docker/certs.d/registry.prod:5000/ca.crt
```

* And For the client server, we need to add a domain host
```shell
# cat /etc/hosts
[registry_server_ip] registry.prod
```

* Check pem file expire date
```
openssl x509 -enddate -noout -in /etc/kubernetes/cert/ca.pem
```