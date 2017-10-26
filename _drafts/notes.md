---
layout: post
comments: true
title:  "Rails Notes"
categories: Rails
tags: rails-error
date:
---

* content
{:toc}

`no implicit conversion of nil into String`


## Model 中定义数组及用法

### 定义

*app/models/event.rb*

```ruby
class Event < ApplicationRecord

  STATUS = ["draft", "public", "private"]
  validates_inclusion_of :status, :in => STATUS

end
```

### 用法

1. 使用 Rails 内置表单

*app/views/events/_form.html.erb*

```
+ <div class="form-group">
+ <%= f.label :status %>
+ + <%= f.select :status, Event::STATUS.map{ |s| [t(s, :scope => "event.status"), s] }, {}, :class => "form-control" %>
+ </div>
```

2. 使用 `simple_form`

*app/views/events/_form.html.erb*

```
<div class="group">
  <%= f.input :status, label: "状态", collection: Event::STATUS, class: "form-control" %>
</div>
```


## 国际化语言支持

### 编辑或创建 locale.rb，设置默认本地化语言

*config/initializers/locale.rb*
```ruby
# config/initializers/locale.rb

Rails.application.config.i18n.default_locale = "zh-CN"
```

### 编辑默认本地化语言文档

*config/locales/zh-CN.yml*
这个文件是严格缩进的，所以一定要注意逐级缩进两个字符

```
zh-CN:
  line:
    trans:
      bus: 大巴
      train: 火车
      ship: 轮船
      plane: 飞机
```

### 使用

1. 直接调用

```
<%= t('line.trans.bus') %>
<%= t('line.trans.ship') %>
```

2. 如果数据来源为数组需循环获得

*app/models/line.rb*

```ruby
class Line < ApplicationRecord
  TRANS = ["bus", "train", "ship", "plane"]
  validates_inclusion_of :trans, in: TRANS
end
```

*/app/views/admin/lines/_form.html.erb*

```
<%= f.input :trans, label: "交通工具", collection: Line::TRANS.map{ |s| [t(s, :scope => "line.trans"), s ] }, class: "form-control" %>
```


## 将字段为数组类型的内容输出

### 存储内容为数组

1. model 中定义一个常量数组

*app/models/line.rb*
```ruby
class Line < ApplicationRecord
  TRANSPORATATIONS = ["bus", "train", "ship", "plane"]
  # validates_inclusion_of :transportations, in: TRANSPORATATIONS
end
```

2. controller 中添加字段到白名单中

*app/controllers/admin/lines_controller.rb*
```ruby
def line_params
  params.require(:line).permit(:title, :description, :days, :transportations => [])
end
```

3. view 中用 `check_boxes` 来实现多选

*app/views/admin/lines/_form.html.erb*
```
<div class="group">
  <%= f.input :transportations, label: "交通工具", as: :check_boxes, collection: Line::TRANSPORATATIONS.map{ |s| [t(s, :scope => "line.transportations"), s ] },
          include_blank: false, include_hidden: false %>
</div>
```

> 注意：在最后添加了 `include_blank: false, include_hidden: false`
>     这是因为参数中会自动多一个空白的值，如 `:transportations => ["", "bus"]`

### 将数组输出

上面定义的数组存入到数据库中其实为 `:text` 类型，也即字符串
要让它比较好的输出，则需要先进行转化：

**text -> array --> output**

1. 方法一：直接在 views 中写

*app/views/admin/lines/index.html.erb*
转化成用 `，`分隔的字符串
```
<td>
  <%= eval(line.transportations).map { |f| t(f, scope: "line.transportations") }.join('，') %>
</td>

```

2. 方法二：写一个 helper

*app/helpers/admin/lines_helper.rb*
```ruby
module Admin::LinesHelper
  def render_transportations(trans)
    eval(trans).map { |f| t(f, scope: "line.transportations") }.join('，')
  end
end
```

而后在 views 中：

**
```
<%= render_transportations(line.transportations) %>
```


## 粗心导致的古怪错误

定义了一个 `boolean` 的字段，但测试来测试去
只有当值为　`true` 时可以提交成功，设为　`false` 则报错

检查了好久才发现原因所在：是 model 中加了验证

