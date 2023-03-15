---
title: Mysql Install
tags:
  - Mysql
date: 2023-02-10
---

## Mysql 8 install for Centos 7

<!-- more -->

Download rpm file
```
curl -sSLO https://dev.mysql.com/get/mysql80-community-release-el7-7.noarch.rpm
```

Check md5sum
```
md5sum mysql80-community-release-el7-7.noarch.rpm
```

```
rpm -ivh mysql80-community-release-el7-7.noarch.rpm
```

Install
```
yum install mysql-server
```

Start and check status
```
systemctl start mysqld
systemctl status mysqld
```

Check temporary password
```
grep 'temporary password' /var/log/mysqld.log
```

Run secure script
```
mysql_secure_installation
```

## For Ubuntu
Tutorial [link](https://www.digitalocean.com/community/tutorials/how-to-install-mysql-on-ubuntu-20-04)


```
sudo apt install mysql-server
```

Change mysql password
```
sudo mysql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
```

Run secure schedule
```
sudo mysql_secure_installation
```



