---
layout: post
comments: true
title:  "Heroku 常见命令"
categories: Rails
tags:  rails heroku
author: booox
date: 2017/04/22 21:02:30
---

* content
{:toc}

Heroku, 一个很受欢迎的云平台，特点是不需要配置环境，代码推送过来就完成部署了。底层系统是基于 Ubuntu 的。



### 添加连接

* 查看 remote : `git remote -v`
* 添加远程仓库： `git remote add heroku git@heroku.com:xxx-xxx-xx.git`
* 删除本地 remote: `git remote rm heroku`
* 推送本地分支到 heroku 的 master 分支： `git push heroku 本地欲推送的分支名:master`

### 基础指令

* 登陆 heroku： `heroku login`
* 新建 heroku 应用： `heroku create`
* 打开 heroku 应用：`heroku open` (在浏览器中)




## 在 heroku 执行 rails/rake 命令

`heroku run rails console`
`heroku run rake db:migrate`

* heroku 三兄弟
`heroku run rake db:drop`
`heroku run rake db:create`
`heroku run rake db:migrate`

`heroku run rake db:seed`


## 常用查错命令

可以用 `heroku logs` 搭配 `grep` 命令来快速查错。
> 注意下面命令中的 `|` ，这个被称为管道命令，可以这样认为将 `|` 前面的输出当作后面的输入
> `-i` : 忽略大小写

* 查看错误:
  * `heroku logs | grep -i error` (`-i` 不区分大小写)
* 查看 model 对应关系：
  * `heroku logs grep -i -E 'belongs_to|has_many’`
* 查找 bootstrap 时：
    * `heroku logs grep -i bootstrap`
    * `git grep -i --name-only bootstrap`






## 常见错误

> `Heroku can't be established`

![](https://ww3.sinaimg.cn/large/006tKfTcgy1few90o2idyj30t807540j.jpg)

* 这种情况多数由于网络的原因，与 heroku 没有建立连接，可以稍候再试，或重启网络，甚至使用 VPN ……


> 快速查错

执行 `heroku logs | grep -i error` ，可以快速检索出 heroku 报错的位置

![]({{site.url}}/images/sanitizer-allowed-tags.png)


> ActionView::Template::Error Missing host to link to!

`config/environments/production.rb` 里添加下面这样一句

`Rails.application.routes.default_url_options[:host] = 'myappsname.herokuapp.com'`

[Heroku/devise - Missing host to link to! Please provide :host parameter or set default_url_options(http://stackoverflow.com/questions/4114835/heroku-devise-missing-host-to-link-to-please-provide-host-parameter-or-set-d)
