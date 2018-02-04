---
layout: post
comments: true
title:  "让 Jupyter 支持 Javascript"
categories: software
tags: software jupyter javascript
date: 2018/02/04 08:15:59
---

* content
{:toc}

让 Jupyter 支持 Javascript



## 环境

macOS
Anaconda

## 安装

在 Terminal 中依次执行：

```
conda install nodejs
npm install -g ijavascript
ijsinstall
```
## 应用

此时再打开 Jupyter Notebook 就可以在新建中看到 `javascript(Node.js)` 了。

## Links

* [ijavascript Installation](http://n-riesco.github.io/ijavascript/doc/install.md.html)
* [ijavascript](https://github.com/n-riesco/ijavascript)
* [Jupyter kernels](https://github.com/jupyter/jupyter/wiki/Jupyter-kernels)
