---
layout: post
comments: true
title:  "Rails no implicit conversion of nil into String"
categories: Rails
tags: rails-error
date:
---

* content
{:toc}

`no implicit conversion of nil into String`





## Error

在上传多图时提示如下错误：

`TypeError in Admin::EventsController#update`

`no implicit conversion of nil into String`

![]({{site.url}}/images/rails-no-implicit-conversion.png)

提示类型错误，应该是多图上传时的类型不对导致。


## 
