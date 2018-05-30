---
layout: post
comments: true
title:  "在 Rails 中使用 Bulma　框架"
categories: rails
tags: rails bulma
date: 2018/05/30 12:07:03
---

* content
{:toc}

通过本文可以了解到如何在 Rails 中使用 Bulma　框架。





## 什么是 Bulma？

[Bulma](https://bulma.io/) 是一个基于 Flexbox 设计的开源 CSS　框架。

容易学习，容易上手，节省开发时间。

## 如何使用 Bulma

使用 [bulma-rails](https://github.com/joshuajansen/bulma-rails) 这个 Gem。

* 编辑 *Gemfile*

在 `group :development, :test do` 上方添加如下代码

> `gem "bulma-rails", "~> 0.7.1"`

执行 `bundle install` 完成安装

* 修改 *app/assets/stylesheets/application.scss*

在最后添加

> `@import "bulma";`

* 修改 *app/assets/javascripts/application.js*

**方法一**

按照官方文档说明，若需要使用 *fontawesome* ，则需要添加下面一行到 ``
`<script defer src="https://use.fontawesome.com/releases/v5.0.7/js/all.js"></script>`

在 `//= require_tree .` 上面添加到 *app/views/layouts/application.html.erb* 的　`<head>`　中间

**方法二**

或者将上述 `all.js` 下载并保存到 *app/assets/javascripts/* 下面

并修改 *app/assets/javascripts/application.js* ，在 `//= require_tree .` 上方添加

> `//= require all`


## 如何自定义 Bulma 颜色等

修改 *app/assets/stylesheets/application.scss*

在 `aa` 前面添加代码，大致变成这样：

```css
/* Use Bulma for styling */

$link: #287FB8;
$link-hover: #344e86;


$yellow:  #ED6504;
$warning: $yellow;

@import "bulma";
```

具体可以参考

[Bulma Customise](https://bulma.io/documentation/overview/customize)
[How to customize bulma variables in rails](https://stackoverflow.com/questions/44347593/how-to-customize-bulma-variables-in-rails)

在文中说到，需要将文件名修改为 *application.css.scss*

但实际使用中，发现不改变名字，也可以，于是名字就没动。




## Links

* [Bulma Github](https://github.com/jgthms/bulma)
* [Learn Bulma CSS for free](https://scrimba.com/p/pV5eHk/c8EqDTa)
