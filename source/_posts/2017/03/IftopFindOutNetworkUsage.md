---
title: Iftop Find Out Network Usage
tags:
  - Linux
date: 2017-03-18
---

```
yum install -y iftop
# you can check iftop help first : iftop -h

# Check display port, sometime will convert port to service name
iftop -P
# Check do not convert port to service name
iftop -NP
```