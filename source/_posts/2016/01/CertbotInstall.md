---
title: Certbot Install
tags:
  - Certbot
  - Linux
date: 2016-01-09
---

<!-- more -->

Install Certbot
```
yum install -y epel-release
yum -y install yum-utils
yum-config-manager --enable rhui-REGION-rhel-server-extras rhui-REGION-rhel-server-optional
```

```
certbot --nginx certonly -d example.com --register-unsafely-without-email
```

# For Ubuntu
Install certbot directly
```
sudo apt install -y certbot
```