---
layout: post
title: "写给毛藏的使用指南"
date: 2013-09-03 16:46
comments: true
categories: 
- 技术
---
写给毛藏的使用指南。

<!--more-->

# 如何安装

**1**. 下载并安装[RailsInstaller](http://files.rubyforge.vm.bytemark.co.uk/railsinstaller/railsinstaller-2.2.1.exe)。

**2**. 安装完成之后，应该会有一个`命令行`跳出来。输入以下指令。

```
git clone https://github.com/Fatman13/MaoZangLED.git
cd MaoZangLED
bundle install
rake db:migrate
```

*注：如果有除了c盘以外其他分盘的话需要替换*`bundle install`*为*`bundle install --path .bundle`*。好像sass在windows上有bug。*

**3**. 至此安装完成。运行`rails s`，使用本地服务器。

**4**. 打开浏览器。输入地址`localhost:3000/products`。

# 如何打开服务器

**1**. 打开命令行

```
cd c:\Sites\MaoZangLED
rails s
```

# 如何更新

**1**. 使用以下命令更新代码。

```
cd c:\Sites\MaoZangLED
git fetch --all
git reset --hard origin/master
bundle install --path .bundle
rake db:migrate
```
