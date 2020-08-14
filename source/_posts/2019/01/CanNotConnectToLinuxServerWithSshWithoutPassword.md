---
title: Can Not Connect To Linux Server With Ssh Without Password
tags:
  - Linux
date: 2019-01-09
---

Sometimes, although you set the authorized_keys correctly, you still can't connect to linux server, It still notice you to enter your password.

<!-- more -->

To resolve this problem, It usually case the error privilege set to ~/.ssh, Set the correct privilege and then you can connect to linux server without password:
```
$ chmod 700 ~/.ssh
$ chmod 600 ~/.ssh/*
drwx------. 2 XXX XXX   58 9月  14 23:02 .
drwx------. 3 XXX XXX   90 9月  14 22:55 ..
-rw-------. 1 XXX XXX  412 9月  14 23:02 authorized_keys
-rw-------. 1 XXX XXX 1675 9月  14 22:59 id_rsa
-rw-------. 1 XXX XXX  400 9月  14 22:59 id_rsa.pub
```
Oh privilege!