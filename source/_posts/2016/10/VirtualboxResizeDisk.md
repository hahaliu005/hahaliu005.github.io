---
title: Virtualbox中调整虚拟机的硬盘大小
tags:
  - Virtualbox
date: 2016-10-31
---

源文链接: http://boluns.blog.163.com/blog/static/69845968201411691347597/

<!-- more -->

由于经常需要在Linux虚拟机下测试一些环境，安装的东西比较多，导致之前分配给Linux虚拟机中的某个分区的空间不够用了，但是又不想重装系统，想直接给空间不足的那个分区调整空间。用的是Ubuntu系统，当时在安装的时候记得当时默认采用LVM来管理硬盘空间的，所以在网上搜索被很多资料，发现是可以动态调整LVM中的逻辑卷大小的，即然可以采用调整LVM的方式来增加分区空间，那么理论上肯定是可行的，于是就到处找资料，做试验验证实际是否可行。
先说一下我的系统环境：
宿主机是：xp sp3
虚拟机软件是：Oracle VM VirtualBox 4.1.16
虚拟机Guest系统是：Ubuntu 10.04 Server x32

虚拟机Ubuntu当时安装时是采用默认的LVM硬盘管理(这个是基础，如果当时没有采用LVM管理的话，也不能动态调整分区的大小了)。当时在VirtualBox中给Ubuntu分配的是8G的空间，固定大小的，随着使用情况的变化，8G的空间很快就用完被，必须增加新的空间。

主要步骤如下：

1：在VirtualBox调整虚拟机硬盘的大小

2：在虚拟机中用fdisk命令将新加的硬盘空间分区，新分区的类型要是LVM的类型8e

3：在虚拟机新建物理卷、卷组、合并卷组、扩展逻辑卷的大小

此方法是己经验证成功了的，中间只重启过一次机器，而且文件都没有损坏(不过在正式环境修改之前，各位自己必须做备份)，详细的过程比较长，如下：

上面是我环境的基本信息，下面就说明一下具体的操作过程：

先把调整之前的硬盘情况列出来：

root@bogon:/home/roger# df -h
Filesystem            Size  Used Avail Use% Mounted on
/dev/mapper/bogon-root 6.7G  1.6G  4.7G  26% /
none                  494M  212K  494M   1% /dev
none                  501M     0  501M   0% /dev/shm
none                  501M  328K  501M   1% /var/run
none                  501M     0  501M   0% /var/lock
/dev/sda1             228M   23M  194M  11% /boot


root@bogon:/home/roger# fdisk -l

Disk /dev/sda: 8589 MB, 8589934592 bytes
255 heads, 63 sectors/track, 1044 cylinders
Units = cylinders of 16065 * 512 = 8225280 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x0005d850

Device Boot      Start         End      Blocks   Id  System

/dev/sda1   *           1          32      248832   83  Linux
Partition 1 does not end on cylinder boundary.
/dev/sda2              32        1045     8136705    5  Extended
/dev/sda5              32        1045     8136704   8e  Linux LVM

从上面可以看到，在调整之前，我的硬盘设备是/dev/sda，大小只有8589 MB，而且只有/dev/sda1、/dev/sda2、/dev/sda5，这个要先记录下来，因为后面增加硬盘后，还需要建立新的分区。


1：首先将虚拟机的硬盘大小限制放大，即将8G的大小调为20G。由于在VirtualBox不能直接调整虚拟机的硬盘空间，需要调用相应的命令来实现。先将虚拟机关闭，启动CMD窗口，切换到VirtualBox的安装路径下面(例如我的VirtualBox安装路径是D:\tools\virtualbox)：

D:\tools\virtualbox>VBoxManage.exe clonehd D:\tools\virtualbox\vm\ubuntu.vdi D:\tools\virtualbox\vm\ubuntu20g.vdi --format vdi
0%...10%...20%...30%...40%...50%...60%...70%...80%...90%...100%
Clone hard disk created in format 'vdi'. UUID: b5bf984b-decf-47f1-9568-1919241b348c

D:\tools\virtualbox>VBoxManage.exe modifyhd  D:\tools\virtualbox\vm\ubuntu20g.vdi --resize 20480
0%...10%...20%...30%...40%...50%...60%...70%...80%...90%...100%

