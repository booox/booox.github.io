---
layout: post
comments: true
title:  "Rails 12 week Notes"
categories: software
tags: Rails 12 week Notes
date: 2018/08/15 08:55:02
---

* content
{:toc}

Rails 12 week Notes


`$ rails g scaffold link title:string url:string`
`$ rake db:migrate`

### Merge to master

`$ git checkout -b the_branch_want_to_merge`
`$ git add .`
`$ git commit -am "some messages"`
`$ git checkout master`
`$ git merge the_branch_want_to_merge`

### Install Devise

Add to Gemfile:

`gem 'devise'`

Run:

`$ rails g devise:install `
`$ rails g devise views `
`$ rails g devise User `
`$ rake db:migrate`

[sig_up](http://localhost:3000/users/sign_up)
[sig_in](http://localhost:3000/users/sign_in)
