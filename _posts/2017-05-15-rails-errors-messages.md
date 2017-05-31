---
layout: post
comments: true
title:  "Rails Errors Messages 不显示"
categories:  Rails
tags:  rails
date: 2017/05/15 15:11:29
---

* content
{:toc}

使用了 `validate_presence_of` 来验证，但错误消息并不显示。



## 验证非空

*app/models/registration.rb*

```ruby
validates_presence_of :name, :email, :cellphone
```

本以为这样设定之后，当 `:name` 等没有填入内容时，表单就会报错

但却没有，才想起来，可能需要额外设置。

### 显示错误信息


*new.html.erb*

```
<%= form_for @registration, url: update_step2_event_registration_path(@event, @registration) do |f| %>

  <% if @registration.errors.any? %>
    <ul>
    <% @registration.errors.full_messages.each do |msg| %>
      <li><%= msg %></li>
    <% end %>
    </ul>
  <% end %>

<% end %>
```

这样就可以在填表时提示必填项目不能为空了。
