---
title: Jenkins Install
tags:
  - Jenkins
date: 2018-10-19
---

# run 
```
docker run -u root -d -p 8080:8080 -p 50000:50000 -v ~/data/jenkins:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock jenkinsci/blueocean 
```