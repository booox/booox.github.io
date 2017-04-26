---
layout: post
comments: true
title:  "duplicate column name: email ALTER TABLE "users""
categories:   Rails
tags:  rails-error devise
date:
---

* content
{:toc}

 执行 devise 安装时，报错 `ALTER TABLE "users"` ，先了解一下 devise 的安装步骤及可能的变化，再列出一些常见的报错。




## devise 安装步骤

1. 添加 `devise` 到 Gemfile
  * `gem 'devise'`
  * `bundle install`
  * 重启 `rails s`
2. `rails g devise:install`
3. `rails g devise user`
4. `rake db:migrate`


## 安装导致的变化

当执行完第 2 步 `rails g devise:install` 之后会添加如下两个文件

```
config/initializers/devise.rb
config/locales/devise.en.yml
```

当执行完第 3 步 `rails g devise user` 之后会发生如下变化：

```
$  rails g devise user
  invoke  active_record
  create    db/migrate/20170425221430_devise_create_users.rb
  create    app/models/user.rb
  invoke    test_unit
  create      test/models/user_test.rb
  create      test/fixtures/users.yml
  insert    app/models/user.rb
   route  devise_for :users
```

可见新增了两个与 `active_record` 相关的文件

  `devise_create_users.rb` 和 `app/models/user.rb`

以及与 `test_unit` 相关的文件
并将 `devise_for :users` 插入到 `config/routes.rb`


## 报错

当我们已经执行完第 3 步 `rails g devise user` 之后，又再次执行该命令，会出现如下结果

```
invoke  active_record
  create    db/migrate/20170425223349_add_devise_to_users.rb
  insert    app/models/user.rb
   route  devise_for :users
```

可见会生成 `add_devise_to_users.rb`

![](images/devise-users.png)

比较这两个文件，其实都是在操作 `users` 这个表，且字段都是一样的。

此时若再执行 `rake db:migrate` ，就会报错：

`SQLite3::SQLException: table "users" already exists: CREATE TABLE "users"`

有时，则又会因为有别的操作，导致如下错误提示：

`SQLite3::SQLException: duplicate column name: email: ALTER TABLE "users" ADD "email" varchar DEFAULT '' NOT NULL`

当我们意识到这两个文件有问题而试着将 `add_devise_to_users.rb` 删除再做 `rake db:migrate` 操作，仍然还是会提示：

`SQLite3::SQLException: table "users" already exists: CREATE TABLE "users"`

这是因为在前一次的 `rake db:migrate` 时已经生成了 `users` 表

### 解决方案

执行 「rake 三兄弟」

```
rake db:drop
rake db:create
rake db:migrate
```

就可以了。
