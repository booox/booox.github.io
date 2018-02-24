---
layout: post
comments: true
title:  "Python3 读取中文输出乱码解决"
categories: python
tags: python
date: 2018/02/24 18:04:51
---

* content
{:toc}

Python3 读取中文输出乱码解决




## 解决方法

读取时设置 `encoding`

```python
with open('test.txt', encoding='utf-8') as f:
  lines = f.readlines()
  for line in lines:
    print(line)
```
