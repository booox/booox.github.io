---
layout: post
comments: true
title:  "用 Charles 抓包（http 与 https)"
categories: network
tags:  network charles
date: 2017/12/11 14:58:17
---

* content
{:toc}

用 Charles 抓包（http 与 https)



文章转自：[charles工具抓包教程(http跟https)](http://blog.csdn.net/lowprofile_coding/article/details/47038675)

## 下载 Charles

[官网下载](http://www.charlesproxy.com/download)

下载好之后，在电脑上安装好。

## 查看代理设置（在电脑上操作）

**NOTE:** 手机要跟电脑要在同一个网络

1. 查看默认端口

在电脑上打开 Charles

`Proxy -> Proxy Settings`

默认端口通常为 `8888` 记下后面要用到。


2. 查看当前电脑 IP

Windows: `> ipconfig/all`
MAC OS / LINUX: `> ifconfig`

假设这里为： `192.168.9.129`


## 设置代理（在手机上设置）

在手机上打开

`设置 - 无线网络 - 选中与电脑同一个无线路由器的网络 - 高级设置 - 手动 HTTP 代理`

* `代理服务器主机名` : `192.168.9.129`
* `代理服务器端口` : `8888`

此时若无错误，手机上访问网页，Charles 中就应该可以看到抓到的数据包了。

## 如何抓取 HTTPS 协议包

1. 先保证 charles 可以正常抓取 http 协议包，即上述步骤没有错误

2. 电脑上设置 Charles 的 SSL 代理

  * `Proxy->SSL Proxy Settings`
  * 选中 `Enable SSL Proxying`
  * 将要抓取的https 域名添加进去。如：`api.douban.com:443`
    * 端口默认为 `443`

  * 依次点击 `Help->SSL Proxying ->Install Charles Root Certificate on a Mobile Device or Remote Browser...`

  * 记下弹出对话框中的地址：
    * 低版本为：`http://charlesproxy.com/getssl`
    * 高版本为：`http://chls.pro/ssl`


3. 手机上下载安装 SSL 证书

 * 手机用浏览器访问上述地址(如：http://chls.pro/ssl)
 * 按提示安装证书

至此就可以抓 HTTPS 协议包了。


## Links

* [charles工具抓包教程(http跟https)](http://blog.csdn.net/lowprofile_coding/article/details/47038675)
