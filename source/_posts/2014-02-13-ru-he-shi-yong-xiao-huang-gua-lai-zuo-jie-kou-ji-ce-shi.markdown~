---
layout: post
title: "如何使用小黄瓜来做接口级自动化测试"
date: 2014-02-13 21:08
comments: true
categories: 
- 技术
- Ruby
- Cucumber 
---
本文将简单介绍如何使用`小黄瓜`来实现接口级别的自动化测试。如果你还没有对小黄瓜有初步认识的话，推荐阅读我之前写的一篇[文章](http://tauntaunslayer13.me/blog/2014/01/26/ru-he-shi-yong-capybarahe-cucumbershi-xian-ye-mian-ji-bie-zi-dong-hua/)。

<!--more-->

# 运行自动化

下载代码，解压，打开命令行，切换到解压后的文件夹中，运行`bundle install`。  
运行`cucumber features/wgpc_check_kai.feature`，运行成功的话，应该能看见如下输出。

![](http://i.imgur.com/ghYr3bJ.jpg)

以及其他的一些case等等...

# feature文档

用`小黄瓜`写测试case的第一步就是写case的feature文档。  
例如：`cuke_oc\features\wgoc_check_kai.feature`。  
`小黄瓜`的feature文档使用[Gherkin](https://github.com/cucumber/cucumber/wiki/Gherkin)格式来更好的帮你理解，管理和维护你的case。

``` cucumber
@checkorder
Feature: Check Order
  为了保证订单的准确性，在提交订单之前，对订单进行验对。

  Background:
    Given 我根据"./data/wgoc_cases/shipping_region.txt"设置配送范围

  Scenario Outline: 验单逻辑
    Given 我根据 <mprpc_data_file> 文件中的配置建立运费信息
    And 我根据 <mock_data_file> 文件中的配置制造mock商品信息
    And 我从 <request_data_file> 文件中读取某http请求
    When 我用post方法发送该请求至oc的话
    Then 我将得到与 <response_data_file> 文件中相同的json串

  Examples: case_1: 默认全国10块, 非促销商品验单成功（购买1件&支持配送<上海&北京&天津>&未绑定运费模板）
    | mprpc_data_file | mock_data_file | request_data_file | response_data_file |
    | empty.json | check_mock_1.yaml | check_request_1.yaml | check_response_1.json |
```
- `Feature`关键词：简单介绍你要测试的feature。`Feature: Check Order`的下一行可以跟任意长度的更为详细的功能摘要。一个`Feature`能包含多个`Scenario`。（以上例子只有一个`Scenario`）  
- `Backgroud`关键词：定义一个全局的setup的步骤。`Background`对应的代码，整个feature只运行一次。
- `Scenario Outline`关键词：配合`<>`括号内的变量与`Examples`关键词，用`Examples`下面的表格中定义的具体数值来对应到`<>`括号中的变量中的位置。（以上面为例，当这个`Scenario`被执行的时候，`<mprpc_data_file>`这个参数的数值，就会被`Examples`中，`mprpc_data_file`栏中的`empty.json`替代。）  
- `Given`关键词：定义case执行时所要做的一些准备工作。  
- `When`关键词：定义case执行时所要做一个关键动作。  
- `Then`关键词：定义case执行后，校验工作，一般断言都放在这里。  
- `@checkorder`关键词：其实这个也不能算是关键词，`@checkorder`定义了这个feature会使用名为`@checkorder`的一个hook。hook定义了这个feature在case级别的setup/teardown应该执行哪儿些代码。

# code block生成

有了feature文档以后，运行`cucumber features/wgpc_check_kai.feature`，`小黄瓜`会帮你生成如下各个关键词对应的code block。（在你还没有定义对应的steps.rb才会生成，参见`cuke_oc/features/step_definitions/check_kai_steps.rb`。文件命名关系不大，但是一般以steps结尾。）

![](http://i.imgur.com/JqweeIf.jpg)

复制粘帖以上`小黄瓜`的输出至`cuke_oc/features/step_definitions/foo_steps.rb`然后就可以开始填写代码了。当case运行时，会按序运行各个code block中的代码。使用除了英文以外的语言时，别忘记在文件顶端加上`# encoding: utf-8`

# 代码详解

``` ruby
Given /^我根据 (.*) 文件中的配置建立运费信息$/ do |mprpc_data_file| 
  @fid = 0
  @mprpc_res = nil
  # 从mprpc_data_file文件中读取这个case所需要的运费模板参数
  @mprpc_params = JSON.parse(File.read('./data/wgoc_cases/' + mprpc_data_file)) 
  # 向桩发送请求，生成运费模板
  @mprpc_res = RestClient.post JSONRPC_SEND_POSTAGEAPI_ADDTEMPLATE_URI, @mprpc_params.to_json unless @mprpc_params.empty?
  # 从返回中获得运费模板的id
  if !@mprpc_res.nil? && @mprpc_res.code == 200
    @fid = (JSON.parse(@mprpc_res.body))['result']['result']
  end
end
```

向桩发送请求，并根据`mprpc_data_file`中的参数，生成运费模板。现在`mprpc_data_file`用的是yaml格式。使用了[RestClient](https://github.com/rest-client/rest-client)这个`gem`来发送各种`http`请求。（什么是[gem](http://guides.rubygems.org/)?）

``` ruby
Given /^我根据 (.*) 文件中的配置制造mock商品信息$/ do |mock_data_file| 
  # 从mock_data_file读取mock商品的参数
  File.open('./data/wgoc_cases/' + mock_data_file, 'r') { |file|
      @mock_params = YAML.load(file.read)
    }
  # 把前面mprpc返回的delivery_id设定为mock商品的fid
  @mock_params['queryInfo']['fid'] = @fid unless @fid == 0
  # 向桩发送请求，生成mock商品的kv和queryInfo信息
  RestClient.get SCAFFOLDING_CREATE_KV_URI, :params => @mock_params['kv'] unless !@mock_params.key?('kv') || @mock_params['kv'].nil?
  RestClient.get SCAFFOLDING_CREATE_PRODUCTAPI_QUERYINFO_URI, :params => @mock_params['queryInfo'] unless !@mock_params.key?('queryInfo') || @mock_params['queryInfo'].nil?
end
```

向商品库的桩发送请求，并根据`mock_data_file`中的参数，制造mock商品。现在`mock_data_file`用的是yaml格式。

``` ruby
Given /^我从 (.*) 文件中读取某http请求$/ do |request_data_file|
  # 从request_data_file中读取验单所需的参数信息
  File.open('./data/wgoc_cases/' + request_data_file, 'r') { |file|
      @request_params = YAML.load(file.read)
    }
end
```

读取`request_data_file`文件中定义的请求信息。

``` ruby
When /^我用post方法发送该请求至oc的话$/ do
  # 向oc发送验单请求
  @last_response = RestClient.get CHECK_REQUEST_URI, :params => @request_params
end
```

根据刚刚读取的请求信息，给oc发送验单请求。


``` ruby
Then /^我将得到与 (.*) 文件中相同的json串$/ do |response_data_file|
  # 使用了Rspec提供的should的断言方法
  JSON.parse(@last_response.body).should == JSON.parse(File.read('./data/wgoc_cases/' + response_data_file))
end 
```

跟`response_data_file`中定义的预期的json串作比较。不相同的话就触发断言。

# 总结

有问题可发送邮件至`lengyu@baidu.com`。
