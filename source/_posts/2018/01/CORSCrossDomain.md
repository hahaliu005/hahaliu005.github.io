---
title: CORS Cross Domain
tags:
  - HTML
date: 2018-01-25
---
* 使用option提前请求服务器查看是不有可访问的权限，服务器返回的几个关键头
```
# 允许访问的域名
Access-Control-Allow-Origin: *
# 允许访问的方法
Access-Control-Allow-Methods: GET
# 带有此头信息的才允许访问,比如说像需要验证信息访问的，客户端问题会发送两次请求，一次option，一次真正的请求
Access-Control-Allow-Headers: authorization
```

<!-- more -->

* 使用同域代理,如example.com/api使用nginx代理至服务器api, example.com网站请求example.com/api/*即为同域

* 使用JSONP，利用script标签使请求服务器，服务器返回一段可执行的javascript代码执行