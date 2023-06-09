---
title: Trojan Install
tags:
  - Trojan
  - Linux
date: 2022-11-09
---

## Articles
- [BBR install](https://teddysun.com/489.html)
- [x-ui install](https://v2rayssr.com/x-ui.html)

<!-- more -->
### Install BBR
### Install x-ui


### Issue
## 突然无法连接
Maybe acme.sh 's key was expired, Check the process of generate key in xui install webpage


前言

sprov 这位大神开发的这套面板程序，很是方便，可以可视化的搭建SS、V2ray、Xray、Trojan等热门的协议，并且可以实时看到 VPS 的性能状态以及流量的使用情况。

那在经过将近两年的不断更新之后，V2-ui面板，迎来了一个比较大的转折——停止更新了。

sprov 大神又用 GO 语言重新开发了一套面板 X-UI。那这套面板相比原来的 V2-ui，兼容性更强，也便于维护， GO 语言的性能更好，而且内存占用也会相对的低一些。

视频观看



功能介绍

系统状态监控
支持多用户多协议，网页可视化操作
支持的协议：vmess、vless、trojan、shadowsocks、dokodemo-door、socks、http
支持配置更多传输配置
流量统计，限制流量，限制到期时间
可自定义 xray 配置模板
支持 https 访问面板（自备域名 + ssl 证书）
更多高级配置项，详见该项目的 GitHub：点击访问


准备工作

1、VPS 一台重置好主流的操作系统 （CentOS 7+、Ubuntu 16+、Debian 8+），作者使用：搬瓦工 VPS

2、域名一个，做好相关的解析，若是需要套用 CDN，请托管域名到 cloudflare ，不会请点击

安装 X-ui 面板

安装 & 升级 X-ui 面板

更新及安装组件

下面环境的安装方式，大家根据自己的系统选择命令安装就好了。

apt update -y          # Debian/Ubuntu 命令
apt install -y curl socat    #Debian/Ubuntu 命令
yum update -y          #CentOS 命令
yum install -y curl socat    #CentOS 命令
请注意！！！！！
以下的两个安装命令：
1 为官方原版，但是2021年8月就没有更新了。
2 为改版X-ui，更新速度值得点赞，加入了很多的新功能，包括最新的 Reality 协议、电报群通知等等功能。
大家自由选择，二选一，都可以正常的使用
1、官方原版安装及升级的一键代码

bash <(curl -Ls https://raw.githubusercontent.com/vaxilu/x-ui/master/install.sh)
2、改版X-ui

项目地址：点击访问

bash <(curl -Ls https://raw.githubusercontent.com/FranzKafkaYu/x-ui/master/install.sh)
申请 SSL 证书

若是不明白 SSL 证书为何物，请看这里：点击观看
若是不明白怎么申请 SSL 证书，请看 本期视频
若你不明白我在说什么，请直接看这里：点击观看
安装 Acme 脚本

curl https://get.acme.sh | sh
80 端口空闲的证书申请方式

自行更换代码中的域名、邮箱为你解析的域名及邮箱

~/.acme.sh/acme.sh --register-account -m xxxx@xxxx.com
~/.acme.sh/acme.sh  --issue -d mydomain.com   --standalone
安装证书到指定文件夹

自行更换代码中的域名为你解析的域名

~/.acme.sh/acme.sh --installcert -d mydomain.com --key-file /root/private.key --fullchain-file /root/cert.crt
节点配置及功能讲解



节点配置及龙能方面，请看 视频教程

V2-ui – X-ui数据迁移

安装完 X-ui 以后，输入以下代码，即可完成用户数据的迁移（不包含管理员账号及密码，仅针对已部署的节点部分）

x-ui v2-ui
迁移成功后请关闭 v2-ui 并且重启 x-ui，否则 v2-ui 的 inbound 会与 x-ui 的 inbound 会产生端口冲突，若是有问题，请看 视频教程