---
layout: post
comments: true
title:  "Git: 获取所有的远端分支"
categories:    Git
tags:  git
date: 2017/11/06 09:52:44
---

* content
{:toc}

Git: 获取所有的远端分支

## 未能成功的方案

下面这两种方法经实验均 **未能成功**
`$ git fetch --all`
[https://stackoverflow.com/a/10312587/4455426](https://stackoverflow.com/a/10312587/4455426)

或
`$ git fetch origin`

[https://stackoverflow.com/a/20783081/4455426](https://stackoverflow.com/a/20783081/4455426)

在另一篇 [How to fetch all git branches](https://stackoverflow.com/a/39985786/4455426) 中看到

> If you've trouble fetching them, then precede the above command with:

> `git config remote.origin.fetch '+refs/heads/*:refs/remotes/origin/*'`

以为找到问题所在了，可是一查看 `git config remote.origin.fetch`

发现原来就是要设的值，于是还是没有解决。
但却了解了 **Refspec** 这个概念，详细：[Git Internals - The Refspec](https://git-scm.com/book/en/v2/Git-Internals-The-Refspec)
（ 或直接 `$ vi .git/config` ）


## 目前解决办法

手动一个一个切下来

`$ git checkout -b local-branch-name origin/remote-branch-name`



## Links

* [Git Internals - The Refspec](https://git-scm.com/book/en/v2/Git-Internals-The-Refspec)
* [How to fetch all git branches](https://stackoverflow.com/a/39985786/4455426)
