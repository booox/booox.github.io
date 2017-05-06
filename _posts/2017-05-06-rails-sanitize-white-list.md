---
layout: post
comments: true
title:  "Rails HTML Tags sanitize"
categories: Rails
tags: rails
date: 2017/05/06 09:29:59
---

* content
{:toc}

有时需要让一些 HTML 标签(tags)或属性(attributes) 能原样显示出来。




## 使用 `simple_format`

`simple_format` 支持多行显示，将每行内容用 `p` 标签括起来。并会将不支持的 HTML 标签过滤掉。

### `simple_format` 使用实例

```ruby
my_text = "Here is some basic text...\n...with a line break."

# 默认输出
simple_format(my_text)
# => "<p>Here is some basic text...\n<br />...with a line break.</p>"
# 可以看到默认情况下是用 <p> 将每行包起来

# 添加外部标签
simple_format(my_text, {}, wrapper_tag: "div")
# => "<div>Here is some basic text...\n<br />...with a line break.</div>"

more_text = "We want to put a paragraph...\n\n...right there."

# 两个换行符产生一个新段
simple_format(more_text)
# => "<p>We want to put a paragraph...</p>\n\n<p>...right there.</p>"

# 添加样式
simple_format("Look ma! A class!", class: 'description')
# => "<p class='description'>Look ma! A class!</p>"

# 过滤不支持的标签，如这里的 <blink>
simple_format("<blink>Unblinkable.</blink>")
# => "<p>Unblinkable.</p>"

# 设置 sanitize 为 false，不过滤标签
simple_format("<blink>Blinkable!</blink> It's true.", {}, sanitize: false)
# => "<p><blink>Blinkable!</blink> It's true.</p>"

```


## Rails 默认输出模式

出于安全的因素，默认情况下，Rails 会将所有 HTML 标签、属性转换成普通文本，如 `<` 变成 `&lt;` 。

## 使用 raw 或 html_safe

在 Rails 默认情况下，会将 HTML 标签和属性全部转成普通文本。可如何不让 HTML 标签和属性转换呢？

可以通过

`<%= raw @job.description %>`

或

`<%= @job.description.html_safe %>`

这样是可以不让 HTML 被转换，但存在很大的安全隐患。

如用户可以输入 Javascript ，如
`<script>alert("Hack you");</script>`
或
`<script>location.href='https://ihower.tw'</script>`


## santize 白名单过滤

需要显示用户输入的 HTML 又要有安全性，那就需要一种白名单的过滤机制了，Rails 提供了 sanitize helper。

### `sanitize` 支持的标签属性

* 允许的标签：
  * 可以通过在 `rails console` 中执行 `ActionView::Base.sanitized_allowed_tags` 来查看
* 允许的属性
  * 可以通过在 `rails console` 中执行 `ActionView::Base.sanitized_allowed_attributes` 来查看

也可查看源代码

![]({{site.url}}/images/sanitizer-allowed-tags.png)


### `sanitize` 用法

`<%= sanitize @job.description %>`

### 添加允许的标签或属性

修改 *config/application.rb* ，
如添加 `table`, `tr`, `td` 标签，添加 `style` , `border` 属性

```ruby
class Application < Rails::Application
  ......
  + config.action_view.sanitized_allowed_tags = Rails::Html::WhiteListSanitizer.allowed_tags + %w(table tr td)
  + config.action_view.sanitized_allowed_attributes = Rails::Html::WhiteListSanitizer.allowed_attributes + %w(style border)
end
```

## Links

* [simple_format (ActionView::Helpers::TextHelper) - APIdock](https://apidock.com/rails/ActionView/Helpers/TextHelper/simple_format)
* [bootstrap-datetimepicker Options](https://eonasdan.github.io/bootstrap-datetimepicker/Options/#options)
* [simple_format does not escape HTML tags](https://makandracards.com/makandra/11459-simple_format-does-not-escape-html-tags)
* [rails-html-sanitizer sanitizer.rb](https://github.com/rails/rails-html-sanitizer/blob/master/lib/rails/html/sanitizer.rb)
