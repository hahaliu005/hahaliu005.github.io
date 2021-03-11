---
title: Gradle Add mirror
tags:
  - Gradle
date: 2021-03-06
---


For system: ~/.gradle/init.gradle
```
allprojects {
    repositories {
        def ALIYUN_REPOSITORY_URL = 'https://maven.aliyun.com/repository/public'
        all { ArtifactRepository repo ->
            if(repo instanceof MavenArtifactRepository){
                def url = repo.url.toString()
                if (url.startsWith('https://repo1.maven.org/maven2')) {
                    project.logger.lifecycle "Repository ${repo.url} replaced by $ALIYUN_REPOSITORY_URL."
                    remove repo
                }
            }
        }
        maven { url ALIYUN_REPOSITORY_URL }
    }
}
```

<!-- more -->

For single project: build.gradle
> buildscript block must before plugins block
```
buildscript {
    repositories {
        maven { url 'https://maven.aliyun.com/repository/public' }
    }
}

allprojects {
    repositories {
        maven { url 'https://maven.aliyun.com/repository/public' }
    }
}
```