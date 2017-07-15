---
layout: post
comments: true
title:  "Rails Devise Errors"
categories: Rails
tags: rails-error devise
date:
---

* content
{:toc}

操作 Devise 时遇到的一些错误





## `Unpermitted parameter: email`

需要设置 `configure_permitted_parameters`

*app/controllers/application_controller.rb*

```ruby
  before_action :configure_permitted_parameters, if: :devise_controller?

  protected

  def configure_permitted_parameters
    added_attrs = [:email, :password, :password_confirmation, :remember_me]
    devise_parameter_sanitizer.permit :sign_up, keys: added_attrs
    devise_parameter_sanitizer.permit :account_update, keys: added_attrs
  end

```

* [https://github.com/plataformatec/devise#strong-parameters](https://github.com/plataformatec/devise#strong-parameters)

![]({{site.url}}/images/rails-no-implicit-conversion.png)




##
