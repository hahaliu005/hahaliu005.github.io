---
title: 针对爬虫的几个composer仓库
tags:
  - PHP
date: 2016-05-06
---

1、Guzzle：功能很完善的httpclient，带异步并发功能，别的脚本语言找不到这么好的httpclient
2、Goutte：对symfony的dom-crawler和css-selector的简单封装，你也可以直接用symfony的css-selector来抽取html的dom元素
3、symfony/process：symfony出品的php开进程的库（封装的proc_open），兼容windows，要知道pcntl扩展不支持windows的
4、php-webdriver：Facebook官方维护的selenium的php客户端