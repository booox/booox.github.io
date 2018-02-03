---
layout: post
comments: true
title:  "微信小程序常见错误"
categories: weapp
tags:  miniprogram wechat weapp
date: 2017/11/26 07:45:30
---

* content
{:toc}

微信小程序常见错误




## `不在以下合法域名列表中`

[wx.request(OBJECT)](https://mp.weixin.qq.com/debug/wxadoc/dev/api/network-request.html)

在官方文档 [关于小程序中网络相关API的说明](https://mp.weixin.qq.com/debug/wxadoc/dev/api/api-network.html) 有说明：

* 服务器域名配置

> 每个微信小程序需要事先设置一个通讯域名，小程序可以跟指定的域名与进行网络通信。包括普通 HTTPS 请求（`request`）、上传文件（`uploadFile`）、下载文件（`downloadFile`) 和 WebSocket 通信（`connectSocket`）

> 小程序必须使用 HTTPS 请求。小程序内会对服务器域名使用的 HTTPS 证书进行校验，如果检验失败，则请求不能成功发起。由于系统限制，不同平台对于证书要求的严格程度不同。为了保证小程序的兼容性，建议开发者按照最高标准进行证书配置，并使用相关工具检查现有证书是否符合要求。

* 配置流程

> 服务器域名请在 *小程序后台-设置-开发设置-服务器域名* 中进行配置，配置时需要注意：

* 跳过域名校验

> 在微信开发者工具中，可以临时开启 *开发环境不校验请求域名、TLS版本及HTTPS证书* 选项，跳过服务器域名的校验。此时，在微信开发者工具中及手机开启调试模式时，不会进行服务器域名的校验。

> 在服务器域名配置成功后，建议开发者关闭此选项进行开发，并在各平台下进行测试，以确认服务器域名配置正确。


### 解决办法：

* 在 *小程序后台-设置-开发设置-服务器域名* 中设置 `request 合法域名`
* 在开发者工具中临时 *跳过域名校验*





## 出现 `400 (Bad Request)`

小程序发起网络请求使用的是 `wx.request`，可用参数包括

* `url`: 必填。请求的接口地址
* `data`: 请求的参数
* `header`: 设置请求的 header，header 中不能设置 Referer。
* `method`:  默认 GET（需大写）有效值：OPTIONS, GET, HEAD, POST, PUT, DELETE, TRACE, CONNECT
* `dataType`:  默认 json 如果设为json，会尝试对返回的数据做一次 JSON.parse

详细查看 [wx.request(OBJECT)](https://mp.weixin.qq.com/debug/wxadoc/dev/api/network-request.html)

官方示例代码为：

```js
wx.request({
  url: 'test.php', //仅为示例，并非真实的接口地址
  data: {
     x: '' ,
     y: ''
  },
  header: {
      'content-type': 'application/json' // 默认值
  },
  success: function(res) {
    console.log(res.data)
  }
})
```

### 解决办法：

修改 `header` 中的 `content-type` 值为 `json`, 即:

```js
header: {
    'content-type': 'json' //
},

// 或 （但不建议）
header: {
    'content-type': 'application/text' //
},

```





## Links

* [微信小程序request出现400的坑](http://blog.csdn.net/u013278099/article/details/54967380)
