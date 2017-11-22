---
layout: post
comments: true
title:  "如何将 jQuery 绑定到 `radio_button` 上"
categories:  jQuery
tags:  jQuery
date: 2017/11/18 09:38:26
---

* content
{:toc}

如何将 jQuery 绑定到 `radio_button` 上


一开始想实现单击 `radio_button` 实现一个功能，就想当然地认为与其它的 `button` 一样，直接用 `on("click")` ，结果却是错的。

## 正确方法

正确方法应该用 `change`

例如：对 `@user` 这个表单

```js
$(document).on("turbolinks:load", function() {
  $("[id^=something_]").on("change", function(t) {   //  以 `something_` 开头的 id
    console.log($(this).val());
  });
});
```
