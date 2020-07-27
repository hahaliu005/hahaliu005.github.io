----------------
title: Openresty Install
tags:
    - Openresty
    - Linux
date: 2019-08-01
----------------

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

Compile openresty
```
wget https://openresty.org/download/openresty-1.15.8.1.tar.gz && \
tar zxvf openresty-1.15.8.1.tar.gz && \
cd openresty-1.15.8.1 && \
./configure --with-http_v2_module --with-http_ssl_module --with-http_gzip_static_module --with-openssl-opt=enable-tls1_3 --with-openssl=/root/openssl-OpenSSL_1_1_1c && \
gmake -j$(nproc) && gmake install
```

## Install use yum directly

```
yum install yum-utils && \
yum-config-manager --add-repo https://openresty.org/package/centos/openresty.repo && \
yum install openresty openresty-resty
```