---
layout: post
comments: true
title:  "本地一切正常，heroku 上图片不显示"
categories:   Rails
tags:  rails heroku
date:
---

* content
{:toc}

本地一切正常，heroku 上图片不显示


Heroku 会将免费用户存放到 `app/public` 当中的图片删除掉
但放在 `app/assets/images` 里的不会被删除（待确认）？


修改 *config/environments/production.rb*

`config.assets.compile = true`

终端里执行：
  `rake assets:precompile`
  `git add .`
  `git commit -m "message"`


`history | tail -20`

`heroku run rake db:migrate`

*config/application.rb*
`config.assets.paths << "#{Rails.root}/app/assets/videos"`

`app/assets/videos/show.mp4`
