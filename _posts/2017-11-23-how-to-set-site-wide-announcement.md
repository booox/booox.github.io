---
layout: post
comments: true
title:  "如何设置 Rails 站点的通知系统"
categories: Rails
tags:  Rails
date: 2017/11/23 10:06:11
---

* content
{:toc}

如何设置 Rails 站点的通知系统




## 创建 Model 及 Controller

* 在 Terminal 当中执行：

`$ rails g model announcement message:text starts_at:datetime ends_at:datetime`
`$ rails g controller announcements`
`$ rails g controller admin::announcements`
`$ rake db:migrate`

* 设定 announcement

```rb
# app/models/announcement.rb
def self.current(hidden_ids = nil)
  result = where("starts_at <= :now and ends_at >= :now", now: Time.zone.now)
  result = result.where("id not in (?)", hidden_ids) if hidden_ids.present?
  result
end
```

## Application.html.erb

*views/layouts/application.html.erb*

```html
<% Announcement.current(cookies.signed[:hidden_announcement_ids]).each do |announcement| %>

  <div class="alert alert-dismissable alert-success announcement" id="announcement_<%= announcement.id %>">
    <%= announcement.message %>
    <%= link_to "不再提醒", hidden_announcement_path(announcement), remote: true %>
  </div>

<% end %>

<!-- 在此行上面添加  -->
<%= render "common/flashes" %>
```

* 设置 css

```css
.announcement {
  text-align: center;
  a {
    float: right;
    color: #980905;
  }
}
```

## Controller

* 管理员通知管理

```rb
# app/controllers/admin/announcements_controller.rb

class Admin::AnnouncementsController < ApplicationController
  before_action :authenticate_user!
  before_action :admin_required
  before_action :find_announcement, only: [:edit, :update, :destroy]

  def index
    @announcements = Announcement.all
  end

  def new
    @announcement = Announcement.new
  end

  def create
    @announcement = Announcement.new(announcement_params)

    if @announcement.save
      redirect_to admin_announcements_path, notice: "Announcement Created Succeed."
    else
      render :new
    end
  end

  def edit
  end

  def update
    if @announcement.update(announcement_params)
      redirect_to admin_announcements_path, notice: "Announcement Updated Succeed."
    else
      render :edit
    end
  end

  def destroy
    @announcement.destroy
    redirect_to admin_announcements_path
  end

  private

  def announcement_params
    params.require(:announcement).permit(:message, :starts_at, :ends_at)
  end

  def find_announcement
    @announcement = Announcement.find(params[:id])
  end

end
```

* 用户通知管理

```rb
# app/controllers/announcements_controller.rb
def hide
  ids = [params[:id], *cookies.signed[:hidden_announcement_ids]]
  cookies.permanent.signed[:hidden_announcement_ids] = ids
  respond_to do |format|
    format.html { redirect_to :back }
    format.js
  end
end
```

## 路由设定

```rb
get '/announcements/:id/hide', to: 'announcements#hide', as: 'hidden_announcement'
```

## 用户选择 hide 后的处理

```js
// app/views/announcements/hide.js.erb
$("#announcement_<%= j params[:id] %>").remove();
```

## Links

* [#103 Site-Wide Announcements (revised)](http://railscasts.com/episodes/103-site-wide-announcements-revised)
