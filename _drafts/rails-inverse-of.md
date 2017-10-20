---
layout: post
comments: true
title:  "inverse_of 是什么意思?"
categories: Rails
tags:  rails inverse_of
date: 2017/10/14 16:34:14
---

* content
{:toc}

inverse_of 是什么意思?



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

* [关于 inverse_of 的困惑与探究](https://ruby-china.org/topics/24998)
