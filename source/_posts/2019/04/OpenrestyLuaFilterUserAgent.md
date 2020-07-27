---
title: Openresty Lua Filter User Agent
tags:
  - Openresty
date: 2019-04-25
---

- Add `access_by_lua_file /test.lua;` in nginx.conf

- test.lua
```
local userAgent = ngx.req.get_headers()["User-Agent"];

local result, err = ngx.re.match(userAgent, "MSIE 7.0|MSIE 8.0");

if result then
  ngx.exit(403);
end
```