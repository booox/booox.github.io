---
layout: post
comments: true
title:  "Rails 5 使用 Migration 来操作数据库"
categories: rails
tags: rails migration
date: 2019/06/02 08:21:37
---

* content
{:toc}

Rails 5 使用 Migration 来操作数据库



在 Rails 的开发中，经常会对数据库进行修改，若直接用 SQL 来实现，则非常麻烦，在 Rails 中提供了一种非常方便的机制，就是利用 Migration 通过使用 Ruby 语言来实现数据库的操作。

## 操作流程

* 创建 migration 文件： `rails g migration file_name`
* 执行： `$ rake db migrate`

## 使用 migration 的优点

* 首先当然是，不用写 SQL 语句啦。用一种语言对接多种数据库，真的很方便。
* 可以随时查看状态： `$ rake db:migrate:status`
* 还可以有后悔药（撤销、回滚）： `$ rake db:rollback`

## 实例

下面以在项目中创建一个 meeting 的 Model 为例。

### 创建新的 Model

**Code-01**

`$ rails g model meeting title:string description:text`

经过此步之后，会创建下面几个文件.

```
Running via Spring preloader in process 15746
      invoke  active_record
      create    db/migrate/20190602010014_create_mettings.rb
      create    app/models/metting.rb
      invoke    test_unit
      create      test/models/metting_test.rb
      create      test/fixtures/mettings.yml
```

打开 *20190602010014_create_mettings.rb* 文件，内容如下：

```ruby
class CreateMettings < ActiveRecord::Migration[5.2]
  def change
    create_table :mettings do |t|
      t.string :title
      t.text :content

      t.timestamps
    end
  end
end
```

这个文件就是一个 migration 文件，我们可以通过执行下面的命令来让它生效：

**Code-02**

`$ rake db:migrate`

在上述命令执行之前，数据库中没有 **meetings** 这个表。执行之后，表就存在了。
当我们再执行 `$ rake db:roolback` (**Code-03**) 之后，表又会被移除。


注意 *create_mettings.rb* 中的 **change**

有时也可能会看到，会分开成 **up** 与 **down**。


*Code-01* 也可以写成

`$ rails g model meeting`

也就是在创建时不添加具体的字段，而是在 *xxxx_create_mettings.rb* 文件中手动添加。


### 添加一个字段

想给会议添加一个 **地点（place）** 的字段。

添加字段，需要先创建一个添加字段的 migration 文档，而后再执行 `$ rake db:migrate` 来完成添加。


这里创建 migration 文档有两种方法：

* 一种只写要创建的文档名。
* 另一种除了文档名，还加上对应的 **字段名称:类型**。

两者区别，是前者需要在创建的文档中手动添加需要添加的语句，而后者则会自动生成对应的语句。

**文档名**

创建 Migration 文档的文件名，有默认的规则，可以写成：

`AddColumnToTable` 或 `add_column_to_table`。我更推荐第二种，阅读起来方便一些。

例如这里的实例：

`$ rails g migration add_place_to_meetings`
`$ rails g migration AddPlaceToMeetings`

这样生成的文档内容如下：

```ruby
class AddPlaceToMeetings < ActiveRecord::Migration[5.2]
  def change
  end
end
```

修改对应的 migration 文档，如下：

```ruby
class AddPlaceToMeetings < ActiveRecord::Migration[5.2]
  def change
    add_column :meetings, :place, :string
  end
end
```

执行下面命令，即可完成添加的操作。

`$ rake db:migrate`


**文档名 + 字段名:类型**

如果在创建 migration 命令时，除了符合规范的文档名，后面还跟上对应的 **字段名称:类型**，则会自动完成里面语句的添加，检查无误之后，就可以执行 `$ rake db:migrate` 了。

如：

`$ rails g migration add_place_to_meetings place:string`

**文档名 + 字段名:类型:index**

如果想给新添加的字段，加上索引（index），可以这样做：

`$ rails g migration add_place_to_meetings place:string:index`

它会自动生成如下文档：

```ruby
class AddPlaceToMeetings < ActiveRecord::Migration[5.2]
  def change
    add_column :meetings, :place, :string
    add_index :meetings, :place
  end
end
```


### 添加字段时加上默认值

```ruby
class AddPlaceToMeetings < ActiveRecord::Migration[5.2]
  def change
    add_column :meetings, :place, :string, default: ''
  end
end
```


### 修改字段名称



```ruby
def change
  rename_column :meetings, :place, :place_name
end
```

### 修改字段类型

```ruby
def change
  change_column :meetings, :place, :text
end
```

但要注意的是，`change_column` 是不可逆的。

> Note that change_column command is irreversible.


可以用下面方法代替，更安全一些：

```ruby
def up
    change_column :meetings, :place, :text
end

def down
    change_column :meetings, :place, :string
end
```




### 修改字段空值及默认值

```ruby
def change
  change_column_null :meetings, :place, false
  change_column_default :meetings, :out_dated, from: false, to: true
end
```

### 删除字段

**remove_column (must supply a type)**

```ruby
def change
    remove_column :meetings, :place, :string
end
```

### 删除表

```ruby
def change
    drop_table :meetings
end
```

### 删除 migration 文档

只要修改创建命令中的 `g` 为 `d` 就可以了。

如： `$ rails d migration add_place_to_meetings`


## Links

* [Active Record Migrations](https://guides.rubyonrails.org/active_record_migrations.html)
* [Active Record - 資料庫遷移(Migration)](https://ihower.tw/rails/migrations.html)
