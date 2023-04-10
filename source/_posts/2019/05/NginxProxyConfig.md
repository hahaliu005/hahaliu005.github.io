---
title: Nginx Proxy Config
tags:
  - Nginx
date: 2019-05-17
---

Something about how to config nginx proxy

<!-- more -->

- This is a common proxy config
```
upstream proxy_servers {
  server 192.168.1.101:1080 weight=2;
  server 192.168.1.102 weight=10;
}
server {
    listen 80 default_server;
    server_name _;
    access_log /data/wwwlogs/proxy.log combined;

    location / {
        proxy_set_header Host $host:$server_port;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_pass http://proxy_servers;
        client_max_body_size 50m;
        client_body_buffer_size 256k;
        proxy_connect_timeout 30;
        proxy_send_timeout 30;
        proxy_read_timeout 60;
        proxy_buffer_size 256k;
        proxy_buffers 4 256k;
        proxy_busy_buffers_size 256k;
        proxy_temp_file_write_size 256k;
        proxy_next_upstream error timeout invalid_header http_500 http_503 http_404;
        proxy_max_temp_file_size 128m;
  }
```

- About proxy_pass
```
# do not set port , set default port 80
proxy_set_header Host $host;
proxy_pass http://127.0.0.1;
# port set , use setted port
proxy_set_header Host $host:$server_port;
proxy_pass http://127.0.0.1:1080;
```
- If you want to proxy websocket as well, add these config to location area
```
proxy_set_header Upgrade $http_upgrade;
proxy_set_header Connection "upgrade";
```

- 400 Request Header Or Cookie Too Large
If you proxy localhost, you may occur this issue, please add `proxy_set_header Host $proxy_host;`