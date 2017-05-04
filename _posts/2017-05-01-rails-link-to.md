---
layout: post
comments: true
title:  "Rails link_to 的用法"
categories: Rails
tags:  rails
author: booox
date: 2017/05/01 20:29:41
---

* content
{:toc}

常见 link_to 的用法




## 最常见的用法

### 访问 `/jobs`

例如，想访问 `jobs#index` 即 `jobs` **Controller** 中的 `index` 这个 **Action**

`<%= link_to "Jobs", jobs_path %>`

> `<a href="/jobs">Jobs</a>`

当然也可以加上括号


`<%= link_to("Jobs", jobs_path) %>`

> `<a href="/jobs">Jobs</a>`

### 访问 `/jobs/:id`

例如，想访问 `jobs#show` 即 `jobs` **Controller** 中的 `show` 这个 **Action**

`<%= link_to job.title, job_path(job) %>`

>  `<a href="/jobs/8">标题</a>`

## 添加样式 CSS 或 ID

有时需要给链接加上样式，那么可以利用 `class` 或 `id`

`<%= link_to("Add a job", new_job_path, class: "btn btn-default" id: "add_job") %>`

> `<a class="btn btn-default" id="add_job" href="/jobs/new">Add a job</a>`


## 使用 block

所谓 `block` ，其实就是一组代码，举个例子

```ruby
def test
  ...
end
```

这里就是一个 block ，由 `def test` 开始，由 `end` 结束，中间可能包括多行代码。

而 `link_to` 也可以支持 block 的写法，特别是当需要在将多个元素一起作为超链接的显示内容时。

```
<%= link_to edit_admin_job_path(job) do %>
  <i class="fa fa-pencil-square fa-fw" aria-hidden="true"></i>
<% end %>
```

> `<a href="/admin/jobs/5/edit"><i class="fa fa-pencil-square fa-fw" aria-hidden="true"></i></a>`


*在上面例子中只是将一个 font-awsome icon 当作链接显示的内容，其实可以有很多条。*


## 修改 HTTP 类型

默认情况下 `link_to` 中 HTTP 的方法为 `GET`，这是可以修改的。

### delete

`<%= link_to("Delete", admin_job_path(job), method: :delete, data: { confirm: "Are you sure?"}) %>`

> `<a data-confirm="Are you sure?" rel="nofollow" data-method="delete" href="/admin/jobs/5">Delete</a>`


或

`<%= link_to("登出", destroy_user_session_path, method: :delete) %>`

> `<a rel="nofollow" data-method="delete" href="/users/sign_out">登出</a>`

### post


`<%= link_to("Hide", hide_admin_job_path(job), method: :post, class: "btn btn-xs btn-default") %>`

> `<a class="btn btn-xs btn-default" rel="nofollow" data-method="post" href="/admin/jobs/5/hide">Hide</a>`


## 链接到图片

### 使用 `image_tag`

`<%= link_to image_tag(tile.image.url), tile %>`

也可以使用 `link_to` 的 block 来定义

```
<%= link_to title %>
  <%= image_tag(title.image.url) %>
<% end %>
```

### 使用 `image_path`

`<%= link_to 'Back to Image', image_path(@image) %>`


### 给图片添加 `alt` 属性

`<%= link_to image_tag(tile.image.url), tile, alt: "title" %>`

## 其它

### 链接锚点

`<%= link_to "Home", root_path(anchor: "home") %>`

> `<a href="/#home">Home</a>`


### 在新窗口打开

`<%= link_to "Bing", "http://cn.bing.com", target: "_blank" %>`

> `<a target="_blank" href="http://cn.bing.com">Bing</a>`


### 支持 javascript

添加 `remote: true` 来让链接来处理 javascript。

`<%= link_to "Javascript", root_path, :remote => true %>`

> `<a data-remote="true" href="/">Javascript</a>`

## link_to_if

这个相当于 `link_to` 再加上一个条件判断
它的语法是：

`link_to_if(condition, name, options = {}, html_options = {}, &block)`

它会先判断条件，如果条件成立，则依照后面给定的 `name` , `options` 等创建超链接，否则只会返回 `name` 。

```
<%= link_to_if(@current_user.nil?, "Login", new_user_session_path) %>
# If the user isn't logged in...
# => <a href="/sessions/new/">Login</a>

<%=
   link_to_if(@current_user.nil?, "Login", new_user_session_path) do
     link_to(@current_user.login, “Logout”, destroy_user_session_path)
   end
%>
# If the user isn't logged in...
# => <a href="/sessions/new/">Login</a>
# If they are logged in...
# => <a href="/accounts/show/3">my_username</a>
```


## Links

* [How to use link_to in Rails](https://mixandgo.com/blog/how-to-use-link_to-in-rails)
* [ActionView::Helpers::UrlHelper](http://api.rubyonrails.org/classes/ActionView/Helpers/UrlHelper.html)
