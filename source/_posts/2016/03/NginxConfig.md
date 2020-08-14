---
title: Nginx config
tags:
  - Nginx
date: 2016-03-06
---

Set default index and dir
```
root /usr/local/code/apvideo/public;
index index.php index.html index.htm;
```

<!-- more -->

For PHP and Laravel
```
location / {
  try_files $uri $uri/ /index.php?$query_string;
}


location ~ \.php$ {
  try_files $uri /index.php =404;
  fastcgi_split_path_info ^(.+\.php)(/.+)$;
  fastcgi_pass 127.0.0.1:9000;
  fastcgi_index index.php;
  fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
  include fastcgi_params;
}
```


### Issue
将本机文件共享至虚拟机中,
在本机上修改文件后刷新页面内容不变, 有可能跟下面这条配置有关, 将之设为off, 但是在正式环境中建议设为on
sendfile off;
进一步排查，发现并不是这么简单的，也有可能跟php-fpm开启了opcache有关，导致文件更改后界面不变，故如果是在测试环境中，需要将opcache禁用，以致于不影响测试：
vim /usr/local/php/etc/php-fpm.conf
php_admin_flag[opcache.enable] = off