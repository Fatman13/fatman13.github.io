---
layout: post
title: "How-to: Host rails app on OpenShift for FREE"
date: 2013-09-16 14:39
comments: true
categories:
- Ruby
- Rails
- 技术 
---
There are many great options for hosting your rails application. For instances, [PaaS](http://en.wikipedia.org/wiki/Platform_as_a_service) like [engine yard](https://www.engineyard.com/) and [heroku](https://www.heroku.com/) provide great scalability and automations to ease your deployment. VPS like [Linode](https://www.linode.com/) provides more traditional hosting services, which gives you full control of a node. But what if you just want to test your idea and have a public domain name (no tunneling and router tricks)? Then OpenShift is a great choice, cause it is FREE! (at least for your first 3 gears)

<!--more-->

# Good Resources

- Official rails quick starter [guide](https://www.openshift.com/kb/kb-e1005-ruby-on-rails-openshift-quickstart-guide).

- Official sample rails app on [github](https://github.com/openshift-quickstart/rails-sunspot-openshift-quickstart).

- Official documentation on deployment [scripts](http://openshift.github.io/documentation/oo_cartridge_developers_guide.html#openshift-builds). 

- Deployment tutorial [guide](http://ror-tech.blogspot.com/2013/04/deploying-rails-application-on-to.html).

# Deployment

*This guide will be similar to steps described in the resouces mentioned above. I will add my thoughts to some of the steps.*

**1**. Create an [OpenShift](http://www.openshift.com) account.

**2**.  Install `rhc` gem. (If you are using `rvm`, don’t use `sudo`.)
```
gem install rhc
``` 

**3**. Create your cartridge. This should create a folder called `[your_rails_app_name]` at `.`.
```
rhc app create -a [your_rails_app_name] -t ruby-1.9
```

**4**. Add database support to your application.
```
rhc cartridge add -a [your_rails_app_name] -c [database_name]
```
Here is a list of supported database.

Short Name | Full name
:------------------- | :-------------------
10gen-mms-agent-0.1 | 10gen Mongo Monitoring Service Agent
cron-1.4 | Cron 1.4
jenkins-client-1 | Jenkins Client
mongodb-2.2 | MongoDB NoSQL Database 2.2
mysql-5.1 | MySQL Database 5.1
metrics-0.1 | OpenShift Metrics 0.1
haproxy-1.4 | OpenShift Web Balancer
phpmyadmin-3 | phpMyAdmin 3.4
postgresql-8.4 | PostgreSQL Database 8.4
postgresql-9.2 | PostgreSQL Database 9.2
rockmongo-1.1 | RockMongo 1.1
switchyard-0 | SwitchYard 0.8.0

**5**. Add `deploy` script to `[your_rails_app_name]/.openshift/action_hooks/`.
```
touch [your_rails_app_name]/.openshift/action_hooks/deploy
```
Add the following code to `deploy` file to initialize database.
``` ruby
pushd ${OPENSHIFT_REPO_DIR} > /dev/null
bundle exec rake db:migrate RAILS_ENV="production"
popd > /dev/null
```
*NOTE: This might be obvious to experienced Linux user, but I failed to realize this the first time. You have to do a `sudo chmod +x [your_rails_app_name]/.openshift/action_hooks/deploy` on your `deploy` script or it won’t be run by the server.*

**6**. Change production database configuration in `config/database.yml`. Then submit your change to your [github](https://github.com/) repository.
``` yaml

production:
  adapter: mysql2
  encoding: utf8
  reconnect: false
  pool: 5
  database: <%=ENV['OPENSHIFT_APP_NAME']%>
  username: <%=ENV['OPENSHIFT_MYSQL_DB_USERNAME']%>
  password: <%=ENV['OPENSHIFT_MYSQL_DB_PASSWORD']%>
  socket: <%=ENV['OPENSHIFT_MYSQL_DB_SOCKET']%>
  host: <%=ENV['OPENSHIFT_MYSQL_DB_HOST']%>
  port: <%=ENV['OPENSHIFT_MYSQL_DB_PORT']%>
```
*NOTE: Remember to change adapter and ENV variable to corresponding database your are using. For example, If your are using postgresql then change `<%=ENV['OPENSHIFT_MYSQL_DB_USERNAME']%>` to `<%=ENV['OPENSHIFT_POSTGRESQL_DB_USERNAME']%>`*

**7**. Download your rails application from your github repository.
```
cd railsapp
git remote add upstream -m master [your_git_repo_ssh_url]
git pull -s recursive -X theirs upstream master
```

**8**. Do `git push`. `git push` will initialize the server and trigger the `deploy` script. If everything goes alright, you should see your application running on `http://[your_rails_app_name]-[your_namespace].rhcloud.com`. You can customize your domain name too. Check this [post](http://ror-tech.blogspot.com/2013/04/deploying-rails-application-on-to.html) out to learn details. 

