---
layout: post
comments: true
title:  "Gems: Datepicker and Datetimepicker"
categories: Rails
tags: gems
date: 2017/05/05 20:36:20
---

* content
{:toc}

Datepicker and Datetimepicker



## Datepicker

* [Gem: bootstrap-datepicker-rails](https://github.com/Nerian/bootstrap-datepicker-rails)

### Install Datepicker

* *Gemfile*

`gem 'bootstrap-datepicker-rails'`

`bundle install` ，重启 `rails s`

* *app/assets/stylesheets/application.scss*

`+ @import "bootstrap-datepicker3";`

* *app/assets/javascripts/application.js*

```
+ //= require bootstrap-datepicker/core
+ //= require bootstrap-datepicker/locales/bootstrap-datepicker.zh-CN
```

* *app/views/users/edit.html.erb*
在 view 中通常用日期时，用的是 `date_field` ，而现在要改成 `text_field`

```
+ <%= ff.text_field :birthday, :class => "form-control" %>

+ <script>
  + $("#user_profile_attributes_birthday").datepicker({ format: "yyyy-mm-dd" });
+ </script>
```

注意：格式要按 Rails 的格式 `"yyyy-mm-dd"`

* 如果要支持多国语言

```
<script>
  $("#user_profile_attributes_birthday").datepicker({ format: "yyyy-mm-dd", language: "<%= I18n.locale %>" });
</script>

```

## 让 Simple form 支持 Datepicker

因为实际的项目中使用了 `simple_form`，所以也就让它支持 Datepicker

有这样一种用法： `<%= f.input :created_at, :as=> 'datepicker' ,:label => 'Date of send' %>`

但实际却会报错：`No input found for datepicker`

这是因为，对于 `simple_form` 这个 Gem，它并不知道 input 有个类型叫 `:datepicker`

而我们要做的是，将上述用法修改为：

`<%= f.input :return_date, as: :string, label: "选择线路返回日期", class: "form-control" %>`


## Datetimepicker

* *Gemfile*

```
gem 'momentjs-rails', '>= 2.9.0'
gem 'bootstrap3-datetimepicker-rails', '~> 4.17.47'
```

`bundle install` ，重启 `rails s`

* *app/assets/stylesheets/application.scss*

`+ @import "bootstrap-datetimepicker";`

* *app/assets/javascripts/application.js*

```
+ //= require moment
+ //= require moment/zh-cn
+ //= require bootstrap-datetimepicker
```

* *app/views/users/edit.html.erb*
在 view 中通常用日期时，用的是 `date_field` ，而现在要改成 `text_field`

```
<div class="form-group" style="position: relative;">
  <%= ff.label :birthday %>
  <%= ff.text_field :birthday, class: "form-control" %>
</div>

+ <script>
  + $("#user_profile_attributes_birthday").datetimepicker({});
+ </script>
```

注意：要将 `datetimepicker` 控件放到一个 `position: relative;` 的元素当中
否则，日期时间选择界面会跑到左下角，而不会出现在正确的位置。

* 如果要支持多国语言

先在 `application.js` 中添加 `moment` 的 locale 文件，如:

`//= require moment/zh-cn`

更多文件可以在这里找到：[Moment Locale Files](https://github.com/moment/moment/tree/develop/locale)

而后在使用 `datetimepicker` 页面最底下添加如下代码：

```
<script>
  $("#user_profile_attributes_birthday").datetimepicker({ locale: 'zh-cn'});
</script>

```


## Error

1. `Uncaught Error: datetimepicker component should be placed within a non-static positioned`

这是因为 `datetimepicker` 控件要求上层元素的 `position` 属性要为 `relative` 。
只要在上层元素添加样式即可： `<div class="form-group" style="position: relative;">`


## Links

* [bootstrap-datetimepicker Options](https://eonasdan.github.io/bootstrap-datetimepicker/Options/#options)
* [Moment Locale Files](https://github.com/moment/moment/tree/develop/locale)
* [Using Locales](http://eonasdan.github.io/bootstrap-datetimepicker/#using-locales)
* [Gem: bootstrap3-datetimepicker-rails](https://github.com/TrevorS/bootstrap3-datetimepicker-rails)
* [Trouble getting datepicker in simple_form](https://stackoverflow.com/questions/22256407/trouble-getting-datepicker-in-simple-form)
