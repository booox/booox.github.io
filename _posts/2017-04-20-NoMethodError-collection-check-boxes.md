---
layout: post
title:  "NoMethodError in collection_check_boxes"
categories: Rails
tags: rails rails-error
author: booox
---

* content
{:toc}

`NoMethodError in collection_check_boxes`



### 错误 `collection_check_boxes`

代码如下：

`<%= f.collection_check_boxes(:group_id, Group.all, :id, :name) %>`

具体错误如下：

`NoMethodError in collection_check_boxes`
`undefined method 'group_id' for`

### 查看语法书

在 Atom 中将光标定位到 `collection_check_boxes` 中，按快捷键 `Ctrl + H` 呼叫出 **Dash** ，得到：

```ruby

# collection_check_boxes(method, collection, value_method, text_method, options = {}, html_options = {}, &block)

<%= form_for @post do |f| %>
  <%= f.collection_check_boxes :author_ids, Author.all, :id, :name_with_initial %>
  <%= f.submit %>
<% end %>

```

很容易就可以看出是 `group_ids` 后面少了 `s`
