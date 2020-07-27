---
title: 'Fix: warning: setlocale: LC_CTYPE: cannot change locale'
tags:
  - Linux
date: 2019-04-09
---

The general reason for this problem is the incorrect LC_CTYPE passed to server.

If you use mac, you can change a ssh config, so ssh client will not pass LC_CTYPE to server.

<!-- more -->

Comment the line below
```
$ cat /etc/ssh/ssh_config | grep SendEnv
# SendEnv LANG LC_*
```

Add these lines...
```
$ cat /etc/environment
LANG=en_US.utf-8 
LC_ALL=en_US.utf-8
```