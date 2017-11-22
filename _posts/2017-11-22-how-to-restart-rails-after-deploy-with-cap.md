---
layout: post
comments: true
title:  "在用 cap 部署之后如何重启"
categories: Rails
tags:  Rails deploy
date: 2017/11/22 20:29:28
---

* content
{:toc}

在用 cap 部署之后如何重启






## 问题

将 Rails 程序部署到阿里云上之后，发现重启是个问题，每次都是将 ESC 重启才可以。

在 terminal 中查看 `$ cap -h` 及 `$ cap production -h` 均无所获。

搜索一下，「`cap production restart`」 马上得到答案。

要检讨一下，这个问题最初被遇到是在三个星期之前了，可是竟然一直没有解决。

## 解决

就一条命令：

`$ cap production deploy:restart`



## Links

* [How to restart puma after deploy?](https://stackoverflow.com/a/38263426/4455426)
