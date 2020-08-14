---
title: Plupload 进行跨域上传
tags:
  - PHP
  - Laravel
  - Plupload
date: 2016-11-11
---

当使用html5进行上传时

一. 在上传服务器添加跨域访问头
```
header('Access-Control-Allow-Origin: *');
header('Access-Control-Allow-Methods: GET, POST, PATCH, PUT, DELETE, OPTIONS');
header('Access-Control-Allow-Headers: Origin, Content-Type, X-Auth-Token, x-csrf-token');
```

<!-- more -->

二. 添加一个option类型的请求:
```
Route::post('/upload', 'Controller@upload');
Route::options('/upload', 'Controller@upload');
```