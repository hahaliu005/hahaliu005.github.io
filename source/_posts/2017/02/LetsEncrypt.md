---
title: Let's Encrypt
tags:
  - Nginx
date: 2017-02-21
---

- Reference from this [site](http://www.laozuo.org/7676.html)

- Generate keys
```
git clone https://github.com/certbot/certbot
cd letsencrypt
./letsencrypt-auto certonly --standalone --email admin@domain.com -d domain1.com -d domain2.com
```

<!-- more -->

- It will generate 4 file in /etc/letsencrypt/live/domain.com
```
cert.pem  - Apache服务器端证书
chain.pem  - Apache根证书和中继证书
fullchain.pem  - Nginx所需要ssl_certificate文件
privkey.pem - 安全证书KEY文件
```

- Set nginx config server area
```
listen 443 ssl http2;
listen [::]:443 ssl http2;
server_name domain1.com domain2.com;

ssl_certificate /etc/letsencrypt/live/domain.com/fullchain.pem;
ssl_certificate_key /etc/letsencrypt/live/domain.com/privkey.pem;
```

- If expired after 90 days, you can renew it
```
./letsencrypt-auto certonly --renew-by-default --email admin@domain.com -d domain1.com -d domain2.com
```