---
layout: post
comments: true
title:  "如何设置 Rails 站点的默认时区"
categories: Rails
tags:  Rails
date: 2017/11/23 09:37:11
---

* content
{:toc}

如何设置 Rails 站点的默认时区




## 添加 time_zone 到用户 profile

`$ rails g migration add_timezone_to_user_profile`

```rb
class AddTimeZoneToUserProfile < ActiveRecord::Migration[5.0]
  def change
    add_column :profiles, :time_zone, :string
  end
end
```

执行 `$ rake db:migrate`

而后在 `app/views/users/edit.html.erb` 中添加

`<%= f.input :time_zone, label: "时区" %>`

![]({{site.url}}/images/time-zone-beijing.png)

注意：
在系统中时区设置，一开始设为： `(GMT+08:00) Beijing` ，实际应该是 **value** 值 `Beijing` .

## 系统时区

```rb
# app/controllers/application_controller.rb

before_action :set_timezone

def set_timezone
  if current_user && current_user.profile.time_zone
    Time.zone = current_user.profile.time_zone
  else
    Time.zone = 'Beijing'
  end
end
```

## Links

* [How do I configure Rails in production to use config.time_zone instead of the server time zone?](https://stackoverflow.com/a/14680946/4455426)
