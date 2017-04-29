---
layout: post
comments: true
title:  "关于 Git workflow 的一个很好的解释"
categories:    Git
tags:  git
date: 2017/04/27 06:56:31
---

* content
{:toc}

 这是关于 Git workflow 的一个清晰的解释。




早上翻查 `git grep` 用法时，看到一个视频 [GIT Workflow - Georgia Tech - Software Development Process](https://www.youtube.com/watch?v=3a2x1iJFJWc) 。这是到目前为止我看到关于 Git 的最好的解释。


### 四棵树

1. `Workspace (Working dir)` ：本地文件夹
2. `Index (stage)` : 通过 `add` 将文件打上标记，加入 stage ，但还没有 `commit` 也即还没有放入本地仓库中
3. `HEAD (local repository)` : `commit` 之后就进入 HEAD ，这样代码的状态就被安全的存储起来
4. `remote repository` : 远程仓库，如 **Github.com**

![]({{site.url}}/images/git-workflow-four-trees.png)

### git clone

从远程仓库到本地 Working dir

`git clone `

![]({{site.url}}/images/git-workflow-clone.png)

### git work flow

这个就更厉害了，清晰完整地显示了常用的 git 的工作流。

![]({{site.url}}/images/git-workflow-all.png)
