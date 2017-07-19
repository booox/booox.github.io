---
layout: post
title:  "API 默认格式错误: missing a template for this request format and variant. "
categories: rails-error
tags:  errors rails
date: 2017/07/19 10:46:58
---

* content
{:toc}

Api::V1::TrainsController#show is missing a template for this request format and variant.


## 错误：

在练习 「Jbuilder 用法」 时，出现如下错误：

`Api::V1::TrainsController#show is missing a template for this request format and variant. request.formats: ["text/html"] request.variant: []`

![]({{site.url}}/images/api-routes-defaults-format-error.png)

## 分析及解决：

* 已经新增文件 *app/views/api/v1/trains/show.json.jbuilder* ，是 JBuilder 模板，即相当于 views 文件，用来定义 JSON 长什么样子。

```
json.number @train.number
json.available_seats @train.available_seats
json.created_at @train.created_at
```

* 修改 *app/controllers/api/v1/trains_controller.rb* ，拿掉 render :json 的部分

```
def show
  @train = Train.find_by_number!( params[:train_number] )

  # render :json => {
  #   :number => @train.number,
  #   :available_seats => @train.available_seats,
  #   :created_at => @train.created_at
  # }
end
```

经过此两步之后，当访问 *http://localhost:3000/api/v1/trains/0822* 时会有数据流出。

分析原因，应该是 rails 不知道如何处理这种格式，而实际上在 **Routes** 中印象中是设置了默认的格式。

仔细查看 *config/routes.rb* 后发现，少了一个 `s`

```ruby
namespace :api, :default => { :format => :json } do
  namespace :v1 do
    #...
  end
end
```

正确的应该为：

```ruby
namespace :api, :defaults => { :format => :json } do
  namespace :v1 do
    #...
  end
end
```

真是「一字之差，谬以千里」。

## `defaults` 到底做什么用

查看 [Rails Routing from the Outside In](http://guides.rubyonrails.org/routing.html)

> You can define defaults in a route by supplying a hash for the :defaults option.


对多数的 APIs 来说，只需要返回 JSON 类型的数据，所以在 API 的 Controller 中会普遍使用 `respond_to` 。

```ruby
class API::V1::PeopleController < ApplicationController

  def index
    @people = Person.all
    respond_to do |format|
      format.json { render :json => @people }
    end
  end

end
```

如果我们去掉`respond_to`，而访问的 *url* 中又不带 `json`，那么在 *logs* 中会有这样的提示：

`Processing by Api::V1::TrainsController#show as HTML`

我们也可以在 *routes* 中设置默认的格式：

```ruby
namespace :api, :defaults => {:format => :json} do
  namespace :v1 do
    resources :people
  end
end
```

此时，查看 *logs* ：

`Processing by Api::V1::TrainsController#show as JSON`
