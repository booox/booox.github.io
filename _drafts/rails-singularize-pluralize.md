---
layout: post
comments: true
title:  "Alfred 101"
categories: Software
tags: alfred
date:
---

* content
{:toc}

简洁的 Alfred 入门指南



## 安装

Alfred 是免费的，Alfred Powerpack 和 Alfred Remote for iOS 是需要另外收费的。
安装 [Alfred 3 for Mac](https://www.alfredapp.com/)


## 基础用法

1. 启动：按 「Alt + Space」
2. 打开应用： 输入应用的全名或部分名称，回车
3. 打开文件或图片： 输入 `open 部分文件名` （注意 `open` 后面有空格)
  * 更方便的方法是：先输入空格自动进入 `open file` 模式下
4. 在 Alfred 提供的默认搜索引擎清单中：
  * 如：`imdb sherlock`
  * 完整清单
  * ![]({{site.url}}/images/alfred-web-search.png)
5. 创建自定义的搜索引擎
  * 在浏览器中搜索一次，替换关键词为 `{query}` ，修改得到的搜索页的链接为： `http://stackoverflow.com/search?q={query}`
  * ![]({{site.url}}/images/alfred-custom-search.png)


## Powerpack

`Powerpack` : 需要另外购买，当然非常超值。激活后才可以使用更多的其它功能。
`Workflow` : 就相当于宏。通过快捷键或关键词， Alfred 可以运行各种不同的脚本。
`Packal` : 是 Alfred 的 Workflows 和 Themes 的动态仓库。

既然是 Mac 下的神软，当然要购买支持了。买了 Mega 版的。

> 下面的功能均需要激活 Powerpack 后才可以有。

### Clipboard History

默认情况下，Alfred 是关闭剪贴板的历史记录的。
开启： [Alfred Preferences - Features - Clipboard]

将快捷键修改为「双击 Ctrol」，单击 Viewer Hotkey ，按两次 Ctrl 键即完成设置。

* ![]({{ site.url }}/images/alfred-clipboard-history-1.png)


需要使用时，直接按两次 Ctrl 键
* ![]({{ site.url }}/images/alfred-clipboard-history-2.png)
