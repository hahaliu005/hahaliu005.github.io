---
title: Centos7 New Host Machine Init
tags:
  - CentOS
  - Linux
date: 2019-05-21
---

* Set hostname
```shell
hostnamectl set-hostname <server-name> && exec bash
```

* Set more secure password
```
$ head -c 32 /dev/urandom | base64 | head -c 16
$ passwd
```

<!-- more -->

* Some internal network do not default startup, check and config it
```shell
# cat /etc/sysconfig/network-scripts/ifcfg-eth0
onboot=yes
```
Then`systemctl restart network`

* Update repo and install some basic tools
```shell
yum -y install epel-release && \
yum -y update && \
yum -y groupinstall "Development Tools" && \
yum -y install tar vim wget net-tools htop tmux nload
```

* Init time
```shell
yum install ntp -y && \
systemctl start ntpd && \
systemctl enable ntpd && \
timedatectl set-timezone Asia/Shanghai && \
timedatectl set-local-rtc 0
```

* We have other tool for firewall, close default firewalld and iptables
```shell
systemctl stop firewalld && \
systemctl disable firewalld
```


* Change ssh to specify port for secure
```shell
sed -i 's/^#Port/Port/g' /etc/ssh/sshd_config && \
sed -i 's/^Port \(.\+\)/Port <ssh-port>/g' /etc/ssh/sshd_config && \
systemctl restart sshd.service
```

* Add ssh connection config for some server
```shell
# cat ~/.ssh/config
Host <server-name>
  HostName <ip-address>
  Port <port-number>
  User <user>
```
A convenience way to copy ssh key `ssh-copy-id <server-name>`

* For online server, add serial console in case that cann't ssh to server
```shell
systemctl enable getty@ttyS1.service && \
echo 'ttyS1' >> /etc/securetty && \
systemctl start getty@ttyS1.service
```

* If you have private gitlab, you can add ssh key to gitlab,  then config ssh
```shell
Host <gitlab-domain>
  HostName <ip-address>
  Port <git-prot>
  User <git-user>
```

* If for docker host, may create a user for docker use
```
useradd -u 1050 -U -m docker-user
```

* If for kubernetes host, may also close swap and selinux
Close swap
```
swapoff -a
sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab
cat /etc/fstab
```
* In some cases, we need to disable selinue
```
# This take effect immediately
setenforce 0
# This will only take effect after reboot
sed -i 's/^SELINUX=\(.\+\)/SELINUX=disabled/g' /etc/sysconfig/selinux
# Maybe /etc/sysconfig/selinux do not have effect
sed -i 's/^SELINUX=\(.\+\)/SELINUX=disabled/g' /etc/selinux/config
# Check current selinux status
sestatus
# Maybe you need to reboot check whether selinux had been disabled, or you will not login with your new ssh port
```

* Sometimes you need to install nfs utils and run it for nfs server or client
```
yum install -y nfs-utils cachefilesd
```

## Ubuntu

Set hostname
```shell
sudo hostnamectl set-hostname hostname && exec bash
```

### Install base tools
```
sudo apt -y update && \
sudo apt install -y build-essential net-tools nload
```

### Set up ufw, allow base ports
```
sudo ufw allow ssh
sudo ufw allow http
sudo ufw allow https
sudo ufw enable
```

### Install zsh and ohmyzsh
```
sudo apt install -y zsh && \
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```
Change theme in ~/.zshrc
```
ZSH_THEME="bira"
```

### Set timezone
```
sudo timedatectl set-timezone Asia/Shanghai
```

### Copy vim config to .vimrc, ref to VimConfig.md