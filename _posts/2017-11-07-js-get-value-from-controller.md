---
layout: post
comments: true
title:  "JS 从 Rails 的 Controller 中获取值的错误用法"
categories:  Rails
tags:  Rails
date: 2017/11/07 08:29:24
---

* content
{:toc}

JS 从 Rails 的 Controller 中获取值的错误用法



## 错误用法示例

问题发在 [js.erb can't get params from Rails Controller](https://stackoverflow.com/questions/47147417/js-erb-cant-get-params-from-rails-controller)

下面的步骤，在以前也用过许多次，都可以的，但这次却在 js.erb 中一直不能正确读到从 Controller 中传过来的值。

**Controller**

```ruby
      def feeling
        @quiz = Quiz.find(params[:quiz_id])
        @question = @quiz.questions.find(params[:id])
        @feeling_value = params[:feeling_value]

        puts "question: #{@question.id}"
        puts "value: #{@feeling_value}"
      end
```

Output in the Rails server console:

```
    question: 67
    value: bad
```

**js.erb**

```js
    console.log("start");

    var feeling_value = <%= @feeling_value %>;
    console.log(feeling_value);

    console.log("end");
```

Output in the Chrome console:

```
    start
```


If comment *feeling_value*:

```js
    console.log("start");

    # var feeling_value = <%= @feeling_value %>;
    # console.log(feeling_value);

    console.log("end");
```

Output in the Chrome console,

```
    start
    end
```

**HTML**

```html
    <a class="item ui-selectee ui-selected" id="item-good" data-remote="true" rel="nofollow" data-method="post" href="/quizzes/5/questions/67/feeling?feeling_value=good">Good</a>
```




## 正确用法

修改 **js.erb**

```js
    console.log("start");

    var feeling_value = "<%= @feeling_value %>";
    console.log(feeling_value);

    console.log("end");
```

也即，在接收从 Controller 中传过来的值时，在 js.erb 中要用「""」 括起来。

这也是为了防范 XSS 跨站攻击，这点要注意！！




## 其它示例

```js
// homeland/app/views/likes/create.js.erb

<% if @success %>
  $("#<%= @element_id %>").attr("class","icon small_liked").data("method","delete").attr("title", "取消赞");
<% else %>
  App.alert("提交失败.")
<% end %>

// homeland/app/views/topics/update.js.erb
<% if @topic.errors.empty? %>
  <% flash[:notice] = t('topics.update_topic_success') %>
  Turbolinks.visit('<%= topic_path(@topic.id) %>');
<% else %>
  $('form[tb="edit-topic"] .alert').remove();
  $('form[tb="edit-topic"]').prepend('<%= j render("shared/error_messages", target: @topic) %>');
<% end %>


```


## Links

* [js.erb can't get params from Rails Controller](https://stackoverflow.com/questions/47147417/js-erb-cant-get-params-from-rails-controller)
