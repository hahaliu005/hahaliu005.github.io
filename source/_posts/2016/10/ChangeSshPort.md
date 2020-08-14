---
title: Change ssh port
tags:
  - SSH
  - Linux
date: 2016-10-04
---

Open the ssh config file , change port config
vim /etc/ssh/sshd_config
```
Port 8194
```

将默认的防火墙切换为iptables:
```
yum install -y iptables-services
systemctl disable firewalld
systemctl mask firewalld
systemctl enable iptables
systemctl stop firewalld
systemctl start iptables
```

如果有iptables
```
vim /etc/sysconfig/iptables
-A INPUT -p tcp -m state --state NEW -m tcp --dport 22 -j ACCEPT
-A INPUT -p tcp -m state --state NEW -m tcp --dport 8194 -j ACCEPT
```

```
systemctl restart  iptables.service
systemctl restart sshd.service
```

Install semanage:
```
yum -y install policycoreutils-python
```

Open port use semanage:
```
semanage port -a -t ssh_port_t -p tcp 1234
```
上面这条会等很久，然后可能会出错，不用管，已经加上去了

List semange port:
```
semanage port -l | grep ssh
```

Open firewall:
```
firewall-cmd --permanent --zone=public --add-port=8194/tcp
```

Reload firewall:
```
firewall-cmd --reload
```

Restart sshd:
```
systemctl restart sshd.service
```

如果是iptables, 把firewall的步骤改为以下步骤：
```
vim /etc/sysconfig/iptables
-A INPUT -p tcp -m state --state NEW -m tcp --dport 22 -j ACCEPT
-A INPUT -p tcp -m state --state NEW -m tcp --dport 8194 -j ACCEPT
```

```
systemctl restart  iptables.service
systemctl restart sshd.service
```