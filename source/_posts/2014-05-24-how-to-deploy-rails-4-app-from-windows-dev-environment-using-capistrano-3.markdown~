---
layout: post
title: "How-to: Deploy Rails 4 app from Windows dev environment using Capistrano 3"
date: 2014-05-24 21:26
comments: true
categories: 
- 技术
- rails
- capistrano
---
This post will be primarily sharing some gotchas when I went through this [post](http://corlewsolutions.com/articles/article-10-how-to-deploy-rails-applications-using-capistrano-3-1-and-windows-7) for deploying to a Linux server from Windows development machines using capistrano 3. 

<!--more-->

# Environment

- Development on Windows 7
- Deployment on [aliyun ECS](http://www.aliyun.com/) Ubuntu 12.04 image
- Rails 4
- Capistrano 3

# Sidenote

- Capistrano will automatically run `rake db:migration` for you if you uncomment `require 'capistrano/rails/migrations'` in capfile.
- Capistrano will automatically precompile assets for you if you uncomment `require 'capistrano/rails/assets'` in capfile.

# fetch not working?

When running `cap production git:check`, I always get something like the following error when I use a variable defined by `set` method.

``` ruby 
set :github_user, 'Fatman13'
set :repo_url, 'https://github.com/#{fetch(:github_user)}/foo.git'
```

```
DEBUG [39feea1e] Running /usr/bin/env git ls-remote -h https://github.com/#{fetch(:github_user)}/foo.git on 114.***.***.***
DEBUG [39feea1e] Command: ( GIT_ASKPASS=/bin/echo GIT_SSH=/tmp/foo/git-ssh.sh /usr/bin/env git ls-remote -h https://github.com/#{fetch(:github_user)}/foo.git )
DEBUG [39feea1e] Finished in 0.962 seconds with exit status 1 (failed).
DEBUG [39feea1e]        bash: -c: line 0: syntax error near unexpected token `('
DEBUG [39feea1e]        bash: -c: line 0: `( GIT_ASKPASS=/bin/echo GIT_SSH=/tmp/deepot/git-ssh.sh /usr/bin/env git ls-remote -h https://github.com/#{fetch(:github_user)}/foo.git )'
DEBUG [39feea1e] Finished in 0.962 seconds with exit status 1 (failed).
```

I have to ditch `fetch` method and write something like...

``` ruby
set :repo_url, 'https://github.com/Fatman13/foo.git'
```

# No such file to load -- bcrypt

Capistrano will try to precompile assets at somepoint while running `cap production deploy`. It kept throwing an error saying `No such file to load -- bcrypt`. I found a comment from [Railscast](http://railscasts.com/episodes/335-deploying-to-a-vps?view=comments) solved my problem.

> I had this issue. I develop on Windows 7, and certain gems have windows-specific versions. I went into my Gemfile.lock and removed all "x86-mingw32" in the gem version numbers. After commiting the changes and deploying again, it worked. I also had this problem with postgres and the pg gem.
> (by [jdresner](https://github.com/jdresner))

# References

- [1] My [gemfile](https://github.com/Fatman13/deepot/blob/master/Gemfile).
- [2] Corlew solutions's [How-To Deploy Rails Applications Using Capistrano 3.1 and Windows 7](http://corlewsolutions.com/articles/article-10-how-to-deploy-rails-applications-using-capistrano-3-1-and-windows-7)
- [3] Corlew solutions's [Guide To Setting Up SSH on Windows 7](http://corlewsolutions.com/articles/article-11-guide-to-setting-up-ssh-on-windows-7)
