---
layout: post
comments: true
title:  "form-for 中 f.object 是什么意思？"
categories: Rails
tags:  rails form-for
date: 2017/10/12 23:53:04
---

* content
{:toc}

form-for 中 f.object 是什么意思？



## 用法举例

```
<%= form_for(@section) do |f| %>

<%= error_messages_for(f.object) %>

  <%= f.text_field(:name) %></td>

  <%= f.select(:position, 1..@subject_count) %>

<% end %>
```



## 讲解

在　`form-for` 中　`f`　可以认为是传给表单的一条记录的对象或实例。
而　`f.object` 则指代作为传给　`form-for` 函数的一个参数对象

在上面的例子中, `f.object` 则返回 `@section`




## Links

* [What does f.object do in the Rails form builder?](https://codedump.io/share/P1bpbM6LLufQ/1/what-does-fobject-do-in-the-rails-form-builder)
