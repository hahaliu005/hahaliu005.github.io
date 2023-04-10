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


## Ubuntu System
Above Ubuntu 22.04 , there's no 7.4 in default repo anymore, need to add PPA
```
sudo apt install -y software-properties-common
sudo add-apt-repository ppa:ondrej/php -y
```

Below Ubuntu 22.04, can install php7.4 directly
```
sudo apt -y install php7.4 php7.4-fpm php7.4-cli php7.4-json php7.4-common php7.4-mysql php7.4-zip php7.4-gd php7.4-mbstring php7.4-curl php7.4-xml php7.4-bcmath
```

### Composer install
```
sudo php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
sudo php composer-setup.php --install-dir=/usr/local/bin --filename=composer
sudo php -r "unlink('composer-setup.php');"
```

### 注意事项
- 也许php默认配置使用apache用户组，如果更换运行服务的用户名与用户组，请留意文件夹权限，如/var/lib/php中的session,opcache可能不可写，可以更改/var/lib/php的用户组

