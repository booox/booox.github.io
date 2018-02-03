---
layout: post
comments: true
title:  "Conda Commond not found"
categories: software
tags: anaconda conda
date: 2018/02/04 07:33:05
---

* content
{:toc}

 装好了 Anaconda，却提示 `zsh: command not found: conda`



## command not found: conda

1. 环境

MacOS
Anaconda

2. 问题

Anaconda 安装好了，Anaconda Navigator 、Spyder、Jupyternotebook 都没有问题，但却无法通过 `conda` 来管理 Python 包。

在 iTerm2 窗口中输入 `conda --version`
却提示： `zsh: command not found: conda`

## 解决办法

1. 编辑 `~/.bash_profile`

在文档末尾添加：

`export PATH="/Users/your-user-name/anaconda3/bin:$PATH"`

注意：将 `your-user-name` 替换成你的用户名

2. 执行 `source`

```
source ~/.bash_profile
echo $PATH
conda --version
```
