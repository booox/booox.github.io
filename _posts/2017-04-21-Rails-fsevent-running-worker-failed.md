---
layout: post
comments: true
title:  "ERROR: fsevent: running worker failed: Resource temporarily unavailable"
categories: Rails
tags: rails-error
author: booox
---

* content
{:toc}

执行 `rails g controller xxx`，报错：
`ERROR -- : fsevent: running worker failed: Resource temporarily unavailable`




### 解决办法

原因： `rails c` 开启
解决：在 rails console 中输入 `exit` 退出，再试，通过。
