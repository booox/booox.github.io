---
layout: post
comments: true
title:  "Git 遇到 Grep"
categories: Linux
tags:  grep git
author: booox
date: 2017/04/27 20:25:00
---

* content
{:toc}

Git 是个非常强大的版本工具工具。Grep 是一个功能非常强大的文本搜索工具。当这两者结合在一起，会碰出什么火花呢？




## Grep 用法

`grep` 是一个在命令行模式下支持 [正则表达式](https://en.wikipedia.org/wiki/Regular_expression) 的强大的文本搜索工具。

### grep 常规的用法

* 普通查找 （在 *file_name* 中查找 *something* ）： `grep 'something' file_name`
* 使用通配符 （在所有扩展名为 *txt* 的文件中查找）： `grep 'something' *.txt`
* 使用正则表达式 （查找所有以 `a` 开头，以 `ple` 结尾的行）： `grep ^a.ple fruitlist.txt`

### grep 更多选项

* `-A num, --after-context=num` : 显示匹配内容后面的 num 行
* `-B num, --before-context=num` : 显示匹配内容前面的 num 行
* `-C [num], --context=num` : 显示匹配内容前面与后面的 num 行
* `-i, --ignore-case` : 忽略大小写
* `-i, --ignore-case` : 忽略大小写
* `-n, --line-number` : 显示在文件中的行号
* `-x, --line-regexp` : 严格匹配整行，只有整行完全匹配时才能匹配
* `-v, --invert-match` : 反向匹配，查找不包含匹配的行


## pipeline 管道命令

`pipeline` 管道命令，即 `|` 符号。可以用一句话来解释：

> 前面的输出，后面的输入

看实例： `ls -l | grep key | less`

将 `ls -l` 的输出作为 `grep key` 的输入
将 `ls -l | grep key` 的输出作为 `less` 的输入

## Git

关于 Git 以前写过，请参考另外几篇日志：

* [关于 Git workflow 的一个很好的解释]()


## 来看 Git 遇到 grep

### git + grep

* 先查看最近一次 commit 的修改内容，然后再在其中查找包含 `Admin` 的内容，并显示成功匹配内容处前三行，且忽略大小写
  * `git show HEAD | grep -i -A 3 'Admin'`

* 以大纲形式显示 commit 记录，且在其中查找 `Admin`
  * `git log --oneline -N | grep -i 'Admin'`



### `git grep`


* 将搜索到的内容进行替换
  * Mac: `git grep -l 'original_text' | xargs sed -i '' -e 's/original_text/new_text/g'`
  * Linux: `git grep -l 'original_text' | xargs sed -i 's/original_text/new_text/g'`

## Links

* [Grep wikipedia](https://en.wikipedia.org/wiki/Grep)
