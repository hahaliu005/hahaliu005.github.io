---
title: Maven Add mirrors
tags:
  - Maven
date: 2021-03-06
---

For System: ~/.m2/settings.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">

    <!-- Add mirrors here -->
    <mirrors>
        <!-- Aliyun -->
        <mirror>
            <id>aliyunmaven</id>
            <name>aliyun maven</name>
            <mirrorOf>*</mirrorOf>      
            <url>https://maven.aliyun.com/repository/public</url>
        </mirror>
    </mirrors>
</settings>
```

<!-- more -->

For project: pom.xml
```xml
<repositories>
    <repository>
        <id>aliyunmaven</id>
        <url>https://maven.aliyun.com/repository/public</url>
    </repository>
</repositories>
```