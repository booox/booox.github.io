---
layout: post
comments: true
title:  "Rails Code Good Habits"
categories: Rails
tags: rails
date:
---

* content
{:toc}

有些好习惯，越早养成越好



###

* 新建 model 时，随手加上 `index`
  * 每个用户有一个 `profile` ： `t.integer :user_id, :index => true`