在CMD下面依输入上面粗体的两条命令
说明一下：
D:\tools\virtualbox\vm\ubuntu.vdi ---------这个是我虚拟机的镜象文件
D:\tools\virtualbox\vm\ubuntu20g.vdi ---------这个是我后面准备调整大小的镜象文件，尽量不要直接修改原文件，否则一旦出错将导致原来的系统无法使用
VBoxManage.exe modifyhd  D:\tools\virtualbox\vm\ubuntu20g.vdi --resize 20480，这行命令中的 --resize参数就是说明调整后的大小是多少，单位是M，例子中20480即20480M，即20G大小
经过上面两条命令后，我们得到一个新的、大小限制为20G的虚拟机镜象文件，此时在VirtualBox里面，将这个修改后的镜象文件替换掉之前的那个镜象文件，具体过程就是在VirtualBox虚拟机的“设置->存储->存储树”里面将之前的设备删除掉，然后再添加 “XXX控制器” 并 “添加虚拟硬盘” 时选择 "使用现有的虚拟盘" 为我们调整了大小的那个VDI文件，然后启动虚拟机Ubuntu，并切换到root帐号：

2：为新增的硬盘建立支持LVM的分区。
虚拟机的硬盘大小限制己经修改了，但是在虚拟机里面，用 fdisk -l 可以看到硬盘的大小是变了，但是分区的大小还是没有变的。因为分区的大小是在当时安装系统是就分好了的，现在就要来重新调整分区的大小了。
先查看一下硬盘大小，看是否增加了


root@bogon:/home/roger# fdisk -l

Disk /dev/sda: 21.5 GB, 21474836480 bytes
255 heads, 63 sectors/track, 2610 cylinders
Units = cylinders of 16065 * 512 = 8225280 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x0005d850

   Device Boot      Start         End      Blocks   Id  System
/dev/sda1   *           1          32      248832   83  Linux
Partition 1 does not end on cylinder boundary.
/dev/sda2              32        1045     8136705    5  Extended
/dev/sda5              32        1045     8136704   8e  Linux LVM

粗体的信息说明我们调整的硬盘大小在Ubuntu中己经可以看到了，但是具体的分区还是没有体现出来，现在我们就要将新加的硬盘建一个分区


root@bogon:/home/roger# fdisk /dev/sda

WARNING: DOS-compatible mode is deprecated. It's strongly recommended to
         switch off the mode (command 'c') and change display units to
         sectors (command 'u').

Command (m for help): n  ---这里输入n，表示要创建新的分区，输入之后的提示如下
Command action
   l   logical (5 or over)
   p   primary partition (1-4)
p --- 这里输入p，表示创建主分支，输入之后的提示如下
Partition number (1-4): 3 --- 这里输入一个1到4范围内的数字，另外由于我们己经有/dev/sda1、/dev/sda2、/dev/sda5这些设备了，所有这里只能输入3或4，输入3之后有下面的提示

First cylinder (1045-2610, default 1045): ---这里直接回车确认使用默认值即可，然后出现类似的提示
Using default value 1045
Last cylinder, +cylinders or +size{K,M,G} (1045-2610, default 2610):---这里同样直接回车确认使用默认值即可，然后出现类似的提示
Using default value 2610

Command (m for help): p  ---这里可以输入p命令将当前分区情况打印出来，类似以下信息

Disk /dev/sda: 21.5 GB, 21474836480 bytes
255 heads, 63 sectors/track, 2610 cylinders
Units = cylinders of 16065 * 512 = 8225280 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x0005d850

   Device Boot      Start         End      Blocks   Id  System
/dev/sda1   *           1          32      248832   83  Linux
Partition 1 does not end on cylinder boundary.
/dev/sda2              32        1045     8136705    5  Extended
/dev/sda3            1045        2610    12577241   83  Linux
/dev/sda5              32        1045     8136704   8e  Linux LVM

看到粗体的内容没？/dev/sda3就是新增加硬盘空间的分区，但是要注意，此时这个分支的Id为83，而我们后面要用到的LVM设备的Id是8e，所以在这里我们必须调整/dev/sda3的类型，用fdisk中的t命令来进行修改：

Command (m for help): t ---这里可以输入t命令调整/dev/sda3的Id类型，类似以下信息

Partition number (1-5): 3  ---这里要说要修改哪个分区的设备，要求输入设备的编号，我们要修改的设备是/dev/sda3，所以这里输入3，然后有下面类似的提示

