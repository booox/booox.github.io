---
layout: post
comments: true
title:  "Rails 中用 Redis 搭建排行榜"
categories:  Rails
tags:  Rails Redis
date: 2017/11/12 06:10:55
---

* content
{:toc}

Rails 中用 Redis 搭建排行榜




因需要在网站在加入一个排行榜 ( leaderboard )，查了一下资料，说基本上都是用 Redis 来完成的。
于是记录一下学习过程。


## 安装 Redis

### 在 Mac OS X 上安装 Redis

安装( 需要 [brew](https://brew.sh/) ):

`brew install redis`

设置 redis 随系统自启动

`launchctl load ~/Library/LaunchAgents/homebrew.mxcl.redis.plist`

启动 redis ( 不使用 `launchctl` )

`redis-server /usr/local/etc/redis.conf`

检查 redis 是否运行

`redis-cli ping`

  **PONG** 表示成功运行

配置文件：

*/usr/local/etc/redis.conf*

卸载 redis

`brew uninstall redis; rm ~/Library/LaunchAgents/homebrew.mxcl.redis.plist`


### 在 Ubuntu上安装 Redis

ref: [How to Install and configure Redis on Ubuntu 14.04](https://hostpresto.com/community/tutorials/how-to-install-and-configure-redis-on-ubuntu-14-04/)

`$ sudo apt-get update`
`$ sudo apt-get upgrade`
`$ sudo apt-get install redis-server`
`$ sudo service redis-server status`

或查看默认端口是否启用

```
$ sudo netstat -naptu | grep LISTEN
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      887/sshd        
tcp        0      0 127.0.0.1:6379          0.0.0.0:*               LISTEN      20478/redis-server
tcp6       0      0 :::22                   :::*                    LISTEN      887/sshd        
```

也可通过 redis 源码来进行安装

ref: [How To Install and Configure Redis on Ubuntu 16.04](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-redis-on-ubuntu-16-04)

## 学习基本语法

目标：用 **Sorted Sets** 来维护一个高分列表，假定名称为 `leaderboard`。
  所谓 **Sorted Sets** 就是一个字符串集合，它可以根据你设置的分数来排序。

命令：

* `zadd`: 来添加或更新得分，　
* `zrank`:　获取一个玩家的排名，正序
* `zrevrank`:　获取一个玩家的排名，倒序
* `zrange`:　获取玩家排名列表
* `zrevrange`:　获取玩家排名列表
* `zscore`:　获取玩家排名得分 `zscore theboard "Bob"`
* `zincrby`:　给玩家　Bob 增加 10 分 `zincrby theboard 10 "Bob"`
  * 如果玩家不存在，则添加
* `DEL` : 删除一个 key


1. 连接到 redis

`$ redis-cli`

2. 使用 `zadd` 添加记录

语法：`zadd [set name] [score] [string]`

```
zadd leaderboard 1 "Alice"
zadd leaderboard 30 "Bob"
zadd leaderboard 5300 "Carol"
```

3. 用 `zrevrange` 倒序

语法：`zrevrange [set name] [first position] [second position]`
注意：如果想从第一个开始，则 first position 要设为 `0`

```
zrevrange leaderboard 0 2
zrevrange leaderboard 0 99

127.0.0.1:6379> zrevrange leaderboard 0 2
1) "Carol"
2) "Bob"
3) "Alice"
```

同时也输出得分

```
127.0.0.1:6379> zrevrange leaderboard 0 2 withscores
1) "Carol"
2) "5300"
3) "Bob"
4) "30"
5) "Alice"
6) "1"
```

再添加一个玩家

`zadd leaderboard 25 "Don"`

再次获取前三名

```
127.0.0.1:6379> zrevrange leaderboard 0 2
1) "Carol"
2) "Bob"
3) "Don"
```

4. 用 `zadd` 来更新得分

`zadd leaderboard 31 "Alice"`

重新获取前三名

```
127.0.0.1:6379> zrevrange leaderboard 0 2
1) "Carol"
2) "Alice"
3) "Bob"
```

5. 用 `zrem` 来删除数据

语法：`zrem [set name] [string]`

```
zrem leaderboard "Carol"

127.0.0.1:6379> zrevrange leaderboard 0 2
1) "Alice"
2) "Bob"
3) "Don"
```

## Mac 系统中安装 Redis，在 Ruby 中调用 Redis

在 Redis 与所选择的编程语言之间，需要通过 Redis 客户端来通话。
可以在官方提供的列表中查询，推荐使用打星号的。

[Redis Clients](https://redis.io/clients)

从中可以看到　Ruby 推荐使用　[redis-rb](https://github.com/redis/redis-rb)

[redis-rb gem docs](http://www.rubydoc.info/gems/redis)

* 安装 redis-rb

`$ gem install redis`

安装好之后，可以执行下面命令，查看是否成功安装

```
$ gem list redis
*** LOCAL GEMS ***

hiredis (0.6.1)
redis (4.0.1, 3.3.3)
redis-namespace (1.5.3)
redis-objects (1.3.0)
redis-session-store (0.9.1)
```


* 在 Ruby 中调用 Redis

 执行 `irb` 进入 Ruby 控制台

```
$ irb
2.3.1 :001 > require 'redis'
 => true
2.3.1 :002 > redis = Redis.new
 => #<Redis client v4.0.1 for redis://127.0.0.1:6379/0>
2.3.1 :003 > redis.set 'key', 'value'
 => "OK"
2.3.1 :004 > redis.get 'key'
 => "value"
```

## 在 Rails 中使用 Redis

* 添加 ` gem 'redis-rails'`

在 Gemfile 中 `group :development, :test do` 上面添加 ` gem 'redis-rails'`

*Gemfile*

```ruby

+ gem 'redis-rails'

group :development, :test do
```

运行　`bundle install` 后重启 `rails s`

*


## 阿里云 ECS 安装 Redis

1. 安装 Redis

`$ sudo apt-get install redis-server`

查看是否正常运行

`$ sudo service redis-server status`
`$ sudo systemctl status redis`
或
`$ redis-cli ping`
> 会输出 `PONG`


2. 配置 Redis

配置文件：
*/usr/local/etc/redis.conf*

`$ sudo vi /etc/redis/redis.conf`

将一些高危命令彻底关闭，来禁用远程修改 DB 文件地址
```
rename-command FLUSHALL ""
rename-command CONFIG   ""
rename-command EVAL     ""
```

并添加了密码

```
bind 127.0.0.1
# ....
requirepass foobar
```

去掉注释，加上一个强密码。这样就只能通过本机，并需要密码才能访问。

重启 redis

`$ sudo service redis restart`





## Links

* [How to install Redis on OS X](http://richardsumilang.com/server/redis/install-redis-on-os-x/)
* [How to Implement a Simple Redis Leaderboard](https://www.1and1.com/cloud-community/learn/database/redis/how-to-implement-a-simple-redis-leaderboard/)
* [Redis Commands](https://redis.io/commands#)
* [Redis Clients](https://redis.io/clients)
* [redis-rb](https://github.com/redis/redis-rb)
* [redis-rb gem docs](http://www.rubydoc.info/gems/redis)




127.0.0.1:6379> KEYS *
1) "quiz:13"
2) "quiz:53"
3) "quiz:117"
4) "quiz:55"
