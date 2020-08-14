---
title: CentOS 7安装配置vsftpd做FTP服务
tags:
  - Linux
  - CentOS
date: 2016-05-05
---

　　Linux菜鸟，还没用过FTP服务；在Windows下用的是Filezilla，一开始还在想Linux下也用它，因为知道它是开源的，在Linux下用它更显得理所当然了，后来发现网上介绍大多都vsftpd，所以就用vsftpd做FTP服务了。

安装

在安装前查看是否已安装vsftpd
[root@localhost ~]# rpm -q vsftpd
vsftpd-3.0.2-9.el7.x86_64

如果有显示类似以上的信息，说明已经安装vsftpd，如果没有，用yum安装：
[root@localhost ~]# yum -y install vsftpd

查看一下vsftpd安装在哪：
[root@localhost ~]# whereis vsftpd
vsftpd: /usr/sbin/vsftpd /etc/vsftpd /usr/share/man/man8/vsftpd.8.gz

启动vsftpd服务：
[root@localhost ~]# systemctl start vsftpd.service

配置

对于从未使用过vsftpd的Linux菜鸟来说，在配置之前，简单了解一下vsftpd服务的过程相当有必要的。安装完默认情况下是开启匿名登录的，对应的是 /var/ftp 目录，这时只要服务启动了，就可以直接连上FTP了。
pcvcdeiMac:~ pcvc$ ftp 192.168.0.117
Connected to 192.168.0.117.
220 (vsFTPd 3.0.2)
Name (192.168.0.117:pcvc): ftp
331 Please specify the password.
Password:
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls
229 Entering Extended Passive Mode (|||37296|).
150 Here comes the directory listing.
drwxr-xr-x    2 0        0               6 Jun 10  2014 pub
226 Directory send OK.
　　
然而，一般情况下我们都是按分配的用户去访问各自的目录，vsftpd的用户分为系统用户和虚拟用户，系统用户也就是系统实际存在的Linux用户，而虚拟用户则是存在于配置文件里面的。系统用户方式比较简单，创建系统用户，确保用户能对FTP目录进行读写就可以了。以下是创建普通Linux用户，详见 useradd 命令。其实，从以上连接ftp信息可以看出，有的系统已默认创建了名为ftp的用户了。
[root@localhost ~]# useradd -g root -M -d /var/www/html -s /sbin/nologin ftpuser
[root@localhost ~]# passwd ftpuser
[root@localhost ~]# 输入密码

把 /var/www/html 的所有权给ftpuser.root
[root@localhost ~]# chown -R ftpuser.root /var/www/html

这样就可以通过ftpuser用户连接FTP了。至于虚拟用户需要做的步骤就比较多了。首先虚拟用户的用户认证是通过pam方式去认证的，pam文件里面指定了认证的db文件，db文件又是通过明文用户名和密码文件生成而来，为何要这么绕我也不大清楚。那么首先我们要指定pam文件，这个在 /etc/vsftpd/vsftpd.conf 配置文件是通过 pam_service_name=vsftpd 配置指定的，其位置是 /etc/pam.d/vsftpd，编辑该文件，把文件内容全部注释掉，加上以下两行：
64位系统：
auth required /lib64/security/pam_userdb.so db=/etc/vsftpd/vuser_passwd
account required /lib64/security/pam_userdb.so db=/etc/vsftpd/vuser_passwd
32位系统：
auth sufficient /lib/security/pam_userdb.so db=/etc/vsftpd/vuser_passwd
account sufficient /lib/security/pam_userdb.so db=/etc/vsftpd/vuser_passwd

db=/etc/vsftpd/vuser_passwd 指定了db文件的位置，接下来就是生成db文件，由于db文件是通过明文用户名和密码文件生成而来，所以先创建一个保存明文用户名和密码的文件 vuser_passwd.txt
[root@localhost ~]# vi /etc/vsftpd/vuser_passwd.txt

该文件奇行为用户名，偶行为密码：
pcvc
iampasswd

然后通过以下命令生成db文件：
[root@localhost ~]# cd /etc/vsftpd
[root@localhost vsftpd]# db_load -T -t hash -f vuser_passwd.txt vuser_passwd.db

至此用户的认证就知道怎么一回事了，如果要添加新的用户，在编辑 vuser_passwd.txt 后要再次生成一下db文件。然后现在每个用户的具体配置，如指向目录、可读写权限等又是在哪配置的呢，原来它是通过一个用户对应一个配置文件来实现的，且这个文件必须用FTP用户名去做文件名，建一个目录专门存放这些文件：
[root@localhost vsftpd]# mkdir vuser_conf
[root@localhost vsftpd]# vi vuser_conf/pcvc

文件内容大致如下：
local_root=/var/www/pcvcnet
write_enable=YES
anon_umask=022
anon_world_readable_only=NO
anon_upload_enable=YES
anon_mkdir_write_enable=YES
anon_other_write_enable=YES

接下来就是根据需要和以上各文件信息来修改配置文件 /etc/vsftpd/vsftpd.conf 了，启用或更改以下配置的值：
# 禁用匿名登录
anonymous_enable=NO
ascii_upload_enable=YES
ascii_download_enable=YES
# 启用限定用户在其主目录下
chroot_local_user=YES

以下配置是需要自己手工添加：
# 设定启用虚拟用户功能
guest_enable=YES
# 指定虚拟用户的宿主用户，CentOS中已经有内置的ftp用户了
guest_username=ftpuser
# 虚拟用户配置文件存放的路径
user_config_dir=/etc/vsftpd/vuser_conf
# 如果启用了限定用户在其主目录下需要添加这个配置
allow_writeable_chroot=YES

用 systemctl restart vsftpd.service 重新启动服务即可用虚拟用户登录FTP了。

如果系统启用了防火墙和SELinux，那么还要做以下配置。

防火墙添加FTP服务：
[root@localhost vsftpd]# firewall-cmd --permanent --zone=public --add-service=ftp
[root@localhost vsftpd]# firewall-cmd --reload

设置SELinux：
[root@localhost vsftpd]# getsebool -a | grep ftp
[root@localhost vsftpd]# setsebool -P ftpd_full_access on

添加新 ftp 用户

1. 编辑 vuser_passwd.txt 文件，添加用户名和密码：
# cd /etc/vsftpd
# vi vuser_passwd.txt
new_ftp_user
new_ftp_user_password

2. 重新生成 vuser_passwd.db 文件：
# db_load -T -t hash -f vuser_passwd.txt vuser_passwd.db

3. 创建用户配置文件：
# cd vuser_conf
# vi new_ftp_user

配置文件内容：
local_root=/new/ftp/directory
write_enable=YES
anon_umask=022
anon_world_readable_only=NO
anon_upload_enable=YES
anon_mkdir_write_enable=YES
anon_other_write_enable=YES

4. 重启服务：
# systemctl restart vsftpd.service