## 未解决

### 如何对已添加的 gem 进行修改？



### 使用 redcarpet 不能渲染表格？

按照 []() 的提示实现了在 Rails 中将 Markdown 文本转换成对应的 HTML。

具体代码如下：

*app/helpers/application_helper.rb*

```ruby
# for markdown
class CodeRayify < Redcarpet::Render::HTML
  def block_code(code, language)
    CodeRay.scan(code, language).div
  end
end

def markdown(text)
  render_options = {
    filter_html: true,
    hard_wrap: true,
    link_attributes: { rel: 'nofollow', target: '_blank' }
  }
  coderayified = CodeRayify.new( render_options )

  options = {
      :fenced_code_blocks => true,
      :no_intra_emphasis => true,
      :autolink => true,
      :tables => true,
      :strikethrough => true,
      :lax_html_blocks => true,
      :superscript => true
  }
  markdown_to_html = Redcarpet::Markdown.new(coderayified, options)
  markdown_to_html.render(text).html_safe
end
# end: markdown
```

在使用过程中发现存在以下问题：

1. 表格不能渲染？（已加上 `tables: true`)
2. 链接生成时不能添加 `target` (已加上： `link_attributes: { rel: 'nofollow', target: '_blank' }`)
3. 多行代码一定要加上语言的名称，否则会报错！(即在多行代码开始标记，连续三个符号后面加上类似 `javascript`, `ruby` 的字样)。
4. 代码语法关键字不能高亮。

**原因及解决办法：**

花了一天的时间，仔细核对了 `helper` 代码，打了许多次 `bundle` 与重启，做了许多搜索，甚至在 stackoverflow 上发了贴，结果还是一样。

却独独没有检查 `views` 中的代码，最后终于想起来看到却是这样：

`<%= highlight( markdown(@section.content), @query_string ) %>`

原来，为了想实现搜索时代码的高亮显示，写成了上面的样子。

改成：

`<%= markdown(@section.content) %>`

问题解决，。





### 如何在 ruby 写数据到文件中？

