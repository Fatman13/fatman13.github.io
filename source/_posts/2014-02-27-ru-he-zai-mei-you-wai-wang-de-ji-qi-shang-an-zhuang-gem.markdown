---
layout: post
title: "如何在没有外网的机器上安装gem"
date: 2014-02-27 20:16
comments: true
categories: 
- 技术
- Ruby
- Bundler
---
本文将简单介绍如何在没有外网的机器上安装`Cucumber`小黄瓜自动化测所需的`gem`库。

<!--more-->

# 1. 软件安装（Jenkins机器）

- 安装`Jumbo`。

```
bash -c "$( curl http://jumbo.baidu.com/install_jumbo.sh )"; source ~/.bashrc
```

- 安装`Ruby 1.9.3`。

```
jumbo install ruby
```

- 安装`svn`。

```
jumbo install subversion
```

# 2. 在无外网的Jenkins服务器（或者Slave机）上安装Gem

- 在jenkins机器上，选择一个合适的文件夹。从`svn`上下载小黄瓜所依赖的`gem`。

```
svn export https://svn.baidu.com/app-test/ecom/shifen/sf-crm/trunk/weigou/cuke_gems/
```

- `cd cuke_gems`至刚刚下载的文件夹中。运行如下指令，会先安装`bundler`这个`gem`。

```
gem install --local bundler-1.3.4.gem
```

- 安装完成后，运行以下指令。

```
bundle install --local 
```

# 3. 添加新的Gem

- 如果需要添加新的gem的话，找一台有外网的机器。（如果Jenkins或者Slave机器是Linux机器，就得用Linux机器，是Windows就得用Windows机器。）
- 先`co`之前的`gem`库。

```
svn co https://svn.baidu.com/app-test/ecom/shifen/sf-crm/trunk/weigou/cuke_gems/
```
- `cd cuke_gems`至刚下载的文件夹中，修改`Gemfile`，添加新的`gem`。（最好能选定`gem`的版本）例如：

``` ruby 
source 'http://rubygems.org'

gem 'json', '1.7.3'
gem 'bundler', '1.3.4'

# 添加新Gem示例 <------
gem 'new_gem', '0.0.1'

group :test do
  gem 'cucumber', '1.2.1'
  gem 'rspec', '2.11.0'
  gem 'httparty', '0.8.3'
  gem 'rest-client', '1.6.7'
end
```
- 添加完成后，运行`bundle install`，然后运行`bundle package`。这样`bundler`就会生成新添加的`gem`的`.gem`文件以提供本地安装了。
- 把更新后的代码提交至`svn`。

```
svn commit -m "add new gem(s)"
```

- 最后重复上文第2大点中的步骤，就能更新Jenkins机器上的`gem`库了。


# 附录

- `bundler`是给`gem`打包的一个`gem`，所以在上文中先安装了`bundler`，然后让`bundler`去本地安装其他的`gem`。
- 更多关于`Gemfile`的使用方法，可以参考[官方网站](http://bundler.io/)。
- 所有`.gem`文件都可以在[rubygems.org](http://rubygems.org/)上下载到。其中包括`gem`的所有历史版本。
- 如有更多问题，发送email至[lengyu@baidu.com](mailto:lengyu@baidu.com)。


