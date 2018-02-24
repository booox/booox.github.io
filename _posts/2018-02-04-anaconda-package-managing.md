---
layout: post
comments: true
title:  "Anaconda 包管理"
categories: software
tags: software anaconda
date: 2018/02/04 07:51:26
---

* content
{:toc}

Anaconda 包管理



## 包管理

1. 查看安装包

在 Terminal 中输入以下命令。

`conda list` : 列出在当前环境中已经安装的包
`conda list | grep beautiful` : 快速查看是否已安装 `beautifulsoup`
`conda search beautiful` : 查看是否已安装 `beautifulsoup` (较慢)

2. 添加安装包

在 Terminal 中输入以下命令。

`conda install --name env_name beautifulsoup4` : 在 `env_name` 环境中安装 `beautifulsoup4`
`conda install beautifulsoup4` : 在当前环境中安装 `beautifulsoup4`

> 在 [Anaconda package lists](https://docs.anaconda.com/anaconda/packages/pkg-docs) 根据系统与 Python 的版本列出了支持的安装包

激活环境
  * Linux, macOS: `source activate env_name`
  * Windows: `activate env_name`

注销环境
  * Linux, macOS: `source deactivate`
  * Windows: `deactivate`

查看已安装包

`conda list | grep beautiful`

3. 删除操作

删除安装包: `conda remove --name env_name beautifulsoup4`
验证已删除：`conda list |grep beautifulsoup4`
删除环境: `conda remove --name env_name all`
验证已删除: `conda info --envs`
删除 Conda:
  * Linux, macOS: `rm -rf ~/anaconda`
  * Windows: 通过控制面板 `添加删除程序` 来删除

## Links

* [Managing packages](https://conda.io/docs/user-guide/getting-started.html)
