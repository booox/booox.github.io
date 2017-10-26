---
layout: post
comments: true
title:  "如何在 Rails 中给 Question 添加 favorite"
categories: Rails
tags: rails assciations
author: booox
date: 2017/10/23 23:20:42
---

* content
{:toc}



如何在 Rails 中给 Question 添加 favorite



## 1. 添加 favorite Model

`rails g model favorite`

编辑 *create_favorites.rb*

```ruby
t.integer :user_id, :integer, index: true
t.integer :question_id, :integer, index: true
```

而后执行 `rake db:migrate` 生成对应表格

## 2.  Associaitons

has_many

```ruby
# user.rb

has_many :favorites
has_many :favorite_questions, :through => :favorites, :source => :question


def favorite_question?(question)
  self.favorite_questions.include?(question)
end

# question.rb
has_many :favorites
has_many :favorite_users, :through => :favorites, :source => :user

# favorite.rb
belongs_to :user
belongs_to :question

```


## 3. Controller


```ruby

# questions_controller.rb

def show
  @question = Questions.find(params[:id])

  # .....

  @not_favorite = current_user && ! current_user.favorite_question?(@question)
  @is_favorite = current_user && current_user.favorite_question?(@question)
end

def favorite
  @question = Questions.find(params[:id])
  @not_favorite = current_user && ! current_user.favorite_question?(@question)
  @is_favorite = current_user && current_user.favorite_question?(@question)

  if @not_favorite
    current_user.favorite_questions << @question

  elsif @is_favorite
    current_user.favorite_questions.delete(@question)
  end
end
```

## 4. Routes

```ruby
resources :questions do
  member do
    post :favorite
  end
end
```

## 5. Helper

```ruby
# for show page
def display_question_favorite(not_favorite, is_favorite)
  if not_favorite
    content_tag(:i, nil, class: 'fa fa-star-o', 'aria-hidden': true, id: "favorite_icon")

  elsif is_favorite
    content_tag(:i, nil, class: 'fa fa-star', 'aria-hidden': true, id: "favorite_icon")
  end
end
```

## 5. View

```ruby
# questions/show.html.erb
<div class="col-xs-3">
  <%= link_to favorite_question_path(@question.id), method: :post, remote: true do%>
    <%= display_question_favorite(not_favorite, is_favorite) %>
  <% end %>
  <span id="favorite_text">收藏</span>
</div>
```

## 6. respond_to

```js
# questions/favorite.js.erb

var not_favorite = <%= @not_favorite %>;
var is_favorite = <%= @is_favorite %>;

if(not_favorite){   
  $( "#favorite_icon" ).removeClass('fa fa-star-o').addClass('fa fa-star');
  $( "#favorite_text" ).text( "取消收藏" );
}else if(is_favorite){
  $( "#favorite_icon" ).removeClass('fa fa-star').addClass('fa fa-star-o');
  $( "#favorite_text" ).text( "收藏" );
};
// 注意这里的顺序，当获取 not_favorite 时，是没有被收藏的，但执行到这里是却又是已经被收藏了

```


## Links
