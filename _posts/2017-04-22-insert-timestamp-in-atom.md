---
layout: post
comments: true
title:  "在 Atom 中快速插入时间戳"
categories: Software
tags: atom date timestamps
author: booox
date: 2017/04/22 09:09:12
---

* content
{:toc}

如何在 Atom 中可以快速插入时间戳？



也有现成的插件，也可以自己写。这里给出自己写的实现方法。

### 1. 修改 Atom 启动脚本

*/Users/your_name/.atom/init.coffee*

添加如下代码：

```js
# add a command to insert the current date and time

daysOfWeek = ['日', '一', '二', '三', '四', '五', '六']
pad = (str, length) ->
  str = String(str)
  str = '0' + str  while(str.length < length)
  str
dateOrTime = (kind) ->
  now = new Date()
  yyyy = now.getYear() + 1900
  mm = pad(now.getMonth() + 1, 2)
  dd = pad(now.getDate(), 2)
  ddd = daysOfWeek[now.getDay()]
  hh24 = pad(now.getHours(), 2)
  mi = pad(now.getMinutes(), 2)
  ss = pad(now.getSeconds(), 2)
  if kind == 1
    "#{yyyy}/#{mm}/#{dd}(#{ddd})"
  else if kind == 2
    "#{hh24}:#{mi}:#{ss}"
  else
    # "#{yyyy}/#{mm}/#{dd}(#{ddd}) #{hh24}:#{mi}:#{ss}"
    "#{yyyy}/#{mm}/#{dd} #{hh24}:#{mi}:#{ss}"

insertText = (str) ->
  return unless editor = atom.workspace.getActiveTextEditor()
  selection = editor.getLastSelection()
  selection.insertText(str)

atom.commands.add 'atom-text-editor', 'atom-date:date', ->
  insertText(dateOrTime(1))
atom.commands.add 'atom-text-editor', 'atom-date:time', ->
  insertText(dateOrTime(2))
atom.commands.add 'atom-text-editor', 'atom-date:datetime', ->
  insertText(dateOrTime(0))

```

### 2. 设置快捷键

*/Users/your_name/.atom/keymap.cson*

添加如下代码：

```
'atom-text-editor':
  'alt-=': 'atom-date:date'
  'alt-;': 'atom-date:time'
  'cmd-shift-I': 'atom-date:datetime'
```

设置完重启 Atom

### 3. 使用方法

`Alt + =` : `2017/04/22(六)`
`Alt + -` : `08:55:42`
`Command + Shift + I` : `2017/04/22 09:05:37`



### Links

* [Atomに現在日時を挿入するコマンドを追加する](http://qiita.com/toruot/items/b26fde1a898bb52985e1)
