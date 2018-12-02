---
layout: post
comments: true
title:  "Rails 5 中使用CarrierWave 在 `remote: true` 实现上传图片不成功"
categories: rails
tags: rails CarrierWave
date: 2018/06/11 05:53:15
---

* content
{:toc}

Rails 5 中使用CarrierWave 在 `remote: true` 实现上传图片不成功



要在一个 modal 中上传图片，使用了 AJAX 的方式（即使用 `remote: true`)，不成功。
所用的 CSS 框架为 Bulma，不包含 Javascript，不提供相关的功能。

报错： `Completed 406 Not Acceptable`
并提示：`ActionController::UnknownFormat (ActionController::UnknownFormat)`



## 原因

搜索了好久，后来发现是 CarrierWave 的问题
在 [form remote=> true returns html instead of js](https://github.com/carrierwaveuploader/carrierwave/issues/412) 页面上有一句话：

> I'm totally stumped on this, I have a regular :remote => :true form with a CarrierWave file upload, and it won't go to the javascript response, just HTML.

大意是：

> 在用 CarrierWave 上传文件时，如果启用 `remote: true`，那么它只会返回 HTML，而不会是 javascript。

所以，即使我在 controller 中写明返回 `format:js`

```rb
if complain.save!
  respond_to do |format|
    format.js
  end
end
```

因为它只能返回 HTML，而我们又要求它返回 JS，于是它就只能：

报错： `Completed 406 Not Acceptable`
并提示：`ActionController::UnknownFormat (ActionController::UnknownFormat)`

而让它返回 HTML 类型时，则正常:

```ruby
if complain.save!
  respond_to do |format|
    format.html { redirect_to(course_section_path(@course, @section)) }
end
```


## 解决办法

安装 [remotipart](https://github.com/JangoSteve/remotipart) Gem

*Gemfile*

```rb
gem 'remotipart', '~> 1.2'
```

执行 `bundle install` 后重启

修改 application.js

*application.js*


```
//= require jquery_ujs
+ //= require jquery.remotipart
```


这样就可以用正常的方法来在 AJAX 中实现文件的上传了。

**View**

```
<%= form_for @xxx, url: xxx_path, remote: true, authenticity_token: true do |f| %>

<% end %>
```

**Controller**

```ruby
if complain.save!
  respond_to do |format|
    format.js
end
```




## Links

* [JangoSteve/remotipart](https://github.com/JangoSteve/remotipart)
* [carrierwaveuploader/carrierwave](https://github.com/carrierwaveuploader/carrierwave/wiki/How-to%3A-Easy-Ajax-file-uploads-with-Remotipart)
* [form remote=> true returns html instead of js](https://github.com/carrierwaveuploader/carrierwave/issues/412)
* [rails 406 not acceptable](https://stackoverflow.com/search?q=rails+406+not+acceptable)
