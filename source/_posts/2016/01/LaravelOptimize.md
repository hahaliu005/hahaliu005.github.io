---
title: Laravel 程序优化技巧
tags:
  - Laravel
  - PHP
date: 2016-01-17
---

- 配置信息缓存 artisan config:cache
- 路由缓存 artisan route:cache
- 类映射加载优化 artisan optimize
- 自动加载优化 composer dumpautoload
- 使用 Memcached 来存储会话 config/session.php
- 使用专业缓存驱动器 config/cache.php
- 数据库请求优化
- 为数据集书写缓存逻辑
- 使用即时编译器（JIT），如：HHVM、OpCache
- 前端资源合并 Elixir