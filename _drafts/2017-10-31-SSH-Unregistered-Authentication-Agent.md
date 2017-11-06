---
layout: post
comments: true
title:  "SSH 连接报错：Unregistered Authentication Agent"
categories: ssh
tags:  centos ssh
date: 2017/10/31 10:34:35
---

* content
{:toc}

SSH 连接报错：Unregistered Authentication Agent




## 问题

Aliyun： CentOS 7.4 64 位
Mac： Sierra v10.12

昨天还一切正常，今天再连就连不上了，查看 log 信息，发现错误：

```
$ sudo tail -f /var/log/secure

Unregistered Authentication Agent for unix-process ...... (disconnected from bus)
```


## 再检查

```
$ sudo ssh -v
```

后来发现是网络的问题，应该是单位的网络限制了 SSH 连接，因为一回到家就可以了。

如何解决？

## Links

* [Securing OpenSSH: 7. Use Public/Private Keys for Authentication](https://wiki.centos.org/HowTos/Network/SecuringSSH)
* [Ubuntu 12.04 上使用 Nginx Passenger 部署 Ruby on Rails](https://github.com/ruby-china/homeland/wiki/Ubuntu-12.04-%E4%B8%8A%E4%BD%BF%E7%94%A8-Nginx-Passenger-%E9%83%A8%E7%BD%B2-Ruby-on-Rails)
* [在Aliyun上快速部署Ruby on Rails](https://ruby-china.org/topics/17553)
* [是否必须每次添加ssh-add](https://segmentfault.com/q/1010000000835302)
