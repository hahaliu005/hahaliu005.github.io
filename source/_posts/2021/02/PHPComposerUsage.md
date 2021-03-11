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

<!-- moe -->

## For Linux
```
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('sha384', 'composer-setup.php') === '756890a4488ce9024fc62c56153228907f1545c228516cbf63f885e036d37e9a59d27d63f46af1d4d07ee0f76181c7d3') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
php composer-setup.php --install-dir=/usr/local/bin --filename=composer
php -r "unlink('composer-setup.php');"
```

# Issues
## Memory limit error, It sometime occurs in version 1, you can update composer to version 2+, or use command below.
```
COMPOSER_MEMORY_LIMIT=-1 composer [command]
```
