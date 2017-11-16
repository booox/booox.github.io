---
layout: post
comments: true
title:  "给 Rails 的 model 加上 schema 的注释"
categories:  Rails
tags:  Rails
date: 2017/11/07 20:50:48
---

* content
{:toc}

给 Rails 的 model 加上 schema 的注释



## 先看效果

```ruby
# == Schema Information
#
# Table name: types
#
#  id         :integer          not null, primary key
#  title      :string
#  created_at :datetime         not null
#  updated_at :datetime         not null
#

class Type < ApplicationRecord
  validates_presence_of :title

  has_many :questions
end
```

## 实现方法

一开始以为是手工添加上去的，还好去搜了一下，结果还真有现成的 Gem。

就是 [ctran/annotate_models](https://github.com/ctran/annotate_models) 。

1. 安装

Gemfile:

```ruby
group :development do
  gem 'annotate'
end
```

执行 `bundle install`，重启 `rails s`

2. 使用

直接在项目根目录下运行 `bundle exec annotate`



## Links

* [ctran/annotate_models](https://github.com/ctran/annotate_models) 
