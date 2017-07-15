---
layout: post
comments: true
title:  "Rails 页面适应手机端及顶部　Icon Bar"
categories:  Rails
tags:  rails bootstrap
date: 2017/06/27 13:33:39
---

* content
{:toc}

站点做好了，PC 上查看还可以，但手机端却不忍直视……



## 手机端自适应

因为原来使用的是　Bootstrap 的　CSS 框架，所以只要在　`head` 部分加入下面一句就可以。

*app/views/layouts/application.html.erb*
```html
<head>
  <title>...</title>
  <%= csrf_meta_tags %>

  <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=0">
</head>
```

有了这一句，就可以让手机端看起来八九不离十了。
而 `viewport` 这个　meta 的设置则意味着让浏览器会（可能）根据屏幕的宽度来渲染页面的宽度。

几个属性简要介绍如下：

* `width` : 根据设备生成的虚拟的宽度（？）
* `device-width` : 设备的物理宽度
* `iniital-scale` : 设置访问页面时的初始缩放比例。1.0　表示不缩放。
* `maximum-scale` : 设置访问页面时的最大可能缩放比例。1.0　表示不缩放。
* `user-scalable` : 设置允许设备能放大或缩小页面，值为　yes 或　no ，0　表示不允许。

详细的可以参考：[Responsive Meta Tag](https://css-tricks.com/snippets/html/responsive-meta-tag/)

## 顶部的　Icon-bar

当在手机上访问页面时，顶部导航会因为宽度不够，而会影响美观。

可以通过设置让顶部的导航菜单收起来，变成一个三条横线的　Icon-bar.

### 1. 添加　`bootstrap/collapse`

*app/assets/javascripts/application.js*

在最后添加一句，加入 `collapse` 折叠的支持
`//= require bootstrap/collapse`

### 2. 修改　navbar，加入三条横线

*app/views/common/_navbar.html.erb*

```html
<div class="navbar-header">
  <!-- .... -->
    <button class="navbar-toggle" type="button" data-toggle="collapse" data-target=".navbar-ex1-collapse">
      <span class="sr-only">Toggle navigation</span>
      <span class="icon-bar"></span>
      <span class="icon-bar"></span>
      <span class="icon-bar"></span>
    </button>
</div>
```

注意后面的　`data-target=".navbar-ex1-collapse"` ，这后面要用。

### 3. 设置需要折叠起来的菜单

*app/views/common/_navbar.html.erb*

将需要折叠起来的菜单加入　`navbar-ex1-collapse` 这个 CSS 的类

```html
<!-- ...... -->
<div class="collapse navbar-collapse navbar-ex1-collapse" id="bs-example-navbar-collapse-1" aria-expanded="false">
    <ul class="nav navbar-nav">
      <!-- ...... -->
    </ul>
    <ul class="nav navbar-nav navbar-right">
      <!-- ...... -->
    </ul>
</div>
```

## Links

* [Responsive Meta Tag](https://css-tricks.com/snippets/html/responsive-meta-tag/)
*
