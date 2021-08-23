---
title: Ingress nginx configration
tags:
  - Ingress
  - Nginx
date: 2021-08-16
---

## Fix cann't get real ip in laravel.
Get configmap which named ingress-nginx-configuration
```
kubectl get cm -n kube-system
```

<!-- more -->

Edit configmap
```
kubectl edit cm ingress-nginx-configuration -n kube-system 
```

Content in data field
```
# Other contents
data:
  compute-full-forwarded-for: "true"
  use-forwarded-headers: "true"
  proxy-body-size: 1024m
```
`compute-full-forwarded-for: "true"` && `use-forwarded-headers: "true"` : Fix Laravel can not get ip correctly
`proxy-body-size: 1024m` : Same as nginx config: client_max_body_size, This can fix file upload too large etc.

Delete nginx-ingress pod, then it will auto start
```
kubectl get pod -n kube-system
kubectl delete pod nginx-ingress-xxxxx -n kube-system
```

Nginx ingress configmap reference [here](https://kubernetes.github.io/ingress-nginx/user-guide/nginx-configuration/configmap/#compute-full-forwarded-for)