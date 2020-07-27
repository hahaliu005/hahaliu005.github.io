---
title: Haproxy Setup 
tags:
  - Haproxy
date: 2019-07-19
---

Some Haproxy config example

<!-- more -->

## Haproxy config example
```
frontend http
    bind *:80
    acl http-svc hdr(host) -i http-svc.example.com
    use_backend http-svc if http-svc
    default_backend http
frontend https
    bind *:443
    acl tls req.ssl_hello_type 1
    tcp-request inspect-delay 5s
    tcp-request content accept if tls
    mode tcp
    acl https-svc req.ssl_sni -i https-svc.example.com
    use_backend https-svc if https-svc
    default_backend https
backend http
    balance roundrobin
    option httpclose
    option forwardfor
    server web1 192.168.1.101
backend https
    balance roundrobin
    mode tcp
    option tcplog
    option ssl-hello-chk
    server web2 192.168.1.102:443
backend http-svc
    balance roundrobin
    option httpclose
    option forwardfor
    server web1 192.168.1.103
backend https-svc
    balance roundrobin
    mode tcp
    option tcplog
    option ssl-hello-chk
    server web2 192.168.1.104:443
```

## Set log with rsyslog
/etc/rsyslog.d/haproxy.conf
```
# Collect log with UDP
$ModLoad imudp
$UDPServerAddress 127.0.0.1
$UDPServerRun 514

# Creating separate log files based on the severity
local0.* /var/log/haproxy-traffic.log
```
Set haproxy config log in global failed
```
global
    log 127.0.0.1:514 local0 debug
```

