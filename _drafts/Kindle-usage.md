---
layout: post
comments: true
title:  "Kindle 用法介绍"
categories: Tools
tags:  tools kindle
date: 2017/07/21 06:36:51
---

* content
{:toc}

记录与 Kindle 的一些用法。


## 简单用法

### 设置邮箱推送 {#email-push}

即，设置好之后我们可以直接通过发邮件将电子书推送到 Kindle 上。
主要是两个部分：

* `〖发送至Kindle〗电子邮箱`
  * 你的电子书是通过附件发送到这个邮箱，它再将你自动发送到 Kindle
* `已认可的发件人电子邮箱列表`
  * 只有在这个列表中的邮箱，才被认可可以发邮件到上面指定的 `〖发送至Kindle〗电子邮箱`

具体设置在 amazon.cn (或amazon.com ，是两套不同的账号) 账户的 `管理我的内容和设备` 中设置。



###



## Calibre 相关

### Calibre 的向导设置 {#calibre-wizard}

可以在首次打开时设置，也可以在 Calibre 打开时，运行 `Preferences > Run welcome wizard`
在其中可以设置:
* 语言
* 设备类型（如： Amazon - Kindle PaperWhite)
* 自动用邮箱推送到 Kindle

![]({{site.url}}/images/calibre-push-to-kindle.png)

### 定时抓取新闻任务 {#schedule-download}

`Fetch news - Schedule new download`

![]({{site.url}}/images/calibre-schedule-fetch-news.png)

### 定时抓取网上内容并发送到 Kindle

1. [设置邮箱推送](#email-push)
2. [Calibre 的向导设置](#calibre-wizard)
3. [设置定时抓取任务](##schedule-download)


## Links

* [Dig into the rails errors
Errors](http://neethack.com/2015/04/dig-into-the-rails-errors/)
* [How to use ActiveModel errors details](https://cowbell-labs.com/2015-01-22-active-model-errors-details.html)
* [ActiveModel::Errors < Object](http://api.rubyonrails.org/classes/ActiveModel/Errors.html#method-i-empty-3F)
