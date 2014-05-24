---
layout: post
title: "Cucumber小黄瓜页面级自动化测试入门教程"
date: 2014-03-12 20:44
comments: true
categories: 
- 技术
- Cucumber
- Capybara
---
之前也写了[一篇](http://tauntaunslayer13.me/blog/2014/01/26/ru-he-shi-yong-capybarahe-cucumbershi-xian-ye-mian-ji-bie-zi-dong-hua/)小黄瓜页面级自动化测试的教程，但是感觉写的可能不是最好。再尝试写一篇入门级别的页面自动化教程。源代码在[这里](https://github.com/Fatman13/cuke_midpage)。

<!--more-->

# 自动化测试对象

这段[源代码](https://github.com/Fatman13/cuke_midpage)主要自动化测试了`百度微购`页面上的几个链接是否为死链。

# 使用代码

- 首先`git clone https://github.com/Fatman13/cuke_midpage.git`。
- `cd cuke_midpage`进入刚下载的文件夹中。
- 运行`bundle install`。
- 跑自动化case运行`cucumber`。（你应该能看到小黄瓜打开火狐，并验证给予的链接是否是死链）

# 源代码结构

小黄瓜大致文件结构如下。

```
D:.
└─features
    ├─step_definitions
    └─support
```

- `features`文件夹：主要存放`.feature`文档。是每个测试case的文字解释。  
- `step_definition`文件夹：主要存放`.rb`文件。每个case具体执行的代码，都应该放在这里。  
- `support`文件夹：主要存放`env.rb`。主要定义一个环境变量。所有`require`都可以放在这儿里。因为`小黄瓜`在运行测试前，一定会先运行`env.rb`这个文件。  

如上所示，开发者只需将对应的`.feature`，`.rb`文件等等，按照小黄瓜约定，放入对应的文件夹中。运行`cucumber`时，小黄瓜就会自动在对应的文件夹中找运行case所需的代码。

# feature文档讲解

作为一个[BDD](http://en.wikipedia.org/wiki/Behavior-driven_development)行为驱动测试框架，小黄瓜会逼迫你写一些文档。给测死链case写一个feature文档可能略显`鸡肋`。这里主要是作为一个简单的例子做讲解一下`features/wgmp_link.feature`。

``` cucumber
Feature: 中间页死链
  为了保证中间页没有死链。

  Scenario Outline: 测死链（Capybara）
    When 我访问"http://weigou.baidu.com"并点击<some_link>链接时
    Then 页面状态应该为"200"

  Examples: 点这些
  	| some_link |
  	| 团购 |

  @new	
  Scenario Outline: 测死链（http client）
    When 我访问<some_link>链接时
    Then 我得到的页面状态应该为"200"

  Examples: 点这些
  	| some_link |
  	| http://weigou.baidu.com/ |
  	| http://weigou.baidu.com/topic/food |
  	| http://weigou.baidu.com/topic/beauty |
```

`feature`文档使用的是[Gerkins](https://github.com/cucumber/cucumber/wiki/Gherkin)的语法。以上这篇`feature`文档，使用了`Scenario Outline:`关键词，写了2个case。第一个是使用`Capybara`测死链，第二个是直接使用`http`客户端测死链。`Given`，`When`，`Then`三个关键词可以用来定义一个case所需要走的一个流程。（这个例子中省略了`Given`）这三个关键词后面的话都是可以根据case的需求改写的。`小黄瓜`会用这些话，用正则表达式自动对应到`feature/step_definitions/link_steps.rb`中每句话对应的`code block`中，并在测试时，运行每句话所对应的代码。

# 代码讲解

接下来讲解一下`feature/step_definitions/link_steps.rb`中的代码。

```ruby
# encoding: utf-8

# 使用Capybara

When /^我访问"(.*?)"并点击(.*)链接时$/ do |arg1, arg2|
  # 正则不仅将When关键词的话对应到这个code block中，
  # 还能从那句话中读取参数。
  # arg1的值是"http://weigou.baidu.com"
  # arg2的值是"团购"

  # visit是capybara封装的一个函数。
  # 他会自动打开火狐，并访问arg1。 
  visit arg1
  # find是capybara封装的一个函数。
  # 这里通过html的a元素上的text找到了这个a元素，
  # 并将他存到@elem变量中。
  @elem = find("a", :text => /\A#{arg2}\z/)
end

Then /^页面状态应该为"(.*?)"$/ do |arg1|
  # 点击该链接
  @elem.click
end

# 使用http client

When /^我访问(.*)链接时$/ do |arg1|
  # 由于在feature文档中使用了examples关键词
  # 这里arg1分别对应了
  # "http://weigou.baidu.com/"
  # "http://weigou.baidu.com/topic/food"
  # "http://weigou.baidu.com/topic/beauty"
  puts arg1
  # 直接使用http客户端，向每个arg1发送get请求。
  @response = RestClient.get arg1
end

Then /^我得到的页面状态应该为"(.*)"$/ do |arg1|
  puts arg1
  # 检查页面status是不是200。
  @response.code.to_s.should eq(arg1)
end
```

这里可以清楚的看到，在`feature`文档中，`When`和`Then`后面跟的话，都一一对应了一块代码。在运行自动化case时，他们会按照`feature`文档中每个`Scenario`的顺序，依次执行每一段对应的代码。


# 附录

- 如有问题可以发送邮件至[lengyu@baidu.com](mailto:lengyu@baidu.com)。
- [这个](https://s3.amazonaws.com/macslow/index.html)挺有意思。

<object data="https://s3.amazonaws.com/macslow/clock7.svg?w=3&amp;h=3" id="clock1" type="image/svg+xml"></object>
