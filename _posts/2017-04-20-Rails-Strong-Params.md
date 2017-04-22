---
layout: post
comments: true
title:  "Rails Strong Parameters"
categories: Rails
tags: rails
author: booox
---

* content
{:toc}

Rails Strong Parameters: 在 Rails 中通过 strong params 来设置允许通过的字段的白名单。



## 基本用法

在 Controller 中通过在 `private` 块中添加 `model_params` 方法来设定白名单。也即，允许放行的字段。

如在 `products_controller.rb` 中可以这样用：

```ruby
class PeopleController < ActionController::Base
  ...

  private

    def product_params
      params.require(:product).permit(:name, :category)
    end

end
```


## 允许数组

将允许 `:id` 为一个数组
`params.permit(:id => [])`

将允许整个 hash 作为参数 (不太确定)
`params.require(:log_entry).permit!`


## 嵌套参数

`params.permit(:name, {:emails => []}, :friends => [ :name, { :family => [ :name ], :hobbies => [] }])`

放行三个属性：`name` , `emails`, `friends`
其中: `emails` 为数组；而 `friends` 则为带有特定属性的数组， `name` 与 `hobbies` 。

## Links

* [Strong Parameters](https://github.com/rails/strong_parameters)
