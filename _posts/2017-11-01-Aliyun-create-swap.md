---
layout: post
comments: true
title:  "阿里云 CentOS 7 创建 swap 分区"
categories: aliyun
tags:  aliyun mac centos
date: 2017/11/01 09:45:42
---

* content
{:toc}

阿里云 CentOS 7 创建 swap 分区




Aliyun： CentOS 7.4 64 位

阿里云的低配主机是没有配置 swap 分区（不知道高版的有没有？）

## 添加 swap 分区

1. 查看磁盘分区：

`$ free -m`

2. 创建 swap 分区

`$ sudo dd if=/dev/zero of=/home/swap bs=1024 count=1048576`

> 例如要设 1 GB 的虚拟内存，则将 count 设为：
> count = 1024MB * 1024 = 1048576

3. 格式化 swap 分区

`$ sudo mkswap /home/swap`

4. 启用 swap 分区

`$ sudo swapon /home/swap`

5. 设置服务器重启之后 swap 仍然有效

`# echo "/swap/swap    swap    swap  defaults    0 0"  >> /etc/fstab`

> 这个命令用 sudo 还执行不了，最后切到 root 才可以执行。


## 删除分区

1. 停止 swap 分区

`$ sudo swapoff /home/swap`

2. 删除 swap 分区文件

`$ sudo rm -fr /home/swap`

3. 删除设置

`$ sudo sed  -i "/'\/swa\/swap   swap   swap  defaults 0 0'//"  /etc/fstab`


## Links

* [在阿里云CentOS 7创建swap分区](https://blog.tanteng.me/2016/03/aliyun-centos-7-swap/)
* [阿里云centos增加swap(虚拟内存)](http://www.cnblogs.com/jifeng/p/4422142.html)
