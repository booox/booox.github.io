---
layout: post
comments: true
title:  "微信小程序的数据绑定"
categories: miniprogram
tags:  miniprogram wechat
date: 2017/11/25 20:49:44
---

* content
{:toc}

微信小程序的数据绑定



[框架](https://mp.weixin.qq.com/debug/wxadoc/dev/framework/MINA.html)

小程序的视图 View 与 逻辑层 JavaScript ，与
Rails 视图 View 与 控制 Controller 可以近似看成一一对应。


## View

```
<!-- This is our View -->
<view> Hello {{name}}! </view>
<button bindtap="changeName"> Click me! </button>
```


## JS

```js
// This is our App Service.
// This is our data.
var helloData = {
  name: 'WeChat'
}

// Register a Page.
Page({
  data: helloData,
  changeName: function(e) {
    // sent data change to view
    this.setData({
      name: 'MINA'
    })
  }
})
```

* 在 `page` 中通过 `data` 将数据传到 View ，从而可以在 View 中使用 `{{ name }}` 来调用存储的数据
* 在 View 中通过 `bindtap` 绑定了按钮与单击按钮后的处理函数 `changeName`
  * 当单击按钮后，View 会发送 `changeName` 事件给逻辑层，而逻辑层则找到对应的函数来处理
* 在逻辑层则通过 `setData` 这个方法来实现改变控件的文本。






## Links

* [框架](https://mp.weixin.qq.com/debug/wxadoc/dev/framework/MINA.html)
