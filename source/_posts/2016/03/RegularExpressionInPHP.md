---
title: PHP 正则
tags:
  - PHP
date: 2016-03-22
---

例：匹配多个中文字符（utf-8）：
```
preg_match('/^[\x{4e00}-\x{9fa5}]+/u', $str, $match)
```