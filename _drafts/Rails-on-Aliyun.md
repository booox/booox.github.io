---
layout: post
comments: true
title:  "在阿里云上部署 Rails"
categories: rails
tags:  rails aliyun
date: 2017/10/30 22:41:56
---

* content
{:toc}

在阿里云上部署 Rails



## 连接 esc

### 连接方法

1. 直接通过 web 页面的远程连接
2. 在 mac 的 iTerm2 中输入
`$ ssh root@aliyun-ip-address`

### 安全连接

出于安全的考虑，一般不允许 root 访问 ssh 。

先用 root 登陆上去，创建新用户 deploy：

`# useradd -m -s /bin/bash deploy`
设置密码
`# passwd deploy`



添加到 sudo 组中：

`# vi /etc/sudoers`

修改
 ```
 root ALL=(ALL) ALL
 deploy ALL=(ALL) ALL
 ```



****


## 安装 RVM 与 Ruby

1. 更新 yum，并安装 curl

```
# centos
$ sudo yum install -y epel-release yum-utils
$ sudo yum-config-manager --enable epel
$ sudo yum clean all && sudo yum update -y
$ sudo yum install curl

# ubuntu
apt-get update
apt-get upgrade
```

安装开发工具

```
# centos
$ sudo yum groupinstall -y 'development tools'
$ sudo yum install -y git-core zlib zlib-devel gcc-c++ patch readline readline-devel libyaml-devel libffi-devel openssl-devel make bzip2 autoconf automake libtool bison curl sqlite-devel
$ sudo yum install -y nginx
$ sudo yum install -y postgresql-server postgresql-contrib postgresql-devel

# ubuntu
apt-get install autoconf bison build-essential libssl-dev libyaml-dev libreadline6-dev zlib1g-dev libncurses5-dev libffi-dev libgdbm3 libgdbm-dev git-core curl libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev libcurl4-openssl-dev python-software-properties
apt-get install nginx
apt-get install postgresql postgresql-contrib
```

2. 安装 RVM:

```
$ \curl -sSL https://get.rvm.io | bash -s stable
```

> 安装时报错： `GPG signature verification failed .... Try to install GPG v2 and then fetch the public key: gpg2 --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3`

重新执行：

```
$ gpg2 --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3
$ \curl -sSL https://get.rvm.io | bash -s stable
```

安装成功，并提示

> To start using RVM you need to run `source /home/deploy/.rvm/scripts/rvm`

```
$ source ~/.rvm/scripts/rvm
```

执行 `rvm -v` ，若显出版本号，则 rvb 安装完成。

3. 安装 Ruby

修改 RVM 的 Ruby 安装源

`$ echo "ruby_url=https://cache.ruby-china.org/pub/ruby" > ~/.rvm/user/db`

列出已知的 Ruby 版本

`$ rvm list known`

 安装一个 Ruby 版本

 `$ rvm install 2.3.1 --disable-binary`

 > 这一步，试了两次均没有成功，查看提示的 log 文件，有如下报错： `sudo -p '%p password required for '\''yum install -y` ,又看了 [快速部署Rails到阿里云](https://forum.qzy.camp/t/rails-ubuntu-centos/1852) 中先安装必要的开发工具，才意识到，这些开发工具的安装应该在 `sudo` 下使用。

> 安装好开发工具等之后，再来安装 Ruby 就成功了。

> 提示： `Ruby was built without documention, to build it run: rvm docs generate-ri`

可以通过查看 Ruby 的版本来验证是否安装成功：

`ruby -v`

## 使用 RVM 快速部署 Nginx + Passenger

首先安装 Passenger

`$ gem install passenger`

查看 passenger 版本： `$ passenger -v`

接着进入 rvmsudo 使用 Passenger 安装 Nginx

`$ rvmsudo passenger-install-nginx-module`

查看 gem 版本: `$ gem -v`
修改 gem 源地址：
  查看：`$ gem sources -l`
  删除：`$ gem sources -r https://rubygems.org/`
  查看：`$ gem sources -a http://mirrors.aliyun.com/rubygems/`

安装 Rails

```
$ gem install rails
$ rails -v
Rails 5.1.4
```

### 安装数据库

`$ sudo yum install -y postgresql-server postgresql-contrib postgresql-devel`

### 添加 swap 分区

```
$ free -m
$ sudo mkdir /swap/swap
$ sudo dd if=/dev/zero of=/swap/swap bs=1024 count=1048576
$ sudo mkswap /swap/swap
$ sudo swapon /swap/swap
$ su - root
# echo "/swap/swap    swap    swap  defaults    0 0"  >> /etc/fstab
# exit
$
```

详见 [阿里云 CentOS 7 创建 swap 分区]("./2017-11-01-Aliyun-create-swap.md")




## Links

* [Ubuntu 12.04 上使用 Nginx Passenger 部署 Ruby on Rails](https://github.com/ruby-china/homeland/wiki/Ubuntu-12.04-%E4%B8%8A%E4%BD%BF%E7%94%A8-Nginx-Passenger-%E9%83%A8%E7%BD%B2-Ruby-on-Rails)
* [How to use ActiveModel errors details](https://cowbell-labs.com/2015-01-22-active-model-errors-details.html)
* [ActiveModel::Errors < Object](http://api.rubyonrails.org/classes/ActiveModel/Errors.html#method-i-empty-3F)
