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

>

  * `rails s` 非正常关闭或试图再次打开： `kill -9 $(lsof -i tcp:3000 -t)`

> fatal in Admin::JobsController#publish : exception reentered

![]({{site.url}}/images/exception-reentered-1.png)


  * 报这个错，`exception reentered` ，搜索了一下，好像都与无限循环有关。可能是循环调用了相同的方法。
  * 最终错误的原因，还真是这个原因
  * ![]({{site.url}}/images/exception-reentered-2.png)
  * 从图中可以看到 定义了一个名称为 `params` 的方法，而在这个方法内又有 `params.require(:job)` 也就是又调用了 `params` ，于是就出现循环调用。


> NameError in Admin : undefined local variable or method 'job_params'

![]({{site.url}}/images/job_params.png)


  * 这个报错的原因是，当 rails 需要调用 `job_params` 这个方法时，却在下面找不到
  * 后来发现方法名称错写成了 `jobs_params`


> NameError : undefined local variable or method `“Logout”' for`

  ![]({{site.url}}/images/link_to-nameError.png)
  * 拼写错误： `Logout` 左右两边的引号为中文的引号


> ArgumentError: No association found for name `profile'. Has it been defined yet?``

![]({{site.url}}/images/no-association-name-profile.png)


* 这个原因，主要是 语句的顺序错了，在定义 `:profile` 之前调用 `:profile` 了，顺序换一下。



> 1 error prohibited this person from being saved: Tickets event must exist

![]({{site.url}}/images/must-exist-inverse-of.png)

  * 这是一对多的练习
    * event: `has_many :tickets, :dependent => :destroy`
    * ticket: `belongs_to :event`
  * 编辑倒是正常，可是新建却报错
  * 最后将 `has_many` 修改如下，问题解决。
    * event: `has_many :tickets, inverse_of: :event, :dependent => :destroy`
  * ref links:
    * [has_many and nested attributes](https://github.com/rails/rails/issues/25198)
    * [has_many](http://api.rubyonrails.org/classes/ActiveRecord/Associations/ClassMethods.html#method-i-has_many)


> AASM::UnknownStateMachineError (There is no state machine with the name 'default' defined in Object)

![]({{site.url}}/images/aasm-state-error-01.png)


* model 中找不到对应的状态的定义
* 原因是　`include AASM` 那一块放到类的定义之外了。
* ![]({{site.url}}/images/aasm-state-error-02.png)


> NameError: uninitialized constant Product::ImageUploader

* 实际操作时， 重启 rails ，并没有解决问题
* 重新 `bundle install` ，之后再次重启 rails 问题竟然消失了。
