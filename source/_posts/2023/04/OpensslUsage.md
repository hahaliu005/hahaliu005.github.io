---
title: Openssl Command Usage
tags:
  - Linux
date: 2023-04-05
---

<!-- more -->
Generate keys for 10 years expire
```
openssl req \
  -newkey rsa:4096 -nodes -sha256 -keyout /usr/local/openresty/nginx/conf/nginx.key \
  -x509 -days 3650 -out /usr/local/openresty/nginx/conf/nginx.crt
```