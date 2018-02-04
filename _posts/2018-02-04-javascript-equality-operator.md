---
layout: post
comments: true
title:  "Javascript 中(==)与(===)区别"
categories:  Javascript
tags: javascript
date: 2018/02/04 11:15:20
---

* content
{:toc}

Javascript 中(==)与(===)区别



## Javascript 中(==)与(===)区别

通过代码来说明是更容易的

```javascript
if(3 == "3"){
    console.log("They are the same");
} else {
    console.log("They are not the same.");
}
```

OUTPUT：

> They are the same

第 2 个示例：

```javascript
if(3 === "3"){
    console.log("They are the same");
} else {
    console.log("They are not the same.");
}
```
OUTPUT：

> They are not the same.

## 总结

`==` 在比较前会进行类型的转换，而 `===` 则会对类型与值都进行比较。
