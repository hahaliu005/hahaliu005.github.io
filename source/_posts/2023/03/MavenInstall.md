---
title: Maven Install
tags:
  - Java
  - Maven
date: 2023-03-15
---

<!-- more -->
```
wget https://dlcdn.apache.org/maven/maven-3/3.9.1/binaries/apache-maven-3.9.1-bin.tar.gz -O /opt/apache-maven-3.9.1-bin.tar.gz
cd /opt/ && sudo tar zxvf /opt/apache-maven-3.9.1-bin.tar.gz
```

Export bin to PATH
```
export PATH="/opt/apache-maven-3.9.1/bin:$PATH"
```