```ruby
class Registration < ApplicationRecord
  validates :batch_id, presence: true
  validates :is_share, presence: true
end
```

就是这句，`validates :is_share, presence: true` 限定了该字段只能为　`true`


## 如何在 `link_to` 中传递参数


`<%= link_to t("registration.new_registration"), new_registration_path(:selected_line_id => line.id), class: "btn btn-primary btn-xs" %>`

这样就可以在　Controller 中使用了

`@selected_line = Line.find(params[:selected_line_id])`

## 用 javascripts 选择单选按钮(radio_buttons)

接前面步骤，可以在 Controller 中接收到 url 传递过来的 `selected_line_id`
这样就可以在下一页的表单中通过　javascripts 使用这个值来选定与之关联的 radio_button

```
<script>
  $('#registration_line_id_<%= @selected_line.id %>').prop('checked', true)
</script>
```

在上面的　js 代码中，我们使用了　`<%= @selected_line.id %>` 来获取对应的 `line_id`，并依此设置对应的 radio_button 的 `checked` 属性为　`true` 。


* [How do I pass a Ruby variable to JavaScript?](https://www.quora.com/How-do-I-pass-a-Ruby-variable-to-JavaScript)
* [Jquery set radio button checked, using id and class selectors](https://stackoverflow.com/questions/15949324/jquery-set-radio-button-checked-using-id-and-class-selectors)



## `simple_form` 实现 boolean 类型的字段的单选按钮

```
<div class="group">
  <%= f.input :is_room_share, label: "是否包间", as: :radio_buttons,
          collection: [["合住", true], ["包间(与家人合住算包间，与同事合住不算包间)", false]],
          :include_blank => false %>
</div>
```

* `as: :radio_buttons` 设置为　radio 按钮
* `collection: [["合住", true], ["包间(与家人合住算包间，与同事合住不算包间)", false]]` 设置集合
* `["合住", true]` : 前面为 Text ，后面为　Value



## 如何写单个的　`radio_button`

`<%= radio_button 'registration', 'is_room_share', false, {:class => "room_share room_share_no"} %>   包间`

文档为： `radio_button(object_name, method, tag_value, options = {}) public`
> 注意：

* `object_name` : `registration`
* `method` : `is_room_share`
* `tag_value` : `false`

上例生成的 HTML 如下：
```
<input class="room_share room_share_no" type="radio" value="false" checked="checked" name="registration[is_room_share]" id="registration_is_room_share_false">
```


## 用 javascripts 实现：单击单选按钮显示对象

要实现这个功能，为了方便起见，最好给　`radio_button` 设置额外的 class
因为后面要将显示隐藏这个　`toggle` 与 `class` （样式）关联起来

*View*

```html
<%= radio_button 'registration', 'is_room_share', false, {:class => "room_share room_share_no"} %>   包间

<div class="group" id="room_mate" style="display:none">
  <%= f.input :room_mate, label: "合住人" %>
</div>
```

*JavaScript*

将事件句柄绑定到　`change` 事件上

```javascript
<script>
  // room_share
  $(".room_share").on("change", function(){
    $("#room_mate").toggle($(this).hasClass("room_share_yes"));
  })
</script>
```



[Rails show fields when radio button is checked](https://stackoverflow.com/questions/12459574/rails-show-fields-when-radio-button-is-checked)


## 如何对身份证号及手机进行验证





``` ruby
validates :name, :phonenumber, :cnid, :gender, presence: true

PHONENUMBER_FORMAT=/\d{11}/
validates :phonenumber,
          :format => {with: PHONENUMBER_FORMAT,
            message: "手机号必须为 11 位数字" }

CNID_FORMAT=/\d{18}/
validates :cnid,
          :format => {with: CNID_FORMAT,
            message: "身份证号必须为 18 位数字" }

```


validates_format_of :phone_number, :with => /\d{7}/
validates :phone_number, :presence => true


validates :street, :city, :zipcode, :country, :phonenumber, :user_id, presence: true
validates :phonenumber, length: { minimum: 9, maximum: 9 }, numericality: { only_integer: true }
validates_format_of :zipcode, with: /\A\d{2}-\d{3}\z/


## 一次提交多个表单




## 使用 `redirect_to` 从一个 controller 到另一个 controller


You can use url_for to get the URL for a controller and action and then use redirect_to to go to that URL.

`redirect_to url_for(:controller => :controller_name, :action => :action_name)`

`redirect_to :controller => 'controllername', :action => 'actionname' `

`redirect_to :controller => "registrations", :action => "new", :selected_line_id => @selected_line_id`

[Passing parameters in rails redirect_to](https://stackoverflow.com/questions/1430247/passing-parameters-in-rails-redirect-to)


## 多个路由（path）指向同一个 controller 的 action



想实现多个路由指向同一个 action ，而又想让这个 action 处理好之后，根据route 的来路不同做不同的处理，特别是最后的 `redirect_to` 跳转到不同的地方。

如：

这个是从 line 页面跳转到用户资料编辑
`<%= link_to "Edit", edit_profile_path(:line => true) %>`

这个直接从 `_navbar.html.erb` 那个下拉菜单中跳转到用户资料编辑页面
`<%= link_to "Edit", edit_profile_path %>`

现在能想到的方法就是在加一个参数，而后在 profile 的 `edit` 这个方法中判断

`if params[:line]`

这的确可以做一些不同的处理。

现在的问题是：

如何将参数从 `edit` 传递到之后 `update` 里面，然后在 `update` 里再做判断？

可以通过在表单页，加上 `hidden_field_tag`

> 使用 hidden_field_tag
> `hidden_field_tag :selected_line_id, params[:selected_line_id]``



## 需要创建字段为空的对象，又需要保留验证

可以把 `create_profile` 替换为 `build_profile`

### 1. 要给用户创建 profile ，model 中加上了一些验证

```ruby
class Profile < ApplicationRecord
  belongs_to :user

  validates :name, :phonenumber, :cnid, :gender, presence: true

  PHONENUMBER_FORMAT=/\d{11}/
  validates :phonenumber,
            :format => {with: PHONENUMBER_FORMAT,
              message: "手机号必须为 11 位数字" }

  CNID_FORMAT=/\d{18}/
  validates :cnid,
            :format => {with: CNID_FORMAT,
              message: "身份证号必须为 18 位数字" }
end
```

### 2. Controller 中需要提获得用户的 profile

```ruby
class UserProfilesController < ApplicationController
  before_action :authenticate_user!
  before_action :find_user_profile

  def show
  end

  def edit
  end

  def update

    if @profile.update(profile_params)
      redirect_to profile_path
    else
      render :edit
    end
  end

  protected

  def find_user_profile
    @user = current_user
    @profile = @user.profile || @user.create_profile  # 有问题
  end

  def profile_params
    params.require(:profile).permit(:name, :phonenumber, :cnid, :gender)
  end


end
```

### 3. view 写法

`<%= link_to "编辑个人信息", edit_profile_path, class: "btn btn-success btn-sm" %>`

### 4. 报错

但上面执行之后就报错了

![]({{site.url}}/images/undefined_method_profiles_path.png)

后将 `@profile = @user.profile || @user.create_profile`
修改为： `@profile = @user.profile || @user.create_profile!` ，即在后面加上 `!`
这样就明显的报错了
提示，字段不能为空，电话、身份证号码位数不满足等

* 既要验证，又要可以创建对象呢？

答案是使用 `build`

即将上一句代码改为：

`@profile = @user.profile || @user.build_profile     # 无问题`

### 5. 解释

`@user.build_profile`
等同于 `Profile.new( :user => @user)`

`@user.create_profile`
等同于 `Profile.new( :user => @user)` 再加上 `@profile.save`


## 在 model 中如何让一字段只有当另一字段存在时才存在

如：只有当 `is_activated` 为 `true` 时，`activated_at` 才需要存在

`validates :activated_at, :presence, if: Proc.new { |a| a.is_activated? }`

* [http://api.rubyonrails.org/classes/ActiveRecord/Validations/ClassMethods.html](http://api.rubyonrails.org/classes/ActiveRecord/Validations/ClassMethods.html)




## `undefined method 'to_sym' for {:default=>false}:Hash`

这个原因是修改 column 时没有指定 type

具体用法：

`change_column(table_name, column_name, type, options = {})`

举例：
`change_column :users, :state, :string, :null => false, :default => 'active'`

`change_column :registrations, :is_room_share, :boolean, default: false`


## 如何在 dropdown 中添加一个分隔线 divider

`<li class="divider"></li>`


## 当页面加载好之后，当某个 radio 被选中时，设置某个对象的样式


### 1. Views

```
<div class="group">
  <%= f.label "是否包间" %><br>
  <%= radio_button 'registration', 'is_room_share', false, {:class => "room_share room_share_no"} %>   包间
  <%= radio_button 'registration', 'is_room_share', true, {:class => "room_share room_share_yes"} %>   合住
</div>

<div class="group" id="room_mate" style="display:none">
  <%= f.input :room_mate, label: "合住人" %>
</div>

```

### 2. jQuery

```javascript
<script>
  //
  //  开关
  $(".room_share").on("change", function(){
    $("#room_mate").toggle($(this).hasClass("room_share_yes"));
  })

  // 页面加载完毕后，如果合住，则显示“合住人”对象
  $(document).ready(function(){
    if ($(".room_share").prop("checked", true)){
      $("#room_mate").css('display', 'block');
    }
  })
</script>
```



## 如何将 form_for 转换成 simple_form

### form_for

```
<%= f.label "性别" %>
<%= f.select :gender, Profile::GENDERS,
        { prompt: " ", selected: @profile.gender },
        { class: "form-control string optional" } %>

```


### simple_form

```
<%= f.input :gender, :collection => Profile::GENDERS,
                    :label => "性别" , :include_blank => false %>
```




```
(byebug) Profile::GENDERS
[["女", "female"], ["男", "male"]]

(byebug) Profile.genders
{"male"=>0, "female"=>1}
```



## 将性别以 string 类型存储

*db/schema.rb*

```ruby
t.string   "gender"
```

*model/user.rb*

```ruby
GENDERS = %w(male female other)

validates :gender, :inclusion => { :in => GENDERS }
```

*Views*

```
<%= f.input :gender, as: :select, collection: User::GENDERS %>
```

## 以 integer 存储性别(未验证)

*db/schema.rb*

```ruby
t.integer   "gender"
```

*model/user.rb*

```ruby
enum gender: [:male, :female, :undisclosed]


```

```
<%= f.input :gender, as: :select, collection: User::GENDERS %>
```


## `resources` vs `resource`


## simple_form 中使用 collection

`    <%= f.input :batch_id, label: "疗养批次", as: :radio_buttons, collection: Batch.all.map{ |s| [ "#{s.title} (#{s.start_date.to_date} 至 #{s.return_date.to_date})", s.id ] } %>
`


`    <%= f.input :line_id, label: "疗养线路", as: :radio_buttons, collection: Line.all.map{ |s| [ "#{s.title} (#{s.days} 天)", s.id ] } %>
`


## 如何选中默认的 radio

只需设置 radio 的 `checked` 属性即可
当 `checked: true` 表示选中
当 `checked: false` 表示未选中

1. 指定选择的对象的 id
一开始指定　`Batch.first` 不行，后来加上　`id` 才可以。

```
<%= f.input :batch_id, label: "疗养批次",
            as: :radio_buttons,
            collection: Batch.all.map{ |s| [ "#{s.title} (#{s.start_date.to_date} 至 #{s.return_date.to_date})", s.id ] },
            checked: Batch.first.id %>
```

2. 例如：

```
<%= simple_form_for @registration do |f| %>


<%= f.input :is_room_share, as: :radio_buttons,
            :collection => [[true, '合住'], [false, '包间']],
            label: "是否包间",
            :label_method => :last,
            :value_method => :first,
            :item_wrapper_class => 'inline',
            :checked => @registration.is_room_share %>
<% end %>            

```

其中的 `:checked => @registration.is_room_share`
即可根据记录的具体的值对 radio 进行对应的选中操作。




## 如何给 simple_form 中的 input 添加 class

### 给 input 的父元素（Wrappper) 添加 class

`item_wrapper_class`

* 给某一个 input 添加 class
`= f.input :email, :input_html => { :class => 'foo' }`

* 给所有的 inputs 添加样式

`simple_form_for(@user, :defaults => { :input_html => { :class => "foo" } })`

* 还可以自己创建

```
# app/inputs/foo_input.rb
class FooInput < SimpleForm::Inputs::StringInput
  def input_html_classes
    super.push('foo')
  end
end

// in your view:
= f.input :email, :as => :foo
```

[https://stackoverflow.com/questions/9371490/how-to-add-a-class-to-the-input-component-in-a-wrapper-in-simple-form-2](https://stackoverflow.com/questions/9371490/how-to-add-a-class-to-the-input-component-in-a-wrapper-in-simple-form-2)


### 如何自定义 simple_form_for 的 input (?)

有时需要给不同的　radio 添加不同的样式

如这里的提问
[Rails Simple Form Radio_buttons: How to add different class for diffent radio input](https://stackoverflow.com/questions/44741659/rails-simple-form-radio-buttons-how-to-add-different-class-for-diffent-radio-in/44742123?noredirect=1#comment76465724_44742123)

**创建　view 的 Helper**

```ruby
def gift_radio_class(val)
  val ? "gift_pack_yes" : " gift_pack_no"
end
```

**erb**

```
<%=f.collection_radio_buttons(:gift_val, [[true, 'Gift Pack'], [false, 'Original Pack']], :first,:second) do |b|%>
<%b.label { b.radio_button(class: gift_radio_class(b.value)) + b.text }%>
  <%end %>
```

* Ref: [http://www.rubydoc.info/github/plataformatec/simple_form/SimpleForm/FormBuilder:collection_radio_buttons](http://www.rubydoc.info/github/plataformatec/simple_form/SimpleForm/FormBuilder:collection_radio_buttons)

## Ruby 中类的继承

### 类的继承简单用法

```ruby
class A
end

class B < A
end
```

此处， 类 B 就继承了类 A 。

* 获取父类名称

```ruby
B.superclass # => A
B.superclass.name # => "A"
```



## 如何动态的根据当前页面，给 navbar 中的 li 添加 `active` 样式

### 先要了解 `current_page?` 的用法

* [current_page?](https://apidock.com/rails/ActionView/Helpers/UrlHelper/current_page%3f)

注意几种用法：

* `current_page?(controller: 'shop', action: 'checkout', order: 'asc')`
* `current_page?(action: 'checkout')`
* `current_page?(product_path(@product))`
* `current_page?(products_path)`

### 直接用 current_page?

**VIEWS**

`<li class="<%= 'active' if current_page?(lines_path) %>"><%= link_to "Lines", lines_path %></li>`

### 定义一个 helper

1. application_helper

*app/helpers/application_helper.rb*

```ruby

def active_class(link_path)
  current_page?(link_path) ? "active" : ""
end
```

2. views

```
<li class="<%= active_class(lines_path) %>"><%= link_to "Lines", lines_path %></li>
```



## `git push` 提示输入用户名和密码，且密码有锁

关于 SSH 可以参考 [Connecting to GitHub with SSH](https://help.github.com/articles/connecting-to-github-with-ssh/)

检查 ssh-key ，如果都无误的话，将 https 修改为 `git` 试一下

`git remote set-url origin git@github.com:username/REPO.git`



## radio 按钮设置了　`checked="checked"` ，但不起作用

原因是有其它的　JavaScript 的影响

解决办法：
直接在js 执行代码前添加　`event.preventDefault();`

```js
<script>

## 如何在 seeds.rb 中添加日期时间的值

```ruby
Meeting.create([
  {
    :title => "This meeting parses fine",
    :startDate => DateTime.strptime("09/01/2009 17:00", "%m/%d/%Y %H:%M"),
    :endDate => DateTime.strptime("09/01/2009 19:00", "%m/%d/%Y %H:%M")
  },
  {
    :title => "This meeting errors out",
    :startDate => DateTime.strptime("09/14/2009 8:00", "%m/%d/%Y %H:%M")
    :endDate => DateTime.strptime("09/14/2009 9:00", "%m/%d/%Y %H:%M")
  }])
```
  $(".room_share").on("change", function(event){
    event.preventDefault();
    $("#room_mate").toggle($(this).hasClass("room_share_yes"));
  })
</script>
```

## 如何在 seeds.rb 中添加日期时间的值

```ruby
Meeting.create([
  {
    :title => "This meeting parses fine",
    :startDate => DateTime.strptime("09/01/2009 17:00", "%m/%d/%Y %H:%M"),
    :endDate => DateTime.strptime("09/01/2009 19:00", "%m/%d/%Y %H:%M")
  },
  {
    :title => "This meeting errors out",
    :startDate => DateTime.strptime("09/14/2009 8:00", "%m/%d/%Y %H:%M")
    :endDate => DateTime.strptime("09/14/2009 9:00", "%m/%d/%Y %H:%M")
  }])
```

[https://stackoverflow.com/questions/5474164/rails-seeding-database-data-and-date-formats](https://stackoverflow.com/questions/5474164/rails-seeding-database-data-and-date-formats)



## 关于　`rake db:reset`

`rake db:reset` 会删除数据库，重新创建它，并加载当前的　schema 到里面。

也即: `rake db:reset` => `rake db:drop db:create db:schema:load db:seed`

但如果想运行所有的　migrations ，则需要： `rake db:migrate:reset`

即: `rake db:migrate:reset` => `rake db:drop db:create db:migrate`

[https://stackoverflow.com/questions/20464924/rails-migration-does-not-change-schema-rb](https://stackoverflow.com/questions/20464924/rails-migration-does-not-change-schema-rb)


## 如何处理继续类中单表继承的问题

### 起因

在一个 model 中添加一个字段，很简单的操作嘛

1. `rails g migration add_column_name_to_tables_name`
2. 编辑新生成的　`db\migrate\....rb`
3. 执行 `rake db:migrate`

也成功执行了，本以为这件事就这样结束了，可是没有想到，这个　model 是继承别的类的。
类的定义是这样的（举例）：　`class Person < Profile`

就导致了一个现象

`rake db:migrate` 成功执行，`db/schema.rb` 中也有新增的字段
但真实的　model 里却没有，进入　console 也没有。
对于当时还不了解　STI 的我来说，‘真是活见鬼了’。

### 原来是你

经过向　ihower 老师请教，才知道这是
因为继承的关系，因为继承，默认情况下　`Person` 与　`Profile` 是用的同一张表，也即　`profiles` 。

```ruby
class SuperClass < ActiveRecord::Base
  self.abstract_class = true
end
class Child < SuperClass
  self.table_name = 'the_table_i_really_want'
end
```

### 单表继承

解决方法之一： 在 base class 中添加 `type` 字段

`rails g migration add_type_to_base_classes type:string`
`rake db:migrate`

之后，再往表中添加字段，都要往 base class 对应的表中添加




## 如何添加小数的 migration

`change_column :contas, :valor_total, :decimal, precision: 5, scale: 2`

https://stackoverflow.com/questions/40160884/rails-float-precision

This will allow you to have 2 digits after the decimal point and 5 digits max.
https://makandracards.com/konjoot/20755-rails-migration-add-float-point-field-with-scale-and-precision




##


### `ERROR:  column "gender" cannot be cast automatically to type integer HINT:  You might need to specify "USING gender::integer".`

```
ActiveRecord::StatementInvalid: PG::DatatypeMismatch: ERROR:  column "gender" cannot be cast automatically to type integer
HINT:  You might need to specify "USING gender::integer".
```

原因是，在设计开始时，将 `gender` 字段从 `string` 改成了 `integer`
而后又修改为 `string` ，但那个 `migration` 还在
删掉后问题解决了。


### ` ActionView::Template::Error (no implicit conversion of Array into String):`

serialize


### `ActionController::ParameterMissing (param is missing or the value is empty: profile):`


## 获取一系列数据

### 使用 `pluck`

`Client.pluck(:id, :name)`

`Client.select(:id).map { |c| c.id }`

`Client.select(:id).map(&:id)`

[pluck](http://guides.rubyonrails.org/active_record_querying.html#pluck)


* [Active Record Query Interface](http://guides.rubyonrails.org/active_record_querying.html)


## 如何在 Controller 中使用 Helper

在 Rails 5 中很简单，直接在 helper 前加上 `helpers` 就可以了

```ruby
class UsersController < ApplicationController
  def index
    render inline: helpers.my_user_helper
  end
end
```

* [Implement helpers proxy in controller instance level](https://github.com/rails/rails/pull/24866)


## 导出 Excel

### 1. Gemfile

```
gem 'rubyzip'
gem 'axlsx'
gem 'axlsx_rails'
```

`bundle` 后重启服务器

### 2. 添加导出按钮

`<%= link_to "导出为 Excel", admin_registrations_path(:format => :xlsx), :class => "btn btn-default" %>`

### 3. Controller 中添加 xlsx 格式

```ruby
respond_to do |format|
  format.html
  format.csv {
   # 略
  }
  format.xlsx
end

```

### 4. 添加 xlsx.axlsx 样板

*index.xlsx.axlsx*

```
wb = xlsx_package.workbook wb.add_worksheet(name: "Buttons") do |sheet|
  sheet.add_row ["报名ID", "票种", "姓名", "状态", "Email", "报名时间"]
  @registrations.each do |r|
    sheet.add_row [r.id, r.ticket.name, r.name, t(r.status, :scope => "registration.status"), r.email, r.created_at]
  end
end
```

### helper 的用法

在 axlsx 中是同 views 中一样，可以直接使用 helper 方法的

### 如何设置字段的类型

特别是长的数字字段，如身份证，则需要要设置为文本类型，可以这样：

```
wb.add_worksheet(:name => "Override Data Type") do |sheet|
  sheet.add_row ['dont eat my zeros!', '0088'] , :types => [nil, :string]
end
```

[https://stackoverflow.com/a/13596008/4455426](https://stackoverflow.com/a/13596008/4455426)


## 如何在 Rails Console 中搜索 Like

`ps = Profile.where("name LIKE ?", "%徐%")`
`ps.pluck(:name)`



## `Invalid Data: . Cell.type must be one of [:date, :time, :float, :integer, :string, :boolean, :iso_8601].`

`Invalid Data: . Cell.type must be one of [:date, :time, :float, :integer, :string, :boolean, :iso_8601].`



## 分享

Junior 初入职通常会遇到两种问题

一是自己完全不懂的

二是与自己以往的认知是不一样的


不要做伸手党

记下关键词，再去 Google


前端：

vue.js
react



remote
  client 需要多次频繁反馈




新手要考虑
  对老的数据、新的数据的验证（有可能


  送 pr 时，要干净不要多余的文档
  送 pr 时，一定要先测试，不要有故障
  很靠谱的去做东西

  在团队中，如何做一个靠谱的人，可能更重要。

  错误：
    不会做也不问
    也不会
    没有公德心：
      不做好测试
    没有遵守“最小惊讶原则”
    没有让同事困扰最小




将自己代码开源
参与开源项目
拉 pull-request


s
h






Junior
Senior


## 判断为零或非零

`5.zero?`
`0.zero?`

`5.nonzero?`
`0.nonzero?`


## `link_to` 根据条件设置 `disabled`

```
<%= link_to "Batch First ( #{line.size} )",
            line_registrations_path(line),
            class: "btn btn-primary btn-xs",
            disabled: line.size.zero? %>

```


## rails 中创建表时添加　`index`

### 使用 `add_index`
```ruby
def change
  create_table :cities do |t|
    t.string :province
    t.string :city

    t.timestamps
  end

  add_index :cities, :juhe_id
end
```

### 使用 `index`

```ruby
def change
  create_table :trains do |t|
    t.string :number, :index => true

    t.timestamps
  end

  add_index :cities, :juhe_id
end
```



## 如何在 ruby 文件中加载别的 ruby 文件

* `require` : 只需加载一次
  * `require "./some.rb"` 加载当前文件夹下的 `some.rb`

* `load` : 只要运行，就会加载
  * `load "./some.rb"` 加载当前文件夹下的 `some.rb`


## `=~` 什么作用?

> ruby 的操作符，匹配正则表达式

```ruby
class Car
  def go(place)
      puts "go to #{place}"
  end

  def method_missing(name, *args)
    if name.to_s =~ /^go_to_(.*)/
      go($1)
    else
      super
    end
  end
end

```



## Mac 用 VirtualBox  安装 windows 7

## 安装好之后 USB 不起作用





## Rails 
