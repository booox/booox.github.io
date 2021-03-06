---
layout: post
comments: true
title:  "Rails 5 渲染 Markdown 及实现代码高亮"
categories: rails
tags: rails redcarpet coderay rouge
date: 2018/08/07 11:01:38
---

* content
{:toc}

Rails 5 渲染 Markdown 及实现代码高亮



Markdown 是一种很方便易学、功能强大、纯文本格式的标记语言。可以让你专注于写作，不会因考虑「如何排版」而分心。而在 Rails 中如何将 Markdown 文本渲染输出以及如何实现代码高亮显示呢？

## 主要实现的思路

将 Markdown 格式的文本通过 Redcarpet 渲染输出；再利用 Coderay 或 Rouge 等实现代码高亮。

Markdown 的文本是纯文本的格式，就像下面：

```Markdown
# 一级标题
## 二级标题

列表
* 项目一
* 项目二
* 项目三
```

而要让这些纯文本的文字能与预先设定的样式对应起来，则需先把上述纯文本通过某种机制处理成 HTML 格式的文本，这样就可以应用 CSS 了。

这时，我们就需要用到 `Redcarpet` (https://github.com/vmg/redcarpet) 。

而为了让 Markdown 文本中的各种语言的代码能够根据关键字等呈现相应的颜色，即代码亮亮显示，我们又要用到 `Coderay` 或 `Rouge` 。


下面就分 `Coderay` 与 `Rouge` 两种方法来分别实现上述功能。

## 使用 Redcarpet 与 Coderay


**安装 gem**

在 *Gemfile* 中添加如下语句：

```
gem 'redcarpet'
gem 'coderay'
```

并执行后重启 Rails：

`bundle install`


**写 markdown 的 helper**

```ruby
module ApplicationHelper
  # for markdown
  class HtmlWithCoderay < Redcarpet::Render::HTML
    def block_code(code, language)
      CodeRay.scan(code, language).div()
    end
  end

  def markdown(text)
    render_options = {
      filter_html: true,
      hard_wrap: true,
      with_toc_data: true,
      link_attributes: { rel: 'nofollow', target: '_blank' }
    }
    html_with_coderay = HtmlWithCoderay.new( render_options )

    options = {
        :fenced_code_blocks => true,
        :no_intra_emphasis => true,
        :autolink => true,
        :tables => true,
        :strikethrough => true,
        :lax_html_blocks => true,
        :superscript => true
    }
    markdown_to_html = Redcarpet::Markdown.new(html_with_coderay, options)
    markdown_to_html.render(text).html_safe
  end
  # end: markdown

end
```

如果想显示代码的行号，只要将

`CodeRay.scan(code, language).div()`

替换成：

`CodeRay.scan(code, language).div(line_numbers: :table)`

**用于代码高亮的样式**

可将该链接下的样式复制

[https://gist.github.com/booox/7e31fd73c07643a16f96a687b51f2728#file-coderay-css](https://gist.github.com/booox/7e31fd73c07643a16f96a687b51f2728#file-coderay-css)

粘贴到新建的 *vendor/assets/stylesheets/coderay.css* 中

并在 *app/assets/stylesheets/application.scss* 中
添加如下代码即可：

`@import "coderay";`




**View 中使用 markdown**

这样，在 view 中就可以使用 `markdown` 这个 helper 了。

```
<%= markdown(@content)  %>
```

## 使用 Redcarpet 与 Rouge

Rouge 与　Coderay 相比起来，支持的语言更多一些

**安装 gem**

在 *Gemfile* 中添加如下语句：

```
gem 'redcarpet'
gem 'rouge', '~> 3.2'
```

并执行后重启 Rails：

`bundle install`


**写 markdown 的 helper**

这里与上面有些不一样，需要创建一个新的文件

*app/helpers/rouge_helper.rb*

```ruby
module RougeHelper
  require 'redcarpet'
  require 'rouge'
  require 'rouge/plugins/redcarpet'

  class HtmlWithRouge < Redcarpet::Render::HTML
    include Rouge::Plugins::Redcarpet
  end

  def markdown(text)
    render_options = {
        filter_html: true,
        hard_wrap: true,
        link_attributes: { rel: 'nofollow', target: '_blank' }
    }
    renderer = HtmlWithRouge.new(render_options)

    extensions = {
        autolink: true,
        fenced_code_blocks: true,
        lax_spacing: true,
        no_intra_emphasis: true,
        strikethrough: true,
        tables: true,
        superscript: true
    }
    markdown = Redcarpet::Markdown.new(renderer, extensions)
    markdown.render(text).html_safe
  end
end

```


**用于代码高亮的样式**

可将该链接下的样式复制

[https://gist.github.com/booox/7e31fd73c07643a16f96a687b51f2728#file-rouge_github-css](https://gist.github.com/booox/7e31fd73c07643a16f96a687b51f2728#file-rouge_github-css)

粘贴到新建的 *vendor/assets/stylesheets/rouge_github.css* 中

并在 *app/assets/stylesheets/application.scss* 中
添加如下代码即可：

`@import "rouge_github";`


或者在命令行下输入如下内容：

`> rougify style github > rouge_github.css`

而更多的样式，可以查看下面的链接：

[https://github.com/jneen/rouge/tree/master/lib/rouge/themes](https://github.com/jneen/rouge/tree/master/lib/rouge/themes)



**View 中使用 markdown**

这里与上面一样了，在 view 中就可以使用 `markdown` 这个 helper 了。

```
<%= markdown(@content)  %>
```

## Markdown 渲染所用样式表

除了上述用于代码高亮的样式表之外，Markdown 渲染后的 HTML　代码还需要对应的样式

可将该链接下的样式复制

[https://gist.github.com/booox/7e31fd73c07643a16f96a687b51f2728#file-github-markdown-css](https://gist.github.com/booox/7e31fd73c07643a16f96a687b51f2728#file-github-markdown-css)

粘贴到新建的 *app/assets/stylesheets/github-markdown.css* 中

并在 *app/assets/stylesheets/application.scss* 中
添加如下代码即可：

`@import "github-markdown";`





## Links

* [Markdown and code syntax highlighting in Ruby on Rails (using RedCarpet and CodeRay)](http://allfuzzy.tumblr.com/post/27314404412/markdown-and-code-syntax-highlighting-in-ruby-on)
* [Syntax highlight with Redcarpet and Rouge](https://www.thecodecreators.com/articles/syntax-highlight-with-redcarpet-and-rouge)
* [Using Markdown and Syntax highlighting in a Rails application](http://anthonycandaele.com/articles/using-markdown-and-syntax-highligting-in-a-rails-application)
* [jneen/rouge](https://github.com/jneen/rouge)
* [vmg/redcarpet](https://github.com/vmg/redcarpet)
