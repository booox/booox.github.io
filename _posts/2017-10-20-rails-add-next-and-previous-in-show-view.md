---
layout: post
comments: true
title:  "如何在 Rails 的 Show 视图中添加「下一页」与「上一页」"
categories: Rails
tags: rails view
author: booox
date: 2017/10/20 12:43:58
---

* content
{:toc}



如何在 Rails 的 Show 视图中添加「下一页」与「上一页」


用分页的 Gem 可以很容易在 index 的视图中实现分页，可是如何在 show 的视图中也显示分页呢？

一开始用关键词 `show next previous` 没有搜索到有用的
后来 换了关键词，`has_many next previous` 就找到几条有用的记录


## 实现在 Show 视图中分页

1. Modle

```ruby
# post.rb

def previous_post
    previous_posts = Post.where [ 'created_at < ?', self.created_at ]
    previous_post = previous_posts.find(:all, :order => 'created_at DESC').first

    previous_post.nil? ? Post.find(:all, :order => 'created_at DESC').first : previous_post
  end

  def next_post
    next_posts = Post.where [ 'created_at > ?', self.created_at ]
    next_post = next_posts.find(:all, :order => 'created_at ASC').first

    next_post.nil? ? Post.find(:all, :order => 'created_at ASC').first : next_post
  end

```

方法如下，具体根据情况进行修改，如可将 `created_at` 换成 `id`, `title` 等字段

ref: [post.rb](https://github.com/Jauny/profile/blob/7592d83393e4cb43f3df77576a04d353d9272bca/app/models/post.rb)


2. Conttroller

```ruby
# post_controller.rb

def show
  @post = Post.find(params[:id])
  @previous = @post.previous_post
  @next = @post.next_post
end
```


3. View

```
<div class="previous"><%= link_to "...#{@previous.title}", post_path(@previous) %></div>
<div class="next"><%= link_to "#{@next.title}...", post_path(@next) %></div>
```

## 当 post 属于某一类时

有时需要处理类似当 post 属于某一分类时的情景

如 post `belongs_to :category`

可以这样处理

```ruby

# model/post.rb
belongs_to :category

def next
  category.posts.where("time > ?", time).order(:time).first
end

def previous
  category.posts.where("time < ?", time).order(time: :desc).first
end

# model/category.rb
has_many :posts

# routes.rb
resources :categories do
    resources :posts, shallow: true
  end
end



# posts_controller.rb
def show
  @category = Category.find_by_id(@post.category_id)
  @posts = @category.posts

  @previous_post = @post.previous
  @next_post = @post.next
end
```

在另一个实例中，甚至使用了查询语句

```ruby
def show
  @quiz = Quiz.find(params[:quiz_id])
  @question = @quiz.questions.find(params[:id])

  @previous_question = @quiz.questions.where('quiz_questions.quiz_id = ? and questions.id < ?', @quiz.id, @question.id).order('id DESC').first
  @next_question = @quiz.questions.where('quiz_questions.quiz_id = ? and questions.id > ?', @quiz.id, @question.id).order('id ASC').first

end
```


## Links

* [Rails 4: get next / previous record of an object that belongs to another object](https://stackoverflow.com/questions/33380851/rails-4-get-next-previous-record-of-an-object-that-belongs-to-another-object)
* [“Previous post” and “Next post” link in Show View (Nested Resources)](https://stackoverflow.com/questions/19471430/previous-post-and-next-post-link-in-show-view-nested-resources)
