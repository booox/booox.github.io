---
layout: post
comments: true
title:  "对 Rails 的字段进行有条件的验证"
categories: Rails
tags: rails validation
author: booox
date: 2017/10/27 06:19:16
---

* content
{:toc}



对 Rails 的字段进行有条件的验证



## 1. 要实现的功能

```ruby
class Exam < ApplicationRecord
  validates :title, presence: true
  validates :subject_id, presence: true, if: :no_exam_type?

  belongs_to :subject

  def no_exam_type?
    !self.exam_type.present?
  end
end
```

想实现对 `exam` 的 `subject_id` 进行验证
当 `exam_type` 字段为空时，`subject_id` 一定要存在
而当 `exam_type` 字段不为空时，`subject_id` 不用存在

但上面的写法一直不能成功。

通过 `rails c` 去测试 `Exam.create!(title: "title test", exam_type: "test")` 一直报错：

`ActiveRecord::RecordInvalid:  验证失败: Subject 不能为空`

## 2.  解决

给 `belongs_to` 加上 `optional: true`

因为:

> 4.1.2.11 :optional
If you set the :optional option to true, then the presence of the associated object won't be validated. By default, this option is set to false.

默认情况下， `belongs_to` 是一定要存在的，也即 `optional: false`

所以将代码修改如下，即可：

```ruby
# exam.rb

class Exam < ApplicationRecord
  validates :title, presence: true
  validates :subject_id, presence: true, if: :no_exam_type?

  belongs_to :subject, optional: true

  def no_exam_type?
    !self.exam_type.present?
  end
end
```


## Links

* [Active Record Associations](http://guides.rubyonrails.org/association_basics.html)
* [How to validates Rails column with self other column not blank?](https://stackoverflow.com/a/46958326/4455426)
