---
layout: post
comments: true
title:  "form-for 中 radio-button 不能被选中"
categories: Rails
tags: rails form-for
author: booox
---

* content
{:toc}


 设置没有发现什么错误,但 radio-button 就是不能选中

## 问题

radio-button 设置好之后,但不能被选中

![]({{site.url}}/images/radio-button-cant-focus.gif)

## 原因及解决

**bootstrap** 没有被正常加载

修改 *app/assets/javascripts/application.js*
添加

```
//= require bootstrap-sprockets
//= require bootstrap
```

注: 如果只用 `bootstrap` ,则弹出式菜单可能无法响应,不能弹出.
