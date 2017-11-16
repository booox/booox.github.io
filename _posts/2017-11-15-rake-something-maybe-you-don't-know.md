---
layout: post
comments: true
title:  "Rake 相关的事"
categories:  Rails
tags:  Rails rake
date: 2017/11/16 10:48:20
---

* content
{:toc}

Rake 相关的事




## 生成 rake 任务

```
$ rails g my_namespace my_task1 my_task2

  create lib/tasks/my_namespace.rake
```

它会为我们新的 task 搭好脚手架

`lib/tasks/my_namespace.rake`

```
namespace :my_namespace do
  desc "TODO"
  task :my_task1 => :environment do
  end

  desc "TODO"
  task :my_task2 => :environment do
  end
end
```

* 查看 task 信息

`$ rake -T my_namespace`

Nice.

* 删除 task

```
$ rails d task my_namespace my_task1 my_task2
      remove  lib/tasks/my_namespace.rake
```

Very Nice.

****

查了一下 `rake` 的帮助，试了一下 `rake -T` 一下子发现好多宝贝，原来他们一直在这里，哈哈。


## 常见的 Rake 任务


列出所有的 rake 任务

```
$ rake -T

rake about                              # List versions of all Rails frameworks and the environment
rake app:template                       # Applies the template supplied by LOCATION=(/path/to/template) or URL
rake app:update                         # Update configs and some other initially generated files (or use just update:configs or update:bin)
rake assets:clean[keep]                 # Remove old compiled assets
rake assets:clobber                     # Remove compiled assets
rake assets:environment                 # Load asset compile environment
rake assets:precompile                  # Compile all the assets named in config.assets.precompile
rake cache_digests:dependencies         # Lookup first-level dependencies for TEMPLATE (like messages/show or comments/_comment.html)
rake cache_digests:nested_dependencies  # Lookup nested dependencies for TEMPLATE (like messages/show or comments/_comment.html)
rake db:create                          # Creates the database from DATABASE_URL or config/database.yml for the current RAILS_ENV (use db:create:all to c...
rake db:drop                            # Drops the database from DATABASE_URL or config/database.yml for the current RAILS_ENV (use db:drop:all to drop ...
rake db:environment:set                 # Set the environment value for the database
rake db:fixtures:load                   # Loads fixtures into the current environment's database
rake db:migrate                         # Migrate the database (options: VERSION=x, VERBOSE=false, SCOPE=blog)
rake db:migrate:status                  # Display status of migrations
rake db:rollback                        # Rolls the schema back to the previous version (specify steps w/ STEP=n)
rake db:schema:cache:clear              # Clears a db/schema_cache.dump file
rake db:schema:cache:dump               # Creates a db/schema_cache.dump file
rake db:schema:dump                     # Creates a db/schema.rb file that is portable against any DB supported by Active Record
rake db:schema:load                     # Loads a schema.rb file into the database
rake db:seed                            # Loads the seed data from db/seeds.rb
rake db:setup                           # Creates the database, loads the schema, and initializes with the seed data (use db:reset to also drop the datab...
rake db:structure:dump                  # Dumps the database structure to db/structure.sql
rake db:structure:load                  # Recreates the databases from the structure.sql file
rake db:version                         # Retrieves the current schema version number
rake dev:cache                          # Toggle development mode caching on/off
rake initializers                       # Print out all defined initializers in the order they are invoked by Rails
rake log:clear                          # Truncates all/specified *.log files in log/ to zero bytes (specify which logs with LOGS=test,development)
rake middleware                         # Prints out your Rack middleware stack
rake notes                              # Enumerate all annotations (use notes:optimize, :fixme, :todo for focus)
rake notes:custom                       # Enumerate a custom annotation, specify with ANNOTATION=CUSTOM
rake restart                            # Restart app by touching tmp/restart.txt
rake routes                             # Print out all defined routes in match order, with names
rake secret                             # Generate a cryptographically secure secret key (this is typically used to generate a secret for cookie sessions)
rake stats                              # Report code statistics (KLOCs, etc) from the application or engine
rake test                               # Runs all tests in test folder
rake test:db                            # Run tests quickly, but also reset db
rake time:zones[country_or_offset]      # List all time zones, list by two-letter country code (`rails time:zones[US]`), or list by UTC offset (`rails ti...
rake tmp:clear                          # Clear cache and socket files from tmp/ (narrow w/ tmp:cache:clear, tmp:sockets:clear)
rake tmp:create                         # Creates tmp directories for cache, sockets, and pids

```

从中可以看到常见的 rake 任务

* 根据描述来筛选 rake 任务

```
$ rake -T | grep cache
rake cache_digests:dependencies         # Lookup first-level dependencies for TEMPLATE (like messages/show or comments/_comment.html)
rake cache_digests:nested_dependencies  # Lookup nested dependencies for TEMPLATE (like messages/show or comments/_comment.html)
rake db:schema:cache:clear              # Clears a db/schema_cache.dump file
rake db:schema:cache:dump               # Creates a db/schema_cache.dump file
rake dev:cache                          # Toggle development mode caching on/off
rake tmp:clear                          # Clear cache and socket files from tmp/ (narrow w/ tmp:cache:clear, tmp:sockets:clear)
rake tmp:create                         # Creates tmp directories for cache, sockets, and pids
```






## Links

* [How to generate rake task](https://railsguides.net/how-to-generate-rake-task/)
