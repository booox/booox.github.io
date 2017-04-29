---
layout: post
comments: true
title:  "PendingMigrationError ... RAILS_ENV=development"
categories: Rails
tags: rails-error
author: booox
date: 2017/04/22 20:04:52
---

* content
{:toc}

`PendingMigrationError ... RAILS_ENV=development`



有时会出现这样的报错
![]({{ site.url }}/images/PendingMigrationError.png)




### 问题原因

出现这个问题的原因，通常是由于创建了一个 migration ，准备要操作数据库，但是却没有执行 `rake db:migrate` 。

### 解决方法

* 尝试一：
`rake db:migrate`

* 尝试二：
`rake db:migrate RAILS_ENV=development`

* 尝试三：
若还不行，执行 `rake` 三兄弟

```ruby
rake db:drop
rake db:create
rake db:migrate
```
