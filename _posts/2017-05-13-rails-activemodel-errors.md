---
layout: post
comments: true
title:  "Rails ActiveModel::Errors"
categories:  Rails
tags:  rails
date: 2017/05/13 22:24:37
---

* content
{:toc}

在 Rails 中 errors 是由 `ActiveModel::Errors` 来处理的。


## Erros 就是一个 hash

`ActiveModel::Errors` 实际是将错误的信息封装成一个 hash 表。它包括属性以及属性的值。

有这样几个基础的方法：

* `add` : 可以通过 `add` 方法来添加属性和值
* `i18n` : 可以将错误类型翻译成不同的语言


### 例一

```ruby

u1 = User.last
errors = ActiveModel::Errors.new(u1)
errors.add(:name, "Errors Name")
errors.add(:base, "Test Base")
errors.add(:base, "Just a base test")
errors.messages
errors.details
errors
# => @messages={:base=>["Test Base", "Just a base test"]},
# => @details={:base=>[{:error=>"Test Base"}, {:error=>"Just a base test"}],

```

可见， messages， details 均为 hash


##


## 创建 model 时对必要条件进行验证

如 `Event` 创建时，需要至少包含一个 `ticket` ，可以通过 `Nested Form` 来处理。同时可以在 model 中加以限制。则可以这样处理：

```ruby
class Event
  validate :check_tickets

  def check_tickets
    if self.tickets.empty?
      self.errors.add(:base, "至少需要一个Ticket")
    end
  end
end
```

这样，当创建 `Event` 时，若没有创建至少一个 `ticket` ，则会报错。

### 例二

```ruby
class User < ActiveRecord::Base
  validates :name, presence: true
end

user = User.new
user.valid?
# => false
user.errors.details
# => {name: [{error: :blank}]}
```

### 例三

```ruby
class User < ActiveRecord::Base
  validate :adulthood

  def adulthood
    errors.add(:age, :too_young, years_limit: 18) if age < 18
  end
end

user = User.new(age: 15)
user.valid?
user.errors.details
# => {age: [{error: :too_young, years_limit: 18}]}

```


## Links

* [Dig into the rails errors
Errors](http://neethack.com/2015/04/dig-into-the-rails-errors/)
* [How to use ActiveModel errors details](https://cowbell-labs.com/2015-01-22-active-model-errors-details.html)
* [ActiveModel::Errors < Object](http://api.rubyonrails.org/classes/ActiveModel/Errors.html#method-i-empty-3F)
