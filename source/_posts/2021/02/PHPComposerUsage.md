---
title: Composer Usage
tags:
  - PHP
  - Composer
date: 2021-02-07
---

# Installation
## For MacOS
```
brew install composer
```

## For MacOS brew
```
brew install composer
```

<!-- moe -->

## For Linux
```
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('sha384', 'composer-setup.php') === '906a84df04cea2aa72f40b5f787e49f22d4c2f19492ac310e8cba5b96ac8b64115ac402c8cd292b8a03482574915d1a8') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
php composer-setup.php --install-dir=/usr/local/bin --filename=composer
php -r "unlink('composer-setup.php');"
```

# Issues
Memory limit error, It sometime occurs in version 1, you can update composer to version 2+, or use command below.
```
COMPOSER_MEMORY_LIMIT=-1 composer [command]
```
