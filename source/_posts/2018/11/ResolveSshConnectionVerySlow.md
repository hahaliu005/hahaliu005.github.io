---
title: Resolve ssh connection very slow 
tags:
  - Linux
date: 2018-11-20
---

If connect to host very slow , (eg: connect to the itself) can try change sshd setting then restart sshd
```
# cat /etc/ssh/sshd_config
UseDNS no
```
`systemctl restart sshd`