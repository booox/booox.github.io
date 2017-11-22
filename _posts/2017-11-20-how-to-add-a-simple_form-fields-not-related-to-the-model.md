---
layout: post
comments: true
title:  "在 Rails 中如何添加一个与 model 没有关联的 `simple_form`"
categories:  Rails
tags:  Rails
date: 2017/11/20 13:56:21
---

* content
{:toc}

在 Rails 中如何添加一个与 model 没有关联的 `simple_form`




## 实现

使用 `simple_fields_for`

```ruby
<%= simple_form_for @user do |f| %>
  <%= f.input :name %>

  <%= simple_fields_for :no_model_fields do |n| %>
    <%= n.input :other_field %>
  <% end %>
<% end %>
```

这里就添加了一个 `other_field` 的字段，而它不并是属于 `user` 这个 model 的。

获取参数就可以使用 `params[:no_model_fields][:other_field]`



## Links

* [rails simple_form fields not related to the model](https://stackoverflow.com/a/33251087/4455426)
