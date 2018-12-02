---
layout: post
comments: true
title:  "Rails 5 中 accepts_nested_attributes_for 不起作用"
categories: rails
tags: rails
date: 2018/06/10 19:08:50
---

* content
{:toc}

Rails 5 中 accepts_nested_attributes_for 不起作用。




如果你在 Rails 5 中使用 `accepts_nested_attributes_for`　并且是 `has_many` 的关系，那么你可能需要使用 `inverse_of` 这个选项。

具体见下面实例

## Model

有三个 model 分别为 `course`, `section` 与 `assignments`。

```ruby

class Course < ApplicationRecord
  has_many :sections, dependent: :destroy
  has_many :assignments, through: :sections
end


class Section < ApplicationRecord
  belongs_to :course

  has_many :assignments, dependent: :destroy
  accepts_nested_attributes_for :assignments, allow_destroy: true

end

class Assignment < ApplicationRecord
  belongs_to :section
end

```

## Controllers

```rb
class Admin::SectionsController < ApplicationController
  before_action :authenticate_user!
  before_action :admin_required


  def new
    @course = Course.find(params[:course_id])
    @section = @course.sections.new
    @section.assignments.build
  end

  def create
    @section = Section.new(section_params)
    @course = Course.find(params[:course_id])
    @section.course_id = @course.id

    if @section.save
      redirect_to admin_course_sections_path(@course)
    else
      render :new
    end
  end


  private

  def section_params
    params.require(:section).permit(:title, :content, :course_id,
                  assignments_attributes: [:id, :title, :content, :_destroy])
  end
end

```

一直未能成功添加　`assignments`

后来加上了 `inverse_of` 之后，马上可以了。

```ruby
class Section < ApplicationRecord
  belongs_to :course

  has_many :assignments, dependent: :destroy, inverse_of: :section
  accepts_nested_attributes_for :assignments, allow_destroy: true

end
```


## Links

* [accepts_nested_attributes_for with Has-Many-Through Relations](https://robots.thoughtbot.com/accepts-nested-attributes-for-with-has-many-through)
* [请问有哪位能讲讲 inverse_of？](https://ruby-china.org/topics/8560)
