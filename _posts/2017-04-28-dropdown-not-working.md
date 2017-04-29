---
layout: post
comments: true
title:  "下拉菜单不起作用"
categories: Rails
tags:  rails-error
author: booox
date: 2017/04/28 22:18:52
---

* content
{:toc}

对产品进行排序的下拉菜单不起作用了，什么原因？




今天遇到一个问题，虽然很小，但事后想想还是有点意思，于是记录一下。

## 问题描述

单击排序按钮没有反应

![]({{ site.url }}/images/button-canot-dropdown.png)

## 思路

1. 首先想到可能是 `app/assets/javascripts/application.js` 中的 `//= require bootstrap/dropdown` 没有加上，或者是其它 js 的加载问题。总之，可能是 js 出了问题。

2. 在 `application.js` 中没有发现什么问题之后，去试了一下用户资料处的下拉菜单， 却发现是正常的。这样就将 js 可能出问题排除在外了。

3. 那到这里，就应该去找 view 的原因了。如果有已知是正确的代码，比如有教程或示例，可以先复制正确的代码去替换，先做排除。

4. 最后，检查发现是由于一个 `h3` 的标签将下拉菜单包在里面导致的，修改之后，问题解决。
