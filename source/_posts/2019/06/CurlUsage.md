---
title: Curl Usage
tags:
  - Curl
  - Linux
date: 2019-06-30
---

Send post request
```
curl -X POST -d 'param1=value1&param2=value2' 'http://example.com/apis'
```
<!-- more -->

Only get header response
```
curl -I http://example.com
```

## Set curl socks5 proxy
Edit ~/.curlrc
```
socks5 = 127.0.0.1:1080
```