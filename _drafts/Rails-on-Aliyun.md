---
layout: post
comments: true
title:  "在阿里云上部署 Rails"
categories: rails
tags:  rails aliyun
date: 2017/10/30 18:12:05
---

* content
{:toc}

在阿里云上部署 Rails

## Mac 远程连接阿里云 (Aliyun) 的 ESC

使用 **iTerm2**

1. 添加一个配置文件

iTerm2 > Preferences > Profiles
（`CMD + ,` > Profiles）

修改 Name 与 Command (选中)

Command: `ssh [-p port-number] root@aliyun-ip-address`

2. 连接

`CMD + O` 打开 Profiles 选项卡，选择上面添加的配置文件

经过上面的操作，就可以通过 ssh 连接到阿里云的服务器了。

3. 设置安全连接

```
$ cd ~/.ssh
$ ssh-keygen -t rsa -C "备注信息"
$ ssh-add id_rsa
$ pbcopy < ~/.ssh/id_rsa.pub
```



## Links

* [Dig into the rails errors
Errors](http://neethack.com/2015/04/dig-into-the-rails-errors/)
* [How to use ActiveModel errors details](https://cowbell-labs.com/2015-01-22-active-model-errors-details.html)
* [ActiveModel::Errors < Object](http://api.rubyonrails.org/classes/ActiveModel/Errors.html#method-i-empty-3F)
