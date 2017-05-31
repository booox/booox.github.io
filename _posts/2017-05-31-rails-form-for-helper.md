---
layout: post
comments: true
title:  "Rails form_for helper"
categories:  Rails
tags:  rails
date: 2017/05/31 21:32:12
---

* content
{:toc}

想了解 form_for 的一些用法。



## 用法举例

*创建新person*
```ruby
<%= form_for @person do |f| %>
  <%= f.label :first_name %>:
  <%= f.text_field :first_name %><br />

  <%= f.label :last_name %>:
  <%= f.text_field :last_name %><br />

  <%= f.submit %>
<% end %>
```

 它会产生 HTML 如下：

 ```html
 <form action="/people" class="new_person" id="new_person" method="post">
  <input name="authenticity_token" type="hidden" value="NrOp5bsjoLRuK8IW5+dQEYjKGUJDe7TQoZVvq95Wteg=" />
  <label for="person_first_name">First name</label>:
  <input id="person_first_name" name="person[first_name]" type="text" /><br />

  <label for="person_last_name">Last name</label>:
  <input id="person_last_name" name="person[last_name]" type="text" /><br />

  <input name="commit" type="submit" value="Create Person" />
</form>
 ```

 特别要注意最后的 `f.submit` 会自动生成值为 `Create Person` 的按钮

 * `PeopleController#new` 对应 `Create`
 * `PeopleController#update` 对应 `Update`
 * 后面接 Class 的名 'Person'

## Links

* [FormHelper](http://api.rubyonrails.org/classes/ActionView/Helpers/FormHelper.html)
* [How does <%= f.submit %> know where to post?](https://stackoverflow.com/questions/32663333/how-does-f-submit-know-where-to-post)
