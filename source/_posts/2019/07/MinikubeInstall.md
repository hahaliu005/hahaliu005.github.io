---
title: Minikube Setup
tags:
  - Minikube
date: 2019-07-24
---

Install minikube
```
brew cask install minikube
```

<!-- more -->

Start minikube
```
minikube start --cpus=6 --memory=8192 --kubernetes-version=v1.12.4 --extra-config=apiserver.service-node-port-range=80-10000
```