---
layout: post
title:  "Rails Sqlite3 Segmentation fault"
categories: Rails
tags: rails
author: booox
---

* content
{:toc}

在 `rails c` 里面执行 `Job.create!(:title => "Foo", :description => "Bar")`，报错：
`sqlite3_adapter.rb:27: [BUG] Segmentation fault at 0x00000000000110`


### 解决办法

将 `gem 'sqlite3'` 移动到 `group :development, :test do`
变成：

```ruby
group :development, :test do
  gem 'byebug', platform: :mri
  gem 'sqlite3'
end
```

而后 `bundle install`
重启 `rails s`
问题解决。
