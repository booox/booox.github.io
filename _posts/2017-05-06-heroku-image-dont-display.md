---
layout: post
comments: true
title:  "本地一切正常，heroku 上图片不显示"
categories:   Rails
tags:  rails heroku
date: 2017/05/06 13:31:57
---

* content
{:toc}

本地一切正常，heroku 上图片不显示



## 不显示原因

* 如果图片是放到 `app/public/images` 里
  * Heroku 会将免费用户存放到 `app/public` 当中的图片删除掉
  * 但放在 `app/assets/images` 里的不会被删除
* 如果图片是放到 `app/assets/images` 里
  * 但 rails 在 `production` 环境时，是不能直接处理静态资源的。
  * 所谓静态资源是指： images, javascript, css 等
  * 要经过 `precompile` 将静态资源打成一个经过压缩的文件，如下图，文件名为 `application-xxx.css` 或 `application-xxx.js`
    * 其中 `xxx` 为时间戳，用于区分由不同时间 `precompile` 的文件
    * ![]({{site.url}}/images/assets-precompile.png)


## 解决办法

### 图片放到 `app/assets/images` 里


1. 修改 *config/environments/production.rb*
`config.assets.compile = true`

2. 终端里执行：
  `rake assets:precompile`
  `git add .`
  `git commit -m "message"`

3. 将修改推送到 heroku
  `git push heroku 最新分支:master`


### 使用图片外链

先将图片存入图床，而后直接使用图片的链接，如
`http://xxx.jpg`

如果在 css 中使用图片链接，

## 几点注意

* 只要修改了静态资源，如图片、CSS 或 Javascript ，推送之前就需要执行 `rake assets:precompile`


## 关于 assets 的 helper


*config/application.rb*
`config.assets.paths << "#{Rails.root}/app/assets/videos"`

`app/assets/videos/show.mp4`


* 返回一个指向资源的字符串
`asset-path("rails.png")` => `"/assets/rails.png"`
* 返回一个资源的地址引用
`asset-url("rails.png")` => `url(/assets/rails.png)`

为了方便起见，不同的 asset 有对应的 `-path` 与 `-url` helper：image, font, video, audio, javascript, stylesheet。
例如：

  * `image-path("rails.png")` => `"/assets/rails.png"`
  * `image-url("rails.png")` => `url(/assets/rails.png)`

  * 其它有： `font-path`, `video-path`, `audio-path`, `javascript-path`, `stylesheet-path`
  * 其它有： `font-url`, `video-url`, `audio-url`, `javascript-url`, `stylesheet-url`

* 返回一个对特定目录用 Base64 进行编码的地址引用

  * `asset-data-url("rails.png")` => `url(data: image/png; base64, iVBORw0K...)`




* [sass-rails asset path helper](https://github.com/rails/sass-rails)
