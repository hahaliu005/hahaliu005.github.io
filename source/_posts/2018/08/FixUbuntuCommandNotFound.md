---
title: 'Fix: Ubuntu command-not-found'
tags:
  - Ubuntu
  - Linux
date: 2018-08-06
---

When you meet this error in the ubuntu system:
```
Sorry, command-not-found has crashed!  bla bla ......
```

<!-- more -->

You can try this:
```
$ export LANGUAGE=en_US.UTF-8
$ export LANG=en_US.UTF-8
$ export LC_ALL=en_US.UTF-8
$ locale-gen en_US.UTF-8
$ dpkg-reconfigure locales
```
Then this error may be resolved.