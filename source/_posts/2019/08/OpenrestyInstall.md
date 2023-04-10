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

# Install For Ubuntu
Ref for this [link](https://openresty.org/en/linux-packages.html)

## For 20.04
```
sudo apt-get -y install --no-install-recommends wget gnupg ca-certificates
wget -O - https://openresty.org/package/pubkey.gpg | sudo apt-key add -
echo "deb http://openresty.org/package/ubuntu $(lsb_release -sc) main" | sudo tee /etc/apt/sources.list.d/openresty.list
sudo apt-get update
sudo apt-get -y install openresty
```
## For 22.04
```
sudo apt-get -y install --no-install-recommends wget gnupg ca-certificates
wget -O - https://openresty.org/package/pubkey.gpg | sudo gpg --dearmor -o /usr/share/keyrings/openresty.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/openresty.gpg] http://openresty.org/package/ubuntu $(lsb_release -sc) main" | sudo tee /etc/apt/sources.list.d/openresty.list > /dev/null
sudo apt-get update
sudo apt-get -y install openresty
```