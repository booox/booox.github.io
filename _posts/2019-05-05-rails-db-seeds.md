---
layout: post
comments: true
title:  "Rails 5 使用 `rake db:seed` 添加测试数据 "
categories: rails
tags: rails seeds
date: 2019/05/05 20:04:43
---

* content
{:toc}

Rails 5 使用 `rake db:seed` 添加测试数据


在 Rails 的开发中，经常会使用一些测试数据，可以使用方法有：

* 在浏览器端手工添加
* 在 `rails console` 中手工添加
* 通过访问数据库来手工添加数据


如果只是很少的数据，这样还能接受。但如果数据量比较大，就非常麻烦，还有一个问题就是数据库被清掉之后，又得重新来过，此时比较方便的方法是，使用 `rake db:seed` 来自动添加测试的数据。

总体思路：

* 修改 *db/seeds.rb* 文件，添加测试数据的代码
* 执行 `$ rake db:seed`，完成数据添加

下面通过具体的例子来说明。

## 添加单条数据

**任务要求** 添加一个新的 Group

```
# db/seeds.rb
Group.create(title: 'abc', description: 'abc123')
```

如果需要登录后才能创建 Group，即与用户关联起来，可以这样：

```
# db/seeds.rb
Group.create(title: 'abc', description: 'abc123', user_id: 1)
```

执行 `$ rake db:seed`


## 添加多条数据

**任务要求** 添加 3 个 Group

即传入一个列表，每条数据为一个字典。

```
# db/seeds.rb
Group.create([{ title: 'b1', description: 'b11', user_id: 1 },
              { title: 'b2', description: 'b22', user_id: 1 },
              { title: 'b3', description: 'b33', user_id: 1 }])
```

执行 `$ rake db:seed`

**任务要求** 添加 50 个 Group

使用循环来实现。

```
# db/seeds.rb
50.times do |number|
  Group.create(title: "title-#{number}", description: "des-#{number}", user_id: 1)
end
```

执行 `$ rake db:seed`


## 从 csv 文件中添加数据

**任务要求** 通过 CSV 文件添加 Group

读取 csv 文件，使用循环添加数据。

```
# db/seeds.rb
require "csv"

CSV.foreach('db/groups.csv') do |group|
  Group.create(title: group[0], description: group[1], user_id: 1)
end
```

执行 `$ rake db:seed`

## 使用 Faker 添加测试数据

Faker 是一个产生测试数据的 Gem.
需要修改 Gemfile 如下：

```
group :development do
  # ...
  gem 'faker'
end
```

执行：

`$ bundle install`

再次编辑 *db/seeds.rb* 文件如下：

```
# db/seeds.rb
require 'faker'

50.times do |group|
  Group.create(title: Faker::Company.name, description: Faker::Company.catch_phrase, user_id: 1)
end
```

执行 `$ rake db:seed`

关于 Faker 的用法，可以查看下面的帮助文档。

## Links

* [stympy - faker](https://github.com/stympy/faker)
