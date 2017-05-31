---
layout: post
comments: true
title:  "Rails rake dev errors"
categories:  Rails
tags:  rails
date: 2017/05/25 08:37:56
---

* content
{:toc}

使用了 `rake dev` 来生成测试数据，出现了两个问题。



## rake task


### 新增任务

*lib/tasks/dev.rake*

```ruby

namespace :dev do

  task :fake => :environment do
    User.delete_all
    Event.delete_all

    users = []
    users << User.create!( :email => "admin@example.org", :password => "12345678" )

    10.times do |i|
      users << User.create!( :email => Faker::Internet.email, :password => "12345678")
      puts "Generate User #{i}"
    end

    20.times do |i|
      topic = Event.create!( :name => Faker::Cat.name,
                             :description => Faker::Lorem.paragraph,
                             :user_id => users.sample.id )
      puts "Generate Event #{i}"
    end
  end

  task :fake_event_and_registrations => :environment do
    event = Event.create!( :status => "public", :name => "FullStack Meetup", :friendly_id => "fullstack-meetup")
    t1 = event.tickets.create!( :name => "Guest", :price => 0)
    t2 = event.tickets.create!( :name => "VIP G1", :price => 190)
    t3 = event.tickets.create!( :name => "VIP G2", :price => 199)

    1000.times do |i|
      event.registrations.create!( :status => ["pending", "confirmed"].sample,
                                   :ticket => [t1, t2, t3].sample,
                                   :name => Faker::Cat.name, :email => Faker::Internet.email,
                                   :cellphone => "123456789", :bio => Faker::Lorem.paragraph,
                                   :created_at => Time.now - rand(10).days - rand(24).hours )
    end

    puts "Let's visit http://localhost:3000/admin/events/fullstack-meetup/registrations"
  end

end

```

### 运行

运行时则使用命令 `rake 任务文件名:方法名`，如:

`rake dev:fake`
`rake dev:fake_event_and_registrations`


## 报错

在实际运行中遇到如下问题：

### rake aborted! ActiveRecord::RecordInvalid: 验证失败: 至少需要一个 Ticket

rake 任务被忽略了。

原因是 Event 在 model 中限制了 `create` 时一定要有 `ticket`

```ruby
 class Event < ApplicationRecord
   #...
   validate :check_tickets

   protected

   def check_tickets
     if self.tickets.empty?
       self.errors.add(:base, "至少需要一个 Ticket")
     end
   end
   # ...
end
```

解决办法：

在运行 `rake dev:fake_event_and_registrations` 之前把 `validate :check_tickets` 注释掉。


### Couldn't find Event

> ActiveRecord::RecordNotFound in Admin::EventRegistrationsController#index

![]({{site.url}}/images/canot-find-event.png)

* 原因是：在 `dev.rake` 中创建 Event 时指定了 `friendly_id`
* 但在 *event.rb* 中设定 `friendly_id` 却是另一回事

*app/models/event.rb*


```ruby
 class Event < ApplicationRecord
   #...
   before_validation :generate_friendly_id, :on => :create

   protected

   def generate_friendly_id
     self.friendly_id = SecureRandom.uuid
   end
   # ...
end
```

可见在 `event.rb` 中将 `friendly_id` 写死成 `SecureRandom.uuid`

* 解决办法，将上述语句修改为：

`self.friendly_id ||= SecureRandom.uuid`
