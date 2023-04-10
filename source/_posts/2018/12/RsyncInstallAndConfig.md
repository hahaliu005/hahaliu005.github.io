---
title: Rsync Install And Config
tags:
  - Rsync
  - Linux
date: 2018-12-05
---

rsync is installed in centos 7 by default.
- Start rsync daemon
```
rsync --no-detach --daemon
```

<!-- more -->

- cat /etc/rsyncd.conf
```
[test]
path = /data/test
comment = This a a test comment
auth users = test
secrets file = /etc/rsyncd.secrets
```

- cat /etc/rsyncd.secrets
```
test:111
```
> Notice: /etc/rsyncd.secrets must set permission to 600 `chmod 600 /etc/rsyncd.secrets`

-----

Then you can connect to rsync server use rsync client
```
rsync -av --password-file=/etc/rsyncd.pass test@[hostIp]::test /data/test
```
> /etc/rsyncd.pass 仅需要放置密码文本
> Notice: /etc/rsyncd.pass store the password of test user, you must set permission to 600 `chmod 600 /etc/rsyncd.pass`
> --compress, -z           compress file data during the transfer


传输时输出详细信息，传输时压缩，删除的文件还会保留在备份服务器的文件夹中，-a相当于-rlptgoD
```
rsync -vza -e ssh brady@192.168.8.115:/data /databak
```

传输时带ssh端口
```
rsync -vza -e 'ssh -p 8194' root@62.210.140.106:/data/resources /data
```

静默模式，传送时压缩，使用ssh方式，同时删除多余的文件
```
rsync -qza -e ssh —delete brady@192.168.8.115:/data /databak
```

> 删除多余的文件，使用`--delete`，例如：`rsync --delete [from] [target]`