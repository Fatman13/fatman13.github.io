---
layout: post
title: "如何整合支付宝到Rails应用中"
date: 2014-06-21 12:38
comments: true
categories: 
- 技术
- Rails
- Alipay
- 支付宝
---
本文将简单介绍如何接入支付宝到你的`Rails4`应用中。本人已知的一些支持接入支付宝的`gem`有[pay_fu](https://github.com/transist/pay_fu)，[activemerchant_patch_for_china](https://github.com/flyerhzm/activemerchant_patch_for_china)等等。但是由于`RoR`目前国内仍然比较非主流，这2个`gem`并没有维护的非常好。本文将介绍如何自己写一个简单接入支付宝的`Rails helper`。（用到了很多[pay_fu](https://github.com/transist/pay_fu)，[activemerchant_patch_for_china](https://github.com/flyerhzm/activemerchant_patch_for_china)中的源代码）

<!--more-->

# 申请支付宝商家服务

在技术集成之前，首先需要申请支付宝[商家服务](https://b.alipay.com/)产品，获取`PID`和`Key`。现在支付宝商家服务最常见的产品包括`双功能收款`和`平台商双功能收款`。二者的主要区别在于`双功能收款`的买家只能对你进行付款，`平台商双功能收款`能让买家直接对入驻你平台的卖家进行付款。审核通过后，就能在[这里](https://b.alipay.com/order/techService.htm?src=nsf05/)找到相关的技术文档，进行技术集成了。

# 支付宝接口调用流程

当用户在你的应用上完成订单，点击例如`去支付`之类的按钮时，按照[支付宝文档](https://b.alipay.com/order/techService.htm?src=nsf05/)中的参数要求生成请求参数，利用`Rails`的 `redirect_to`函数，让用户跳转到`支付宝网关的url+请求参数`。你应用这边的事情就结束了，一切顺利的话，用户跳转到支付宝网关，登录支付宝账号并支付，一笔交易就完成啦！需要注意的是，请求参数中提供了2个可选参数`return_url`和`notify_url`。分别是支付宝交易处理完成后，同步和异步反调请求地址。也就是说，你的应用需要提供2个`url`给支付宝调用，用来通知你交易的状态。如果这2个参数为空，支付宝不会进行反调。所有交易记录都可以在支付宝账号中查看。

# 关于生成签名

你发送给支付宝网关的请求参数，支付宝会通过验证`sign`和`sign_type`这2个必填参数来判断请求的合法性。但是如何生成`sign`和`sign_type`呢？举个例子，假如说现在除了`sign`和`sign_type`我只要传`a=1`，`b=2`，`c=3`这三个参数给支付宝网关。那么sign的值应该如下。（注意：参数需按照字母顺序排序。示例使用`sign_type`为MD5，其他还有`RSA`等等的验证方式。`key`就是之前申请支付宝商家服务时，支付宝给你的一个密钥。）
```
sign      = MD5('a=1&b=2&c=3' + key )
sign_type = 'MD5'
```

# helper函数

首先在helper文件夹下建`alipay`文件夹。这里比较随意，你也可以不建这个文件夹。在`alipay`下建立`gateway_helper.rb`。名字可以随意，`_helper.rb`后缀能保证默认配置下，`Rails`读取这个文件。

```
D:.
├─app
│  ├─assets
│  ├─controllers
│  ├─helpers
│  │  └─alipay
```

# 生成请求参数

在`gateway_helper.rb`中添加如下代码。（其中用到了很多[pay_fu](https://github.com/transist/pay_fu)，[activemerchant_patch_for_china](https://github.com/flyerhzm/activemerchant_patch_for_china)中的源代码）

``` ruby
# encoding: utf-8
require 'digest/md5'

module Alipay #:nodoc:
  module GatewayHelper #:nodoc:

	# 直接叫这个函数，放入参数，就能实现跳转啦
    def alipay_purchase(options={})
	  # 准备请求参数
      query = prep_params(options)
	  # 阿里网关url + 请求参数
      alipay_url = 'https://mapi.alipay.com/gateway.do?' + querify_esc(query)
	  # 页面跳转
      redirect_to alipay_url
    end

    private

    def prep_params(options={})
      query_params = {
        # ===> 配置信息 <===
		#   隐藏隐私信息在配置文件中，配置文件不要用SCM管理。
        :partner => ALIPAY_CONFIG[Rails.env]['pid'],
        :"_input_charset" => 'utf-8',
		# return_url和notify_url暂时不用
        # :notify_url => options[:notify_url],
        # :return_url => options[:return_url],
		# 支付宝商家服务类型，这里是双功能收款。
        :service => 'trade_create_by_buyer',
        # ===> 交易信息 <===
		#   你系统中的订单号，交易成功的订单号再次请求会失败
        :out_trade_no => options[:out_trade_no],
        :price => options[:price],
        :quantity => 1,
        # :seller_email => SELLER_EMAIL,
        :seller_id => ALIPAY_CONFIG[Rails.env]['pid'],
        :payment_type => 1,
		#   题目（你网站名称）
        :subject => options[:subject],
		#   商品清单（我个人理解。。。）
        :body => options[:body], 
        # ===> 物流 <===
        :logistics_type => 'POST',
        :logistics_fee => 0,
        :logistics_payment => 'BUYER_PAY',
        # ===> 用户信息 <=== 
        :receive_name => options[:receive_name],    
        :receive_address => options[:receive_address],    
        :receive_mobile => options[:receive_mobile],    
        :receive_zip => options[:receive_zip],    
        :receive_phone => options[:receive_phone],    
      }
      query_params = Hash[query_params.sort]
	  # sign = MD5('a=1&b=2&c=3' + key )
      query_params[:sign] = Digest::MD5.hexdigest(querify(query_params) + ALIPAY_CONFIG[Rails.env]['key'])
      query_params[:sign_type] = 'MD5'
      query_params
    end 

	def querify(query_hash)
	  # 把哈希变成'a=1&b=2&c=3'，验证md5用
      query_hash.map {|key, value| "#{key}=#{value.to_s}" }.join("&")
    end

    def querify_esc(query_hash)
      # 把哈希变成'a=1&b=2&c=3'，跳转用
      query_hash.map {|key, value| "#{key}=#{CGI.escape(value.to_s)}" }.join("&")
    end	

  end
end
```

这样就`okay`了。下面将简单示范参考调用方法。

# 在routes.rb中 （参考用法）

``` ruby
resources :orders do 
  post 'pay', on: :member
end
```

# 在View中 （参考用法）

``` erb
<%= link_to pay_order_path(@order), class: 'btn-u', method: :post do %>去支付<% end %> 
```

# 在Controller中 （参考用法）

``` ruby
def pay
  alipay_purchase(
      :out_trade_no => @order.id, 
      :price => @order.total_price, 
      :subject => '某某网',
      :body => @order.products,
      :receive_name => @order.name,    
      :receive_address => @order.address,    
      :receive_mobile => @order.cell_phone,    
      :receive_zip => @order.zip,    
      :receive_phone => @order.phone
  )
end
```

# 附录

- 平台商双功能收款接入[文档](http://pan.baidu.com/s/1qWwgs7E)
