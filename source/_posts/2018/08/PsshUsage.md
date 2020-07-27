---
title: Pssh Usage
tags:
  - SSH
  - Linux
date: 2018-08-06
---

When there are many server need to manage , maybe you need to use pssh to manage your servers.

First install pssh.

<!-- more -->

Then create a hosts.txt which include all the server you need to manage . just like this
```
# this is the command in the host file
ubuntu@127.0.0.1:80
# command2
ubuntu@127.0.0.2:80
```

Then you can use this file like this:
```
$ pssh -i -h hosts.txt "echo 'hello'"
```
Option i can output the std output on your screen;option -h cat you hosts file.

Then after the command you want to execute 

Or you can use like this:
```
$ pssh -h hosts.txt -o output.log -e error.log "echo 'hello'"
```
Option o to write the std output in the output.log file; Option e to write the err output to error.log file