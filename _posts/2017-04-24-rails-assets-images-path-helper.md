---
layout: post
comments: true
title:  "rails image path 图片路径"
categories:  Rails
tags:  rails assets
date: 2017/04/25 22:39:18
---

* content
{:toc}

Rails 中静态资源的地址引用，特别是 images 如何调用有时是个问题……



## 图片放在 app/assets/images

### 在 scss  中使用：

`background: url("icon.png");`

### 在 view 中使用

如 `（*.html.erb) `

可直接用：`image_url('logo.png’)`
或用：`<%= image_tag 'logo.png' %>`  
注意都没有前缀，因为，它会自动将 *app/assets/images* 文件夹下图片进行索引。


## 图片放到 public 中

如：*public/images/1.jpg*

### 从 scss  中引用：

`background-image: url("/images/1.jpg");`

注意，前面有个 “/“ 符号

### 从 view 中引用
任何在 `public` 文件夹中的文件，都可以通过根目录 ( `/` ) ，所以，可以使用：
   ` <img src="/images/meta_learn.png" />`
如果你想使用 rails tag ，则可以使用：
    `<%= image_tag(“rss.jpg”, :alt => “rss feed”) %>`


## 在 Rails Console 中测试

```ruby

2.3.1 :002 > helper.image_tag('abc.jpg')
 => "<img src=\"/images/abc.jpg\" alt=\"Abc\" />"
2.3.1 :003 > helper.image_path('abc.jpg')
 => "/images/abc.jpg"
2.3.1 :004 > helper.image_url('abc.jpg')
 => "/images/abc.jpg"
2.3.1 :005 > helper.image_submit_tag('abc.jpg')
 => "<input alt=\"Abc\" type=\"image\" src=\"/images/abc.jpg\" />"
2.3.1 :006 > helper.image_alt('abc.jpg')
 => "Abc"

 2.3.1 :009 > helper.asset_url('abc.jpg')
 => "/abc.jpg"
2.3.1 :010 > helper.asset_path('abc.jpg')
 => "/abc.jpg"

```



## Links

* [ActionView::Helpers::AssetTagHelper](http://api.rubyonrails.org/classes/ActionView/Helpers/AssetTagHelper.html)
