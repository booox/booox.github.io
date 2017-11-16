---
layout: post
comments: true
title:  "在实现 Leaderboard 中对Redis 获取的内容进行分页"
categories:  Rails
tags:  Rails Redis
date: 2017/11/13 14:44:55
---

* content
{:toc}

在实现 Leaderboard 中对Redis 获取的内容进行分页



## 获取内容

**Model**

```ruby

# leaderboard.rb
class Leaderboard
  # ...

  def self.top(key, limit = -1)
    top = Redis.current.zrevrange(key, 0, limit, :withscores => true)
    leaders = []
    top.each_with_index do |e, i|
      leaders << {display_name: e[0], score: e[1].to_i, rank: i + 1}
    end

    leaders
  end

  # ...
end
```

**Controller**

```ruby
@leaders = Leaderboard.top(key).paginate( :page => params[:page], per_page: 10 )
```

> 这里获取到的 `@leaders` 为 Array


## 分页实现

1. 新建文件 `config/initializers/will_paginate_array_fix.rb`

```ruby
# will_paginate_array_fix.rb

require 'will_paginate/array'
```

创建好之后,重启 `rails s`

2. 分页

`my_array.paginate(:page => x, :per_page => y)`



## Links

* [Ruby on Rails will_paginate an array](https://stackoverflow.com/a/8407304/4455426)
