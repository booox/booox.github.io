---
layout: post
comments: true
title:  "Rails 常见报错 Error"
categories: Rails
tags:  rails-error
author: booox
date: 2017/04/28 22:40:52
---

* content
{:toc}

这是一个 Rails 的报错列表





## Rails 报错

> `undefined method 'empty?' for 5000:Integer`

![]({{site.url}}/images/empty-5000-integer.png)

  * 错误原因是对数字应用 `simple_format` 方法了。将原来的 `<%= simple_format(@job.wage_upper_bound) %>` 修改为 `<%= @job.wage_upper_bound %>`


> `check_controller_and_action is not a supported controller name. This can lead to potential routing problems.`

![]({{site.url}}/images/potential-routing-problems-1.png)

![]({{site.url}}/images/potential-routing-problems-2.png)

  * 这个错误提示的信息，其实相对并不是很详尽。不过通过 `This can lead to potential routing problems.` ，大致推断，可能与 `routes.rb` 有关。检查后发现多了一行，只有 `resources`，去掉后问题解决。


>  `rm file1 file2`

![]({{site.url}}/images/rm-file1-file2.png)

  * 这其实不是一个 Rails 的报错
  * 他本想执行的是：`mv app/assets/stylesheets/application.css app/assets/stylesheets/application.scss`
  * 却把命令错打成: `rm  app/assets/stylesheets/application.css app/assets/stylesheets/application.scss`
  * 其实就是一个终端命令不熟悉的而导致的。当执行 `rm file1 file2` 这样它会将 file1 与 file2 一并删除，而实际情况是 file1 是存在的，所以被删除掉了。而 file2 却是不存在的，所以它提示 `No such file or directory`

> Routing Error: undefined local variable or method `protected` for main: Object

![]({{site.url}}/images/undefined-protected-1.png)

  * 最上面显示的是 `Routing Error` ，首先考虑可能是 `config/routes.rb`，检查后发现并没有问题
  * 考虑提示 `protected` ，此为 controller 中定义的方法，接着查看 `jobs_controller.rb` 就发现问题了。其实就是关闭的 `end` 的位置不对引起的。
  * ![]({{site.url}}/images/undefined-protected-2.png)


  
