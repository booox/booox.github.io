---
layout: post
comments: true
title:  "jQuery 实现 modal 可以拖放"
categories:  jQuery
tags:  jquery rails
date: 2017/11/07 18:02:09
---

* content
{:toc}

jQuery 实现 modal 可以拖放



## 添加 jQuery-ui 支持

**Gemfile**

`gem 'jquery-ui-rails'`

执行 `bundle install` ，重启 `rails s`

**application.js**

` //= require jquery-ui/widgets/draggable `

**application.scss**

` *= require jquery-ui/draggable `



## 启用拖放

```js
// application.js

$(document).ready(function() {
  $("#complainModal").find(".modal-dialog").draggable({
      handle: ".modal-header"
  });
}）
```

**html.erb**

```html
<div class="modal fade modal-draggable message-modal-style" id="complainModal" tabindex="-1" role="dialog" aria-labelledby="myModalLabel">
    <div class="modal-dialog ui-draggable">
        <div class="modal-content">
            <div class="modal-header ui-draggable-handle">
                <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                    <span aria-hidden="true">×</span>
                </button>
                <h4 class="modal-title" id="complainModalLabel">您对本题有什么想说的？</h4>
            </div>

            <%= render "form" %>
        </div>
    </div>
</div>
```

## Links

* [jQuery ui Draggable](http://jqueryui.com/draggable/)
