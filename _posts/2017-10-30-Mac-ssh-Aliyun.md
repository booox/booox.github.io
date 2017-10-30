---
layout: post
comments: true
title:  "Mac 主机远程 ssh 阿里云 CentOS 主机"
categories: mac
tags:  aliyun mac centos
date: 2017/10/30 21:24:17
---

* content
{:toc}

Mac 主机远程 ssh 阿里云 CentOS 主机




## 环境

Aliyun： CentOS 7.4 64 位
Mac： Sierra v10.12

## 直接用 iTerm2 的 ssh 连接

使用 **iTerm2**

1. 添加一个配置文件

iTerm2 > Preferences > Profiles
（`CMD + ,` > Profiles）

修改 Name 与 Command (选中)

Command: `ssh [-p port-number] root@aliyun-host-ip-address`

如果是普通用户，则修改此处，如： `ssh user-name@aliyun-host-ip-address`

（因为忽略了这个地方，也折腾了好一会儿）

2. 连接

`CMD + O` 打开 Profiles 选项卡，选择上面添加的配置文件

经过上面的操作，就可以通过 ssh 连接到阿里云的服务器了。

## 用公钥与私钥来验证

1. mac 主机（Client）

```
$ cd ~/.ssh
$ ssh-keygen -t rsa -C "备注信息，可写可不写"   # 生成公钥、私钥对
$ ssh-add id_rsa
$ pbcopy < ~/.ssh/id_rsa.pub
```

这里复制的内容后面要用到。

其中：

* `ssh-keygen -t rsa` 会生成两个文件：*id_rsa* 与 *id_rsa.pub*
*  *id_rsa* 为私钥； *id_rsa.pub* 为公钥
* `ssh-add` 的作用只是把我们的私钥添加到 *ssh-agent* 所管理的一个 *session* 当中。(具体参考：[是否必须每次添加ssh-add](https://segmentfault.com/q/1010000000835302))
* `pbcopy` 一个特殊的管道复制命令，等下可以直接在 iTerm2 中用 `CMD + V` 来粘贴。

2. 阿里云的 CentOS 主机

为了安全因素，通常不直接用 root 身份来访问 ssh 或安装软件等
所以，需要创建一个专门的用户，假定为 `deploy`
先在阿里云控制台地方，单击“远程连接”，会是 `root` 身份

* 创建新用户

`# useradd -m -s /bin/bash deploy`

* 设置密码:

`# passwd deploy`

* 添加到 `sudo` 组:

`# vi /etc/sudoers`

  复制 `root ALL=(ALL) ALL`，并修改为：

  `deploy ALL=(ALL) ALL`

* 进入用户目录

```
# su - deploy
$ mkdir ~/.ssh
$ vi ~/.ssh/authorized_keys
```

将前面复制的 `id_rsa.pub` 的内容，粘贴到这里
先按 ESC，再输入 `:wq` 后保存退出。

* 设置权限

```
$ chmod 700 ~/.ssh
$ chmod 600 ~/.ssh/authorized_keys
```

这是由于 */etc/ssh/sshd_config* 中 `StrictMedes` 的默认设置所要求的。

**这里也是一个坑，折腾了一个多小时，都是泪**


## 设置阿里云的 sshd_config

`# vi /etc/ssh/sshd_config`

找到并设置如下：

```
PermitRootLogin no
AllowUsers deploy   # 在最后添加
```

测试是否可以用公钥/私钥对验证
如果可以，则将密码验证也去掉：

`PasswordAuthentication no`

终于，可以好好玩耍了。


## Links

* [Securing OpenSSH: 7. Use Public/Private Keys for Authentication](https://wiki.centos.org/HowTos/Network/SecuringSSH)
* [Ubuntu 12.04 上使用 Nginx Passenger 部署 Ruby on Rails](https://github.com/ruby-china/homeland/wiki/Ubuntu-12.04-%E4%B8%8A%E4%BD%BF%E7%94%A8-Nginx-Passenger-%E9%83%A8%E7%BD%B2-Ruby-on-Rails)
* [在Aliyun上快速部署Ruby on Rails](https://ruby-china.org/topics/17553)
* [是否必须每次添加ssh-add](https://segmentfault.com/q/1010000000835302)
