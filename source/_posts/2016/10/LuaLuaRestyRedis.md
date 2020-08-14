---
title: 利用 Lua 的函数式编程简化 lua-resty-redis 的使用
tags:
  - Lua
  - Redis
  - Openresty
date: 2016-10-26
---

在利用 OpenResty 编写高性能服务的时候，很有可能会使用到 Redis。在 OpenResty 中，我们通常使用 lua-resty-redis 这个模块操作 Redis。

<!-- more -->

在 lua-resty-redis 的示例中，我们可以总结出以下几个步骤：

导入 resty.redis 模块
实例化 redis 对象:


```lua
localred=redis:new()
red:set_timeout(1000)
```
连接到服务器:
```lua
localok,err=red:connect("127.0.0.1",6379)
```
操作 Redis
将连接添加到连接池中
```lua
localok,err=red:set_keepalive(1000,100)
```
以上的几个步骤是标准的使用方式，我们需要在每个使用的地方重复上面的几个步骤。不知不觉，Lua 代码会变的庞大起来。

通过上面的步骤，我们不难发现: 除了 Redis 操作不一样，从实例化 Redis 到最后的连接资源管理都是一样的。这时候，我们就可以使用通用的处理流去标准化它了！

在 OpenResty最佳实践 中有章节介绍了对 lua-resty-redis 的第二次简化封装跟需要注意的事项。但是，我还是利用了 Lua 支持函数式编程的特性做了一个更加简化的版本:



1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
37
38
39
40
41
42
43
44
45
46
47
48
49
50
51
52
53
54
55
```lua
localredis=require"resty/redis"
locallog=ngx.log
localERR=ngx.ERR
localsetmetatable=setmetatable
local_M={
_VERSION="1.0.0",
_AUTHOR="AndyAi"
}
localmt={__index=_M}
localfunctionerrlog(...)
log(ERR,"Redis:",...)
end
function_M.exec(self,func)
localred=redis:new()
red:set_timeout(self.timeout)
localok,err=red:connect(self.host,self.port)
ifnotokthen
errlog("Cannotconnect,host:"..self.host..",port:"..self.port)
returnnil,err
end
red:select(self.database)
localres,err=func(red)
ifresthen
localok,err=red:set_keepalive(self.max_idle_time,self.pool_size)
ifnotokthen
red:close()
end
end
returnres,err
end
function_M.new(opts)
localconfig=optsor{}
localself={
host=config.hostor"127.0.0.1",
port=config.portor6379,
timeout=config.timeoutor5000,
database=config.databaseor0,
max_idle_time=config.max_idle_timeor60000,
pool_size=config.pool_sizeor100
}
returnsetmetatable(self,mt)
end
return_M
```
从上面的代码中可以看出:

我们将 Redis 的操作看成一个函数，在这个函数执行之前进行实例化跟连接；在函数执行之后进行连接资源的管理。
而，在实际的使用过程中也变得非常简单:

```lua
localredis=require("resty.rediscli")
localred=redis.new({host="127.0.0.1"})
localres,err=red:exec(
function(red)
returnred:get("123456")
end
```
使用 Redis 的 Pipeline 也同样很简单:
```lua
red:exec(
function(red)
red:init_pipeline()
red:set(key,value)
red:expire(key,expires)
returnred:commit_pipeline()
end
```
怎么样? 我们使用 Lua 支持函数编程的特性恰到好处地将代码很好地简化了。这样，不管是我们在实现逻辑，还是在维护逻辑都可以把精力更多地放在业务实现上，而不是处理这些琐碎资源管理。


上面的代码可以在 GitHub 上获取: lua-resty-utils。另外，不需要担心它的稳定性！它已经被使用在 GrowingIO 的几个非常重要的服务上！