Hex code (type L to list codes): 8e ---这里是输入要修改的类型代码，LVM设备的代码是8e，所在这里我们就输入8e，然后出现类似的提示

Changed system type of partition 3 to 8e (Linux LVM)

Command (m for help): --- 此时就可以输入w命令保存并退出，出现类似提示
Command (m for help): w
The partition table has been altered!

Calling ioctl() to re-read partition table.

WARNING: Re-reading the partition table failed with error 16: Device or resource busy.
The kernel still uses the old table. The new table will be used at
the next reboot or after you run partprobe(8) or kpartx(8)
Syncing disks.


此时新的分区己经创建，但是如果要系统内核能够识别到，则需要重启系统，

root@bogon:/home/roger# reboot 重启系统，

系统重启后，查看一下新建的分区是否己经识别到：


root@bogon:/home/roger# fdisk -l

Disk /dev/sda: 21.5 GB, 21474836480 bytes
255 heads, 63 sectors/track, 2610 cylinders
Units = cylinders of 16065 * 512 = 8225280 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x0005d850

   Device Boot      Start         End      Blocks   Id  System
/dev/sda1   *           1          32      248832   83  Linux
Partition 1 does not end on cylinder boundary.
/dev/sda2              32        1045     8136705    5  Extended
/dev/sda3            1045        2610    12577241   8e  Linux LVM
/dev/sda5              32        1045     8136704   8e  Linux LVM

这说明新的分区己经被内核识别到了，下一步的关键就是创建物理卷、卷组、合并卷组了

3：调整LVM

给新建的分区创建物理卷

root@bogon:/home/roger# pvcreate  /dev/sda3
  Physical volume "/dev/sda3" successfully created

新建卷组

root@bogon:/home/roger# vgcreate ubuntu20 /dev/sda3
  Volume group "ubuntu20" successfully created

ubuntu20是新卷组的名称

合并卷组，先查看一下当前有哪些卷组

root@bogon:/home/roger# vgscan
  Reading all physical volumes.  This may take a while...
  Found volume group "bogon" using metadata type lvm2
  Found volume group "ubuntu20" using metadata type lvm2

这里说明当前有bogon、ubuntu20两个卷组，bogon这个卷组是当时安装系统时自动建的，ubuntu20这个就是我们刚才建的卷组，现在将这两个卷组合并

root@bogon:/home/roger# vgmerge bogon ubuntu20
  Volume group "ubuntu20" successfully merged into "bogon"

现在己经将两个卷组合并成一个了，那么最后我们就是要来调整分区的大小，先看一下调整之前分区的大小情况，调整之前/dev/mapper/bogon-root的大小是6.7G

root@bogon:/home/roger# df -h
Filesystem            Size  Used Avail Use% Mounted on
/dev/mapper/bogon-root 6.7G 1.6G  4.7G  26% /
none                  494M  216K  494M   1% /dev
none                  501M     0  501M   0% /dev/shm
none                  501M  328K  501M   1% /var/run
none                  501M     0  501M   0% /var/lock
/dev/sda1             228M   23M  194M  11% /boot

准备开始调整逻辑卷的大小了

root@bogon:/home/roger# lvextend -l+100%FREE /dev/mapper/bogon-root
  Extending logical volume root to 18.75 GiB
  Logical volume root successfully resized

动态调整/dev/mapper/bogon-root的大小，否则看到的还是之前的大小

root@bogon:/home/roger# resize2fs -p /dev/mapper/bogon-root
resize2fs 1.41.14 (22-Dec-2010)
Filesystem at /dev/mapper/bogon-root is mounted on /; on-line resizing required
old desc_blocks = 1, new_desc_blocks = 2
Performing an on-line resize of /dev/mapper/bogon-root to 4916224 (4k) blocks.
The filesystem on /dev/mapper/bogon-root is now 4916224 blocks long.

最后一步，激动人心的时刻来了

root@bogon:/home/roger# df -h
Filesystem            Size  Used Avail Use% Mounted on
/dev/mapper/bogon-root 19G  1.6G   16G  10% /
none                  494M  216K  494M   1% /dev
none                  501M     0  501M   0% /dev/shm
none                  501M  328K  501M   1% /var/run
none                  501M     0  501M   0% /var/lock
/dev/sda1             228M   23M  194M  11% /boot