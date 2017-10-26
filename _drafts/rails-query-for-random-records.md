---
layout: post
comments: true
title:  "Rails 获取随机记录"
categories:  Rails
tags:  rails
date: 2017/10/20 19:39:30
---

* content
{:toc}

Rails 获取随机记录


## 使用 `Order("RANDOM()")`

`Group.where(id: GroupUser.select(:group_id)).select(:uid).order("RANDOM()").limit(10)`


## 使用 `Sample`

```ruby
[1,2,3,4,5,6].sample     # => 4     
[1,2,3,4,5,6].sample(3)  # => [2, 4, 5]     
[1,2,3,4,5,6].sample(-3) # => ArgumentError: negative array size     
[].sample     # => nil     
[].sample(3)  # => []   

@user = User.all.sample(1)

User.where(active: true).sample(5)  # => return randomly 5 active user's from User table
```



## Links

* [Ruby on Rails + Active Record: Order, distinct and random in one query](https://stackoverflow.com/a/45762698/4455426)
