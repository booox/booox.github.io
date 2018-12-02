---
layout: post
comments: true
title:  "微信技巧"
categories: software
tags: software wechat
date: 2017/12/30 21:08:59
---

* content
{:toc}

 上传图片出错


## Errors

When I just post some text, everything is OK.
But when I try to upload image(s) from the file field, the errors appear.


Chrome hint:

  `ActionController::UnknownFormat`

Rails log:
  ```
  Completed 406 Not Acceptable in 95ms (ActiveRecord: 1.6ms)
  ActionController::UnknownFormat (ActionController::UnknownFormat):
  ```
When I checked rails console, the image was successfully inserted.


```
Started POST "/sections/1/complain" for 127.0.0.1 at 2018-06-20 14:51:00 +0800
Processing by SectionsController#complain as HTML
  Parameters: {"utf8"=>"✓", "authenticity_token"=>"9rJOtCvS/96OeodlYQwVm4WyoH07qdn/xsfYheuKD8/2+GlOSMZ3oQghJRtejczWoVZxZVxQdkfjswl91hFNDA==", "complain"=>{"content"=>"werq", "images"=>[#<ActionDispatch::Http::UploadedFile:0x00007faea5fd8378 @tempfile=#<Tempfile:/var/folders/9d/r_j7lbfx4_ngssrlm8f1cf4r0000gn/T/RackMultipart20180620-76271-1akth5a.jpg>, @original_filename="1111.jpg", @content_type="image/jpeg", @headers="Content-Disposition: form-data; name=\"complain[images][]\"; filename=\"1111.jpg\"\r\nContent-Type: image/jpeg\r\n">]}, "commit"=>"Add Complain", "course_id"=>"2", "id"=>"1"}
  User Load (0.3ms)  SELECT  "users".* FROM "users" WHERE "users"."id" = ? ORDER BY "users"."id" ASC LIMIT ?  [["id", 1], ["LIMIT", 1]]
  Course Load (0.1ms)  SELECT  "courses".* FROM "courses" WHERE "courses"."id" = ? LIMIT ?  [["id", 2], ["LIMIT", 1]]
  Section Load (0.1ms)  SELECT  "sections".* FROM "sections" WHERE "sections"."course_id" = ? AND "sections"."id" = ? LIMIT ?  [["course_id", 2], ["id", 1], ["LIMIT", 1]]
   (0.1ms)  begin transaction
  SQL (0.4ms)  INSERT INTO "complains" ("user_id", "section_id", "content", "created_at", "updated_at", "images") VALUES (?, ?, ?, ?, ?, ?)  [["user_id", 1], ["section_id", 1], ["content", "werq"], ["created_at", "2018-06-20 06:51:00.097147"], ["updated_at", "2018-06-20 06:51:00.097147"], ["images", "[\"1111.jpg\"]"]]
   (0.6ms)  commit transaction
   (0.0ms)  begin transaction
   (0.0ms)  commit transaction
Completed 406 Not Acceptable in 95ms (ActiveRecord: 1.6ms)



ActionController::UnknownFormat (ActionController::UnknownFormat):

app/controllers/sections_controller.rb:19:in `complain'
```


## Goal

User can upload multiple images when add **complain**.

### Modle

*app/models/section.rb*

```ruby
class Section < ApplicationRecord
  has_many :complains, dependent: :destroy
end
```

*app/models/complain.rb*

```ruby
class Complain < ApplicationRecord
  mount_uploaders :images, ComplainImageUploader
  serialize :images, JSON

  belongs_to :user,  optional: true
  belongs_to :section, optional: true
end
```


*app/models/user.rb*

```ruby
class User < ApplicationRecord
  has_many :complains, dependent: :destroy
end
```

### Controller

```ruby
class SectionsController < ApplicationController

  def show
    @section = @course.sections.find(params[:id])
    @complain = Complain.new
  end

  def complain
    @section = @course.sections.find(params[:id])

    complain = Complain.create( user: current_user,
                                section_id: @section.id,
                                content: params[:complain][:content],
                                images: params[:complain][:images] )
    if complain.save!
      respond_to do |format|
        format.js
      end
    end
  end

end

```

How can I fix this? Any idea, please.
