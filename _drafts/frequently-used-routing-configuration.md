---
layout: post
comments: true
title:  "常见 Rails Routing 设置"
categories: Rails
tags: rails
date:
---

* content
{:toc}

关于 Rails 中 Routing (路由) 的一些设置。



## 基础知识

* 路由的作用有些像调度，当收到用户的访问请求之后，由它决定用哪一个 Controller 的 Action 来处理。
* 在 Rails 中，地址的请求通常设置为 `controller/action` 的形式
* 设置文件： *config/routes.rb*
* 在路由表当中，匹配顺序是从上到下，也即如果上面找到匹配的，就不会再往下找了。
* 验证路由信息，终端里执行： `rake routes`
  * ![]({{ site.url }}/images/rake-routes.png)


## 常见用法

### `resources` 与 CRUD

用法举例： `resources :photos`

* 它会用创建七个不同的 `routes`，全部指向 `Photos` 这个 `controller`
* CRUD 即常见的几种操作数据库的方法

```
    Prefix Verb   URI Pattern                       Controller#Action
      jobs GET    /jobs(.:format)                   jobs#index
           POST   /jobs(.:format)                   jobs#create
   new_job GET    /jobs/new(.:format)               jobs#new
  edit_job GET    /jobs/:id/edit(.:format)          jobs#edit
       job GET    /jobs/:id(.:format)               jobs#show
           PATCH  /jobs/:id(.:format)               jobs#update
           PUT    /jobs/:id(.:format)               jobs#update
           DELETE /jobs/:id(.:format)               jobs#destroy

```

  * 其中 `PATCH` 与 `PUT` 均为 `update` 指定的 `photo`
