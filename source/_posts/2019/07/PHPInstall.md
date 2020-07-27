---
title: PHP Install
tags:
  - PHP
date: 2019-07-13
---

### Install PHP in Centos
```
rpm -Uvh http://rpms.remirepo.net/enterprise/remi-release-7.rpm && \
yum-config-manager --enable remi-php73 && \
yum install -y php-cli php-common php-devel php-fpm php-gd php-pdo php-mbstring php-mcrypt php-mysqlnd php-xml php-bcmath php-opcache
```

<!-- more -->

### Install PHP composer
```
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php composer-setup.php --install-dir=/usr/local/bin --filename=composer
php -r "unlink('composer-setup.php');"
```

### Config mirror for china
```
composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/

composer config -g --unset repos.packagist # 取消全局配置
```
