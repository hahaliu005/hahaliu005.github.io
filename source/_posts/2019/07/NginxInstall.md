---
title: Nginx Install
tags:
  - Nginx
  - Linux
date: 2019-07-03
---

## Compile from source
Install requirements
```
yum install epel-release -y && \
yum groupinstall 'Development Tools' -y && \
yum install pcre-devel zlib-devel -y
```
<!-- more -->
Compile openssl
```
wget https://github.com/openssl/openssl/archive/OpenSSL_1_1_1c.tar.gz && \
tar zxvf OpenSSL_1_1_1c.tar.gz && \
cd openssl-OpenSSL_1_1_1c && \
./config && \
make -j$(nproc) && \
make install
```
Compile nginx
```
wget http://nginx.org/download/nginx-1.17.1.tar.gz && \
tar zxvf nginx-1.17.1.tar.gz && \
cd nginx-1.17.1 && \
./configure --with-http_v2_module --with-http_ssl_module --with-http_gzip_static_module --with-openssl-opt=enable-tls1_3 --with-openssl=/root/openssl-OpenSSL_1_1_1c && \
make -j$(nproc) && make install
```
