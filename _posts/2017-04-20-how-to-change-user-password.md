---
layout: post
title:  "Rails 管理员密码忘记了怎么办？"
categories: Rails
tags: rails
author: booox
---

* content
{:toc}


新手问题，注册了用户开通了管理员权限，却忘记了管理员的密码怎么办？



假定管理员邮箱为 `name@gmail.com`
以下操作均在控制台进行，进入控制台的方法，终端里在项目文件夹下执行：

`rails c`

## 修改管理员密码

### 方法一：

```ruby
u = User.where(email: 'name@gmail.com').first
u.password = 'new_password'
u.password_confirmation = 'new_password'
u.save!
```

### 方法二：

```ruby
User.find_by(email: 'name@gmail.com').reset_password('new_password','new_password')
```
