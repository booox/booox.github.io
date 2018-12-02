---
layout: post
comments: true
title:  "Rails 中 Devise 用用户名登陆而非邮箱地址"
categories:  Rails
tags:  rails devise
date: 2017/06/27 19:24:16
---

* content
{:toc}

如何让 Devise 允许用户用用户名而不是邮箱登陆？



## 操作步骤：

### 1. 在 users 表中添加 username 字段

* 创建 migration

`rails generate migration add_username_to_users username:string`

* 运行 migration
`rake db:migrate`

*config/initializers/devise.rb*
在文档中找到  `config.authentication_keys = [ :email]`
取消注释，并修改为： `config.authentication_keys = [:username]`



###

当用户还没设置用户名时
上面的用法会有一个问题，就是当用户没有设置用户名时，导航栏上就会是空的（除非设置用户名是必填栏位）。所以还是要判断一下。

在 app/models/user.rb 里增加一个方法

```
def display_name
  if self.username.present?
    self.username
  else
    self.email.split("@").first
  end
end
```

当 username 存在的时候，显示 username，当 username 不存在的时候，用 email 的前缀作为用户名显示。

View 里的代码再改一下

`Hi!,<%= current_user.display_name %>`


## Links

* [How To: Allow users to sign in using their username or email address](https://github.com/plataformatec/devise/wiki/How-To:-Allow-users-to-sign-in-using-their-username-or-email-address)