[A Ruby write to file example](https://alvinalexander.com/blog/post/ruby/how-write-text-to-file-ruby-example)

* 常规做法

```ruby
# open and write to a file with ruby
open('myfile.out', 'w') { |f|
  f.puts "Hello, world."
}
```

* 变化一: 使用 `do...end`

```ruby
open('myfile.out', 'w') do |f|
  f.puts "Hello, world."
end
```

* 变化二: 使用 `<<` 操作符

注意：后面的 `\n`，而这个在使用 `puts` 时则会自动加上。

```ruby
open('myfile.out', 'w') do |f|
  f << "Hello, world.\n"
end
```

* 写多行

```ruby
open('myfile.out', 'w') { |f|
  f << "Four score\n"
  f << "and seven\n"
  f << "years ago\n"
}
```



### 如何在 ruby 中生成给定范围的整数数组？

```
> [*1..10]
=> [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

> (1..10).to_a
=> [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```


### jQuery 如何获取当前页面 url？

`$(location).attr("href");`




### 如何实现页面滚动到某个特定位置，执行某些操作？

**目标:**

当页面向下滚动到某个位置，`学习中` 变成 `已学完`

**思路：**
先用 jQuery 实现效果，再实现后台操作数据库

```js
$(document).ready(function() {

  var surf_height, page_scroll_height, el_offset_top, offset;
  var learn_state

  learn_state = $(".learn-state");
  surf_height = $(window).height();  // 浏览器窗口高度
  el_offset_top = learn_state.offset().top;  // 元素距 document 顶顶端高度
  offset = 80;  //  偏移量

  $(window).scroll(function() {
    page_scroll_height = $(this).scrollTop();        // 页面滚动高度
    var undo_text = $(".undo-text");
    if(page_scroll_height + offset > el_offset_top) {
      if(undo_text.length == 0) {
        $(".learn-state-btn svg").remove();
        $(".learn-state-btn").append('<i class="fas fa-check-circle finished"></i>');
        $(".learn-state-text").text('已学完');
        $(".undo").append('<span class="undo-text">撤销</span>');
        $(".undo").append('<i class="fas fa-undo"></i>');
      }
    }
  });
});
```


### jQuery 如何？

* 如何获取元素?

可使用 `find()`


* 如何获取元素的 class?

`var my_class = $(".xxx").attr("class");`





### heroku 常见命令？

将代码推送到 heroku 上出现错误提示：

`Devise.secret_key was not set. Please add the following to your Devise initializer:`

1. 在 `config/initializers/devise.rb` 中添加：

```ruby
config.secret_key = ENV['DEVISE_SECRET_KEY'] if Rails.env.production?
```

2. 生成 secret_key

`rake secret`

3. 设置 config vars

在 heroku 上设置 ［config vars](https://devcenter.heroku.com/articles/config-vars#managing-config-vars)


* 查看当前 config vars

```
$ heroku config
$ heroku config:get GITHUB_USERNAME
```

* 设置 config vars

```
$ heroku config:set GITHUB_USERNAME=joesmith
$ heroku config:set S3_KEY=8N029N81 S3_SECRET=9s83109d3+583493190
```

* 移除 config vars

```
$ heroku config:unset GITHUB_USERNAME
```



### heroku 常见命令？

登陆 heroku
`heroku login`

 创建新的 app，免费用户最多可创建 5 个 apps，可删除再建新的
`heroku create`

上传到 heroku

现在我们最新的代码在 `01-done` 这个分支上
`git push heroku 01-done:master`

在 heroku 上执行创建数据库等操作

`heroku run rake db:migrate`


### Devise 如何禁用用户注册？

* controller:

```ruby
class RegistrationsController < Devise::RegistrationsController
  def new
    flash[:info] = 'Registrations are not open yet, but please check back soon'
    redirect_to root_path
  end

  def create
    flash[:info] = 'Registrations are not open yet, but please check back soon'
    redirect_to root_path
  end
end
```

* routes.rb

```ruby
if Rails.env.production?
    devise_for :users, :controllers => { :registrations => "registrations" }
  else
    devise_for :users
  end
```


* [disabling Devise registration for production environment only](https://stackoverflow.com/a/8291318/4455426)



### Rails 5 jQuery 不起作用？

这是因为在 Rails 5 中已经不再把 `jQuery` 作为默认的 javascript 框架支持了。

如果还想使用，则在 Gemfile 中添加

`gem 'jquery-rails'`

之后再 `bundle install`

再将下面代码添加到

*app/assets/javascripts/application.js*

```js
//= require jquery
//= require jquery_ujs
```



* [Rails 5.1 has dropped dependency on jQuery from the default stack](https://blog.bigbinary.com/2017/06/20/rails-5-1-has-dropped-dependency-on-jquery-from-the-default-stack.html)



### 如何用 CSS 或 Html 来表示键盘上的具体键？

可以定义 `<kbd>` 的样式

```CSS
kbd {
    display: inline-block;
    margin: 0 .1em;
    padding: .1em .6em;
    font-family: Arial,"Helvetica Neue",Helvetica,sans-serif;
    font-size: 11px;
    line-height: 1.4;
    color: #242729;
    text-shadow: 0 1px 0 #FFF;
    background-color: #e1e3e5;
    border: 1px solid #adb3b9;
    border-radius: 3px;
    box-shadow: 0 1px 0 rgba(12,13,14,0.2),0 0 0 2px #FFF inset;
    white-space: nowrap
}
```

样式来自 [iTerm 2: How to set keyboard shortcuts to jump to beginning/end of line?](https://stackoverflow.com/q/6205157/4455426)

则使用时就可以用：

```html
<kbd>Ctrl</kbd>
<kbd>left</kbd>
<kbd>←</kbd>
<kbd>→</kbd>
```


### 如何绑定左键与右键到链接上？

这个问题直观上来看是：

单击左键（或右键）实现跳转到上一页（或下一页）

可转换思路：

单击左键（或右键）实现上一页（或下一页）的单击。

这样问题就简化了。

* 先来看如何用 jQuery 检测键被按下去：

```js
$(document).keydown(function(e) {
  console.log(e.keyCode);
});
```

* 添加链接的类

如上一页添加 `previous-page`
如下一页添加 `next-page`

* js 代码

```js
$(document).keydown(function(e) {
  if(e.keyCode == 37) {  // Left
    $('a.previous-page')[0].click();
  } else if(e.keyCode == 39) { // Right
    $('a.next-page')[0].click();
  }
});
```
* 其他有用的信息

[jQuery Keypress Arrow Keys](https://stackoverflow.com/a/19347349/4455426)


### Rails 5 对搜索内容进行高亮显示？

可以使用 `highlight` 这个 helper 方法

`<%= highlight( content, params[:search] ) %>`


* `highlight` 负责高亮（其实是添加 `<mark>` 标记)


使用：

```
<%= highlight(section.title, @query_string) %>
```

* 另一个方法　`excerpt`   负责做摘录

* [ActionView::Helpers::TextHelper](http://api.rubyonrails.org/classes/ActionView/Helpers/TextHelper.html)
* [截取部分高光](https://stackoverflow.com/a/29772040/4455426)


### Rails 5 如何启用搜索？

#### 使用 `ransack` 做搜索

* [ransack](https://github.com/activerecord-hackery/ransack)

**实现目标：**

在 navbar 的搜索栏中搜索 `section` 的 `title` 与 `content`　中内容。


* 1. 安装

在 Gemfile 中添加

`gem 'ransack'`

再执行：`bundle install`

重启 rails

* 2. Routes

*config/routes.rb*

```ruby
resources :sections do
  collection do
    get  :search
  end
end
```

* 3. View 搜索框

使用 `form_tag`

```html
<%= form_tag search_course_sections_path(@course), method: :get do %>
  <div class="field">
    <p class="control has-icons-left">
      <%= text_field_tag(:q, "", placeholder: '搜索本课程教材', class: 'input is-small') %>
      <span class="icon is-small is-left">
        <i class="fas fa-search"></i>
      </span>
    </p>
  </div>
<% end %>
```

* 4. View 搜索结果显示

*search.html.erb*

```html

<% if @sections.present? %>
  <% @sections.each do |section| %>
    <div class="columns">
      <div class="column is-6">
        <%= link_to course_section_path(@course, section) do %>
          <i class="fas fa-circle session-icon"></i>
          <%= section.title %>
        <% end %>
      </div>
    </div>
  <% end %>
<% else %>
  <div class="box notification is-primary">
    <p class="title is-size-6">
      <%= t("hint.no_search_result") %>
    </p>
  </div>
<% end %>
```





* 2. Controller

```ruby
class SectionsController < ApplicationController
  before_action :authenticate_user!
  before_action :validate_search_key, only: [:search]

  layout "course", only: [:search, :show]

  def search
    @course = Course.find(params[:course_id])

    if @query_string.present?
      search_result = Section.ransack(@search_criteria).result(distinct: true)
      @sections = search_result
    end
  end

  private

  def validate_search_key
    @query_string = params[:q].gsub(/\\|\'|\/|\?/, "") if params[:q].present?
    @search_criteria = search_criteria(@query_string)
  end

  def search_criteria(query_string)
    { title_or_content_cont: query_string }
  end
end
```


### Rails 中为什么 `1 / 2 = 0`？

```ruby
> 1 / 2
0
> 8 / 5
1
```

如果想返回带小数的，则要将被除数或除数至少一个转变为浮点数

```ruby
> 1.0 / 2
0.5
> 8 / 5.0
1.6

9.to_f / 5  #=> 1.8
9 / 5.to_f  #=> 1.8
```


[to_f mode](https://stackoverflow.com/a/5503586/4455426)

或使用 `[Numeric#fdiv](http://www.ruby-doc.org/core-2.0.0/Numeric.html#method-i-fdiv)`

```ruby
9.fdiv(5)  #=> 1.8
```


### Rails 如何从 console 中运行 helper 中的方法？

* 使用 `helper` 对象

```
$ ./script/console
>> helper.number_to_currency('123.45')
=> "R$ 123,45"
```

If you want to use a helper that's not included by default (say, because you removed helper :all from ApplicationController), just include the helper.

如果打算使用一个并没包括在默认 helper 里的方法（比如，从 `ApplicationController` 中移除了 `helper :all`)，那么只需要 `include xxx`

```
>> include BogusHelper
>> helper.bogus
=> "bogus output"
```

### Rails 5 设置 flash messages？

环境为 Rails 5，CSS 框架使用 Bulma

1. `_flashes.html.erb`

`touch app/views/common/_flashes.html.erb`

```
<% if flash.any? %>
  <% user_facing_flashes.each do |key, value| %>
    <section class="hero notification-hero">
      <div class="hero-body">
        <div class="notification is-<%= flash_class(key) %>">
          <button class="delete"></button>
          <%= value %>
        </div>
      </div>
    </section>
  <% end %>
<% end %>
```

`notification-hero` 为自定义的类，用于 javascript 来选择元素。

2. javascript

```javascript
'use strict';

document.addEventListener('DOMContentLoaded', function () {
  // Notification

  var rootEl = document.documentElement;
  var $notifications = getAll('.notification-hero');
  var $notificationCloses = getAll('.notification-hero .notification .delete, .notification .delete');

  if ($notificationCloses.length > 0) {
    $notificationCloses.forEach(function ($el) {
      $el.addEventListener('click', function () {
        closeNotifications();
      });
    });
  }

  function closeNotifications() {
    $notifications.forEach(function ($el) {
      $el.remove();
    });
  }
});
```

3. layout: *application.html.erb*

```
<%= render "common/flashes" %>
```

4. controller

```

redirect_to admin_articles_path, notice: t("messages.admin.articles.create")

# or
flash[:warning] = t("messages.admin.articles.destroy")
redirect_to admin_articles_path


```


### Rails 5 设置默认本地语言 locales？

1. 创建 locale.rb 文件

*config/initializers/locale.rb*

```ruby

# Where the I18n library should search for translation files
I18n.load_path += Dir[Rails.root.join('lib', 'locale', '*.{rb,yml}')]

# Whitelist locales available for the application
I18n.available_locales = ['zh-CN', 'en', 'zh-TW']

# Set default locale to something other than :en
# I18n.default_locale = :en
I18n.default_locale = 'zh-CN'

```

**注意：记得重启。**

2. 创建语言文件

*config/locales/zh-CN.yml*

**注意：yml 文件是严格缩进的。**

```ruby

"zh-CN":
  title:
    index: "Hello world"

```

3. 在 view 文件中使用

*xxx.html.erb*

**注意：yml 文件是严格缩进的。**

```

<%= t("title.index") %>

```




### Rails 5 如何修改每页的标题？

1. 在 layout 中

```
<head>
  <title><%= @title %></title>
</head>
```

2. 在页面第一行

`<% @title = "Some Text" %>`

提示：可以创建本地语言文件，这样方便集中修改。


### Rails 5 如何修改项目名称？

* 修改模块名称

*config/application.rb*

```ruby
# config/application.rb
module xxx  # <-- here
```

* 修改 session 名称（可选）

```ruby
# config/initializers/session_store.rb
Rails.application.config.session_store :cookie_store, key: '_xxx_session'  # <-- here
```

* 修改 view title 名称（可选）

*app/views/layouts/application.html.erb*

```
<title>...</title>  # <-- here
```

* 修改数据库名称

*config/database.ym*


* 修改结束之后，别忘记重启 sever








### Migration: 如何删除 column?

`remove_column :courses, :is_hidden`


### 如何格式化时间？

时间显示为：`2018-06-30 06:49:32 +0800`
如何去掉后面的时区显示？

### CSS 鼠标样式禁止

`cursor:not-allowed;`

### 能否给 <a> 添加 `alt` 属性？

不可以，搜索官方文件

1. 搜索官方文档，`w3 html5`：https://www.w3.org/html/。（需要了解的是 w3.org 是官方组织）
2. 到官方文档：https://www.w3.org/TR/html5/
3. 搜索 `a element`：https://www.w3.org/TR/html5/textlevel-semantics.html#the-a-element
4. 会发现， `alt` 属性并不被允许。

但，可以用 `title` 的方式来变通一下。





### Ruby 中判断元素是否在数组中？

搜索关键词：`ruby element array`

可使用 `includes?`

```ruby
>> ['Cat', 'Dog', 'Bird'].include? 'Dog'
=> true
```

### 统计代码运行效率？

```ruby
require 'benchmark'

a1 = []; a2 = []
[a1, a2].each do |a|
  1000000.times { a << rand(999999) }
end

puts "Merge with pipe:"
puts Benchmark.measure { a1 | a2 }

puts "Merge with concat and uniq:"
puts Benchmark.measure { (a1 + a2).uniq }

puts "Concat only:"
puts Benchmark.measure { a1 + a2 }

puts "Uniq only:"
b = a1 + a2
puts Benchmark.measure { b.uniq }


Merge with pipe:
  1.000000   0.030000   1.030000 (  1.020562)
Merge with concat and uniq:
  1.070000   0.000000   1.070000 (  1.071448)
Concat only:
  0.010000   0.000000   0.010000 (  0.005888)
Uniq only:
  0.980000   0.000000   0.980000 (  0.981700)

```

### ruby 中如何将一个数组添加到另一个数组中？

搜索关键词：`ruby array merge`

可以用

```ruby
> [1, 2, 3, 4].concat([3, 4, 5, 6])
=> [1, 2, 3, 4, 3, 4, 5, 6]
> [1, 2, 3, 4].concat([3, 4, 5, 6]).uniq
=> [1, 2, 3, 4, 5, 6]
```

还可以试试

```ruby
a = [1,2,3]
[4,5,6].each {|i| a << i }
```



### 链接是否可以 disabled？

* 链接没有 disabled 这个属性

可以在 [w3c: the-a-element](https://dev.w3.org/html5/html-author/#the-a-element) 中看到

链接不像 button 可以 `disabled`。

* 链接可以通过 javascript 来禁用

```js
$('a').click(function(event){
   event.preventDefault();
});
```

* 在 Rails 中可用 `link_to_if`

这是一个变通的方式就是使用 `link_to_if`

它可以根据条件来显示链接，或纯文本


[link_to_if](http://api.rubyonrails.org/classes/ActionView/Helpers/UrlHelper.html#method-i-link_to_if)









### `course_ids` 第一个为空值？

`"course_ids"=>["", "2", "4", "8", "9", "13"]`



### `name.parameterize` 做什么用？

```ruby
def set_slug
  self.slug = self.name.parameterize
end

def self.for_cloud

  where(show_in_cloud: true).order("RAND()").limit(20)
end

```

https://github.com/BigDilettante/jazzdrummersresource/blob/e360802900f2957466f7876fefa467003fcb41ed/app/models/tag.rb


----
****

## 已解决

### 批量更新记录


* 根据条件，使用 `update_all`

```ruby

User.where(name: 'test1').update_all(name: 'test')

# Update all customers with the given attributes
Customer.update_all wants_email: true

# Update all books with 'Rails' in their title
Book.where('title LIKE ?', '%Rails%').update_all(author: 'David')

# Update all books that match conditions, but limit it to 5 ordered by date
Book.where('title LIKE ?', '%Rails%').order(:created_at).limit(5).update_all(author: 'David')

Model.where(:foo => 'bar').where(:attr => 1).update_all("author = 'David'")

```

* 若根据记录的 ids 来更新

```ruby
User.where(:id => ids).each do |usr|
 usr.update_attribute(:fruit_name, fruit_names[ids.index(usr.id)])
end
```


### 如何更改 Git branch 的名字？

这里要区分情况，你是只修改改本地的分支名字，还是要本地与远端一起改掉？

* 只修改本地分支，不改远端

如果在当前分支里面：

`git branch -m <newname>`

如果不在当前分支里面，或者想修改任意分支：

`git branch -m <oldname> <newname>`

这里的 `-m` 表示 `--move`。

[How do I rename a local Git branch?](https://stackoverflow.com/a/6591218/4455426)

* 本地与远端都修改


```ruby
git branch -m master master-old
git push remote :master         # delete master
git push remote master-old      # create master-old on remote

git checkout -b master some-ref # create a new local master
git push remote master          # create master on remote
```


[Rename master branch for both local and remote Git repositories](https://stackoverflow.com/a/1527004/4455426)



### Git: 如何撤消所有改动，恢复到上一次的 Commit？

`git reset --hard HEAD`

https://stackoverflow.com/a/12049323/4455426

[How to revert Git repository to a previous commit?](https://stackoverflow.com/questions/4114095/how-to-revert-git-repository-to-a-previous-commit)


### 如何验证字段的唯一性？

```ruby
class Account < ApplicationRecord
  validates :email, uniqueness: true
end
```

### 如何用 jquery-validation-rails 验证字段唯一性

参考：

* [check_username](https://github.com/sul-dlss/revs/search?q=check_username&unscoped_q=check_username)
* [How to Integrate Client Side Validations to Check for Uniqueness in Rails](http://www.dailysmarty.com/posts/how-to-integrate-client-side-validation-to-check-for-uniqueness-in-rails)

* [Is there any way to validate uniqueness of email on JS in a ruby on rails app?](https://stackoverflow.com/questions/38597357/is-there-any-way-to-validate-uniqueness-of-email-on-js-in-a-ruby-on-rails-app)


### jQuery 的 blur 事件

> 当元素失去焦点时发生 blur 事件。

> `blur()` 函数触发 blur 事件，或者如果设置了 function 参数，该函数也可规定当发生 blur 事件时执行的代码。


### case 的使用

```ruby
def set_active_menu
  @current = case controller_name
             when 'pages'
               ['/wiki']
             else
               ["/#{controller_name}"]
             end
end
```


### ruby 正则匹配 `?`

使用 `\?`

例如：

```
<%= active_link_to courses_path(tab: 'one'),
      wrap_tag: :li, class_active: 'is-active',
      active: /^\/courses\?tab=one$/ do %>
```


### `has_and_belongs_to_many` vs `has_many :through` 有什么区别，及什么时候使用谁？

* `has_and_belongs_to_many`

可以给你一个引用两个 models 的简单查询表。

例如：故事可以属于多个类别，类别可以有许多个故事。

```
Categories_Stories Table
story_id | category_id
```

* `has_many :through`

在两个关联的 model 之外给你第三个 model，在这个 model 当中存储着不属于两个原始 model 的各种其它信息。

例如：用户可以订阅许多本杂志，而杂志可以有许多个订阅者。

这样，我们可以在中间多一个 `subscription`  model，这就给了我们一个与上面例子类似的查询表，但却可以有一些额外属性。



```
Subscriptions Table
person_id | magazine_id | subscription_type | subscription_length | subscription_date
```

* 如何选择呢？

Rails 提供了两个不同的方法来声明 **多对多** 关系。

简单的方法是使用 `has_and_belongs_to_many`，这允许你直接建立关联:

```ruby
class Category < ApplicationRecord
  has_and_belongs_to_many :stories
end

class Story < ApplicationRecord
  has_and_belongs_to_many :categories
end
```

第二种方法是使用 `has_many :through` 来声明 **多对多** 关联。这使得关联并非直接发生，而是通过一个 `join` 模型。

```ruby
class Assembly < ApplicationRecord
  has_many :manifests
  has_many :parts, through: :manifests
end

class Manifest < ApplicationRecord
  belongs_to :assembly
  belongs_to :part
end

class Part < ApplicationRecord
  has_many :manifests
  has_many :assemblies, through: :manifests
end
```

最简单的使用法则是：如果你需要将处理的关联模型当作一个单独的实体，那么，你就需要创建一个 `has_many :through` 关系。如果你不需要关联模型做任何事，那么，就直接用 `has_and_belongs_to_many`（当然，你要记得 **创建中间表** ）。

创建中间表：

`rails g migration create_join_table_category_story category:index story:index`

此外，如果你需要验证、回访或额外属性，那么，你需要的也是 `has_many :through`。

* [Choosing Between has_many :through and has_and_belongs_to_many](http://guides.rubyonrails.org/association_basics.html#choosing-between-has-many-through-and-has-and-belongs-to-many)
* [has_and_belongs_to_many vs has_many through](https://stackoverflow.com/questions/2780798/has-and-belongs-to-many-vs-has-many-through)
* [top answers](https://stackoverflow.com/a/2781049/4455426)



### 判断 record 是否存在，若不存在，则创建？


有两种方法，`find_or_create_by` 或 `first_or_create`：

```ruby
User.find_or_create_by(first_name: "John", last_name: "Smith")
User.where(first_name: "John", last_name: "Smith").first_or_create
```

两种方法都支持 block

```ruby
User.where(last_name: "Smith").first_or_create do |user|
  user.first_name = "John"
end

User.find_or_create_by(last_name: "Smith") do |user|
  user.first_name = "John"
end
```

还都有对应的加上感叹号的方法，若在创建记录时验证不通过，则会抛出异常

`find_or_create_by!` 与 `first_or_create_by! `

* [Create record if it does not exist in ActiveRecord](https://chodounsky.net/2015/09/14/create-record-if-it-does-not-exist-in-activerecord/)


### 根据关联关系，添加新的记录

例如，course 可以有许多 tag，tag 也可以包括多个 courses。

可使用 `<<` 与 `>>` 符号来新增记录与删除记录。

```ruby
courses.each do |course|
  course.tags << tag
end
```

* [Add record to a has_and_belongs_to_many relationship](https://stackoverflow.com/questions/2740070/add-record-to-a-has-and-belongs-to-many-relationship)


### 如何判断记录属于某一个集合，记录存在？

* 可以使用 `exists?` 来判断给定 `id` 或条件是否存在于表、集合中。

因为并不返回查询的记录，所以效率较高

```ruby
Person.exists?(5)
Person.exists?('5')
Person.exists?(['name LIKE ?', "%#{query}%"])
Person.exists?(id: [1, 4, 8])
Person.exists?(name: 'David')
Person.exists?(false)
Person.exists?
Person.where(name: 'Spartacus', rating: 4).exists?
```
[exists?](http://api.rubyonrails.org/classes/ActiveRecord/FinderMethods.html#method-i-exists-3F)

```ruby
if Business.exists?(user_id: current_user.id)
  # same as Business.where(user_id: current_user.id).exists?
  # ...
else
  # ...
end
```

[Business.exists?](https://stackoverflow.com/a/16682728/4455426)

* 使用 `.present?`（或 `.blank?`，与 `.present?` 相反）

```ruby
if Business.where(:user_id => current_user.id).present?
  # less efficiant than using .exists? (see generated SQL for .exists? vs .present?)
else
  # ...
end
```

* 如果要查询到的记录还需要使用

```ruby
if business = Business.where(:user_id => current_user.id).first
  business.do_some_stuff
else
  # do something else
end

# or
business = Business.where(user_id: current_user.id).first
if business
  # ...
else
  # ...
end
```
