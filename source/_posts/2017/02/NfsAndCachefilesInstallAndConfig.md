---
title: NFS And Cachefiles Install And Config
tags:
  - NFS
date: 2017-02-23
---

## Installtion
* For every host you need to use nfs, you'd better install these two soft
```shell
yum install -y nfs-utils cachefilesd
```

<!-- more -->

* Startup service
```
systemctl enable nfs-server
systemctl start nfs-server
systemctl enable cachefilesd
systemctl start cachefilesd
```

## For nfs server host 
* Config nfs permission
```
# cat /etc/exports
/test 10.72.2.0/24(rw,async,all_squash,anonuid=1000,anongid=1000)
```
* Reload the exports config
```shell
exportfs -ra
```

## For client
* This is the command-line mount
```
mount -t nfs -o nosuid,noexec,nodev,rw,bg,soft,fsc,rsize=32768,wsize=32768 10.72.2.24:/test /test
```
* And put it to fstab is better
```
# cat /etc/fstab
10.72.2.24:/test /test nfs defaults,nosuid,noexec,nodev,rw,bg,soft,fsc,rsize=32768,wsize=32768 0 0
```
* reload fstab
```shell
mount -a
```

## TIPS
* After reconfig fstab, if you want cachefiles take effect, you need to unmount first.
* If you link the mounted device to docker before, And then after you change mounte config just like fstab and reload, you need to restart your docker container as well
* For mac system, there maybe an error
```
mount_nfs: can't mount /home/vagrant/Public/code from 192.168.1.201 onto /Users/brady/Public/code2: Operation not permitted
```
To solve this, add insecure option in nfs server config
```
/test 10.72.2.0/24(…,insecure)
```
Or add a 'resvport' option when you mount to mac
```
sudo mount -t nfs -o resvport 192.168.199.201:/data/code /data/code
```
* If you want your mac as nfs server, you need to config /etc/exports like this:
```
/data/code -mapall=username:wheel -network 192.168.8.0 -mask 255.255.255.0
```
Auto start nfs for mac
```
sudo nfsd enable
sudo nfsd start
```

# For Ubuntu
Ref [link](https://www.digitalocean.com/community/tutorials/how-to-set-up-an-nfs-mount-on-ubuntu-20-04)