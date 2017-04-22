---
layout: post
comments: true
title:  "uninitialized constant User"
categories: Rails
tags: rails-error
author: booox
date: 2017/04/22 08:31:00
---

* content
{:toc}

执行 `rails s` ，报错： `uninitialized constant User`


### 解决

这种情况通过是由于对应的 model 没有正确建立甚至没有创建而导致

经过检查，没有发现 `app/model/user.rb`

执行
`rails g devise user`

而后再
`rake db:migrate`

问题得以解决
