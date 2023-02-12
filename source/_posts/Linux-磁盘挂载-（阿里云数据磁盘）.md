title: Linux 磁盘挂载 （阿里云数据磁盘）
toc: true
cover: /gallery/covers/CP77-COVER.jpg
author: Zzzang
tags:
  - WebDav
categories:
  - WebDav
date: 2023-02-06 01:36:00
---
[参考链接 https://blog.csdn.net/ju_362204801/article/details/122873049](https://blog.csdn.net/ju_362204801/article/details/122873049)

一、查看挂载之前的情况
<!--more-->

1、检查现在磁盘情况
使用 `df -h`  命令来查看一下磁盘情况
![title](https://www.zzzang.cn//api/file/getImage?fileId=62de60195cfbab673300015e)
上图就是我没对单独购买的硬盘进行挂载之前用 df -h 查看的情况

可以看到，没挂载之前确实是只显示了一块40G的硬盘，没显示另外一块40G的硬盘

2、查看硬盘个数及分区
使用 `fdisk -l`  命令来查看一下硬盘个数和分区

![title](https://www.zzzang.cn//api/file/getImage?fileId=62de60305cfbab6733000160)
从上图可以看到2块硬盘都在

一块是 /dev/vda 这一块有分区

另外一块是 /dev/vdb 这一块没分区

你买了以后，两块硬盘就都在的，只不过是另外一块，你没设置分区挂载，还不能用

所以，下边我们操作一下分区、挂载

二、进行分区挂载
1、使用  `fdisk /dev/vdb`    命令进行分区
![title](https://www.zzzang.cn//api/file/getImage?fileId=62de60435cfbab6733000161)

2、再次查看磁盘个数及分区 `fdisk -l`
![title](https://www.zzzang.cn//api/file/getImage?fileId=62de604a5cfbab6733000162)

发现 最下面比刚才多了一个分区

3、格式化新分区
使用命令   `mkfs.ext3 /dev/vdb`  来格式化新分区（使用ext3扩展文件系统）
![title](https://www.zzzang.cn//api/file/getImage?fileId=62de60525cfbab6733000163)

4、创建挂载目录
cd /  进入最外一层根目录，在根目录下 使用 mkdir 命令来创建一个你想要的新目录，我这里新目录取名为extra
`mkdir extra`


5、挂载分区到刚才创建的目录
使用 `mount /dev/vdb /extra/`  命令进行挂载
![title](https://www.zzzang.cn//api/file/getImage?fileId=62de60605cfbab6733000164)
然后再次 `df -h` 看一下，就可以看到新的硬盘分区已经挂载上去了

`lsblk -f` 命令可以列出文件系统块设备，且能显示设备的 UUID 值。
```
[root@iZwz9exnhbzxes54x0ca6lZ ROOT]# lsblk -f
NAME   FSTYPE LABEL UUID                                 MOUNTPOINT
vda                                                      
└─vda1 ext4         aa2763ec-3885-4ef2-ba3c-76bbd5f1cb79 /
vdb    ext3         fcdefb8d-eb6d-4669-afe9-0e72c145080e /data

```


6、设置开机自动挂载
使用命令 `vim /etc/fstab`

把  /dev/vdb /extra ext3 defaults 0 0
![title](https://www.zzzang.cn//api/file/getImage?fileId=62de606e5cfbab6733000165)
这个内容写进去，然后保存

 用命令 `reboot` 重启一下服务器就ok了
 

 
 7.取消挂载
取消目录占用
```
fuser -k /file1
fuser -k /dev/sdb1
```
取消挂载点
```
umount /file1
umount /dev/sdb1
```