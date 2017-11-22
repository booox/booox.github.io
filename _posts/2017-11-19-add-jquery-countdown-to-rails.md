---
layout: post
comments: true
title:  "添加 jQuery Countdown 到 Rails"
categories:  Rails
tags:  jQuery Rails
date: 2017/11/19 10:38:09
---

* content
{:toc}

添加 jQuery Countdown 到 Rails



下面实现的是如何在 Rails 项目中添加一个倒计时器。

## 应用 jQuery Countdown

* 下载 jQuery Countdown

在 [jQuery Countdown](http://keith-wood.name/countdown.html) 页面上找到 "Download now"

解压下载好的压缩包：

  * 将 `js/jquery.countdown.min.js` 放到 `vendor/assets/javascripts/`
  * 将 `js/jquery.plugin.min.js` 放到 `vendor/assets/javascripts/`
  * 将 `js/jquery.countdown.css` 放到 `vendor/assets/stylesheets/`
  * 将 `img` 放到 `public/images/` （如果用的计时器需要图片的话）


* 修改 `app/assets/javascript/application.js`

添加带 `+` 的两行，注意顺序： `plugin` 在 `countdown` 上面

```
+ //= require jquery.plugin.min  
+ //= require jquery.countdown.min
//= require_tree .
```
* 修改 `app/assets/javascript/application.scss`

在最后添加

```
+ @import "jquery.countdown";
```

* 修改 `some_page.html.erb`

在想放置计时器的位置添加

```
+ <span id="countdown"></span>
```

`countdown` 用于后面自定义 CSS，名称可自定义

* 修改 `some_page.html.erb`

还是上面同一个页面，最后添加：

```html
<script>
  $('#countdown').countdown({until: '+20m', compact: true, description: ""});
</script>
```

其它样式，可以到 [jQuery Countdown](http://keith-wood.name/countdown.html) 页面上查找。

找到合适的，查看代码，即可拿来用了。

* 自定义样式

修改 `app/assets/javascript/application.scss`

当然，也可以放到别的样式文件中，因为 rails 最后会合并成一个样式文件的


```css
#countdown {
  font-size: 12px;
}
```



## Links

* [jQuery Countdown](http://keith-wood.name/countdown.html)
