---
layout: post
comments: true
title:  "在 Rails 中使用 Ajax 的小结"
categories:  Ajax Rails
tags:  ajax rails jquery
date: 2017/11/18 09:45:15
---

* content
{:toc}

在 Rails 中使用 Ajax 的小结



这段时间做的项目中不少地方用到了 Ajax，这里稍微理一下。

## 什么时候用 Ajax ?

当我们不想刷新页面，而直接更改页面的部分内容时，就要想到用 Ajax 。

## 流程

### 用户角度

用户操作 -> 效果实现（页面无需刷新）

### 开发者角度

**Controller**

> 处理并返回数据 (json)

* 只需要返回一个数据

可以直接写在 `json:` 后面

```ruby
respond_to do |format|
  format.json {render json: @size}
end
```

或

`render json: @size`

* 需要返回多个数据

在 `json:` 后面用 hash 表示

```ruby
respond_to do |format|
  format.json {render json: {id: @id, size: @size }}
end
```

或

`render json: {id: @id, size: @size }`




**application.js**

> 绑定事件，获取数据，下步处理

1. 绑定事件到 `HTML` 元素
2. 声明请求类型:  `method` (`GET`, `DELETE`, `POST`)
3. 传递参数给指定的 `url`，用于处理请求
4. 声明服务器返回的数据类型 `dataType` (`json`, `script`, `html`)
5. 根据返回数据(`data`)，用 jQuery 完成下一步的处理
  * 如删除元素(`$("#post-" + data["id"]).remove();`)，
  * 添加样式(`addClass`)


这是一个示例：


```js
$(document).on("turbolinks:load", function() {
  $("[id^=quiz_category_id_]").on("change", function(t) {
    t.preventDefault();
    var category_id = $(this).val();

    $.ajax ({
      method: 'GET',
      url: "/admin/categories/" + category_id + "/get_questions_size",
      dataType: 'json',
      success: function(data){
        console.log(data);
      }
    });
  });
});
```
