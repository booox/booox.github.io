---
layout: post
comments: true
title:  "给 Jupyter 添加书签目录（TOC）"
categories: software
tags: software jupyter
date: 2018/02/10 09:08:03
---

* content
{:toc}

给 Jupyter 添加书签目录（TOC）



## 环境

* macOS
* Anaconda
* Jupyter

## 安装

1. 需要用这个安装侧边栏 管理插件

[jupyter_nbextensions_configurator](https://github.com/Jupyter-contrib/jupyter_nbextensions_configurator)

`conda install -c conda-forge jupyter_nbextensions_configurator`

装好之后，重启 Jupyter

 2. 再安装包罗万象的插件包

[jupyter_contrib_nbextensions](https://github.com/ipython-contrib/jupyter_contrib_nbextensions)

`conda install -c conda-forge jupyter_contrib_nbextensions`

执行了两三次都提示 `An HTTP error occurred when trying to retrieve this URL.`

后来翻墙问题解决。

3. 接着就可以通过在地址栏中输入

`<base_url>/nbextensions ` 来访问控制面板。

4. 启用 `Table of Contents (2)`

-----


## Links

* [jupyter 的插件: jupyter_contrib_nbextensions](https://github.com/ipython-contrib/jupyter_contrib_nbextensions/tree/master/src/jupyter_contrib_nbextensions/nbextensions/toc2)
