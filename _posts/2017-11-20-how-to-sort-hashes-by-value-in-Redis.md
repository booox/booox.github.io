---
layout: post
comments: true
title:  "在 Redis 中如何根据值来对 hash 进行排序"
categories:  Redis
tags:  Redis
date: 2017/11/20 05:48:50
---

* content
{:toc}

在 Redis 中如何根据值来对 hash 进行排序




在 Redis 中 `hash` 可以很方便的存储一个对象的多个值。比如一个用户的姓名、得分、排名等等。而如何对 hash 进行排序的呢？

## 如何排序

思路就是先把 `hash` 中的 key 先压到一个 `SET` 中，而后再用 `SORT` 命令的 `BY` 来排序。

## 实现

* `user:100` 与 `user:200` 均存为 `hash` 类型

```
127.0.0.1:6379> HSET user:100 visits 10
(integer) 1
127.0.0.1:6379> HGETALL user:100
1) "visits"
2) "10"
127.0.0.1:6379> HSET user:100 score 110
(integer) 1
127.0.0.1:6379> HINCRBY user:100 score 10
(integer) 120
127.0.0.1:6379> HGETALL user:100
1) "visits"
2) "10"
3) "score"
4) "120"
127.0.0.1:6379> HSET user:200 score 20
(integer) 1
127.0.0.1:6379> HSET user:200 score 20 name "Bob" visits 11
(integer) 2
127.0.0.1:6379> HGETALL user:200
1) "score"
2) "20"
3) "name"
4) "Bob"
5) "visits"
6) "11"
```

* 将 `user:100` 与 `user:200` 压入名为 `quiz:5` 的 `SET` 中。

```
127.0.0.1:6379> SADD quiz:5 user:200
(integer) 1
127.0.0.1:6379> SADD quiz:5 user:100
(integer) 1

127.0.0.1:6379> SMEMBERS quiz:5
1) "user:200"
2) "user:100"
127.0.0.1:6379> HGETALL user:200
1) "score"
2) "20"
3) "name"
4) "Bob"
5) "visits"
6) "11"
127.0.0.1:6379> HGETALL user:100
1) "visits"
2) "10"
3) "score"
4) "120"


```

* 进行排序， `desc` 为倒序

```
127.0.0.1:6379> SORT quiz:5 by *->score
1) "user:200"
2) "user:100"
127.0.0.1:6379> SORT quiz:5 by *->score desc
1) "user:100"
2) "user:200"
```


## Links

* [How to sort a redis hash by values in keys](https://stackoverflow.com/a/31285884/4455426)
* [Redis Docs: SORT](https://redis.io/commands/SORT)
