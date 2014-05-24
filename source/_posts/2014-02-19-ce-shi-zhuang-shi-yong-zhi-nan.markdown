---
layout: post
title: "测试桩使用指南"
date: 2014-02-19 21:35
comments: true
categories: 
- 技术
- Python
- Django
---
本文将简单介绍`测试桩`现有的一些功能以及如何使用`测试桩`提供的`API`接口。

<!--more-->

# 测试桩简介

现在测试桩使用`Django`框架，大致分为`mysite`，`jsonrpc`，`scaffolding`这3个模块。  
（注：在项目的根目录使用了`python manage.py startapp [模块名]`这个命令创建了这些模块，模块化`Django`提供的服务）

- `mysite`是项目的第一个模块。主要提供了kv以及商品库的几个接受jsonrpc请求的接口。`mysite`下的`urls.py`是定义`测试桩`各项API接口url的主入口。  
- `jsonrpc`提供了几个发送jsonrpc请求的接口（mcpack封装），还有发送通用的jsonrpc请求的接口。（无mcpack封装，vanilla版jsonrpc请求）  
- `scaffolding`主要负责提供制造各种mock商品的接口，便于自动化case的setup中，制造case所需的mock商品。  

# Chrome插件

使用`chrome`的`cRest Client`插件可以手动向`测试桩`发送各种请求。

![](http://i.imgur.com/oJEztF2.jpg)

# Jsonrpc工具

输入`测试桩`商品库地址`http://st01-ecom-jn2.st01.baidu.com:8890/serviceAPI/api`，选择api，method，并填入请求参数就可以手动给`测试桩`发送jsonrpc请求了。jsonrpc工具可在附件中下载。

![](http://i.imgur.com/ALJTrbi.jpg)

# 1. Mysite接口 

（可试用jsonrpc工具手动访问这些接口，或者使用renliang开发的jsonrpc-client）

### 1.1 ProductAPI/queryInfo 获取商品详情

- http://st01-ecom-jn2.st01.baidu.com:8890/serviceAPI/api
- ProductAPI
- queryInfo
- {"products":["183019616"],"uc_id":29844}

### 1.2 PromotionAPI/query 获取促销详情

- http://st01-ecom-jn2.st01.baidu.com:8890/serviceAPI/api
- PromotionAPI
- query
- {"promotion_id":"3000","uc_id":29844}

### 1.3 StockAPI/updateIncrement 增减库存

- http://st01-ecom-jn2.st01.baidu.com:8890/serviceAPI/api
- StockAPI
- updateIncrement
- {"uc_id":6195321,"products":[{"product_id":"600528576","items":[{"stock":-2,"region":"全国"}]}]}

### 1.4 PromotionAPI/updateStockInc 增减促销库存

- http://st01-ecom-jn2.st01.baidu.com:8890/serviceAPI/api
- PromotionAPI
- updateStockInc
- {"uc_id": 29844,"product_outerid":"F47","promotion_id":37,"stock_inc":5}

# 2. kv接口

（kv接口主要应用于验单时，查询该商品是否存在。新kv接口是一个新的技术优化，与老kv接口还并存着。。。）

### 2.1 新kv接口

http://st01-ecom-jn2.st01.baidu.com:8890/serviceAPI/midpage/product/details?ids=183019611  

### 2.2 老kv接口

http://st01-ecom-jn2.st01.baidu.com:8890/api/midpage/product/details?ids=183019611  

# 3. Scaffolding接口

（主要用于各种商品库接口的mock数据的制造，便于自动化case的setup。）

### 3.1 制造kv接口的返回值

- http://st01-ecom-jn2.st01.baidu.com:8890/scaffolding/create/kv?product_id=77582589&stock=0&promotionId=1013
- (可以跟更多参数。参照`mysite/response/new_KV/new_kv_template.json`。)

### 3.2 制造老kv接口的返回值（老kv）

- http://st01-ecom-jn2.st01.baidu.com:8890/scaffolding/create/oldkv?product_id=77582589&merchantId=298440&newPrice=100.00  
- (可以跟更多参数。参照`mysite/response/KV/kv_template.json`。)

### 3.3 制造queryInfo接口的返回值

- http://st01-ecom-jn2.st01.baidu.com:8890/scaffolding/create/ProductAPI/queryInfo?product_id=77582589&active=0&fid=1157  
- (可以跟更多参数。参照`mysite/response/ProductAPI/queryInfo/queryInfo_template.json`。)

### 3.4 制造query接口的返回值

- http://st01-ecom-jn2.st01.baidu.com:8890/scaffolding/create/PromotionAPI/query?product_id=77582589&active=0&fid=1157  
- (可以跟更多参数。参照`mysite/response/PromotionAPI/query/query_template.json`。)

# 4. Jsonrpc接口

（用post方法，带着参数，访问这些接口。`测试桩`会代为发送jsonrpc请求，相当于一个接口级的jsonrpc工具。推荐使用上文提到的Chrome中`cRest Client`这个插件。使用post方法的话，Request entity中放请求的json串，或者其他参数。）

### 4.1 vanilla示例（无mcpack封装，支持大多数jsonrpc的api）

- http://st01-ecom-jn2.st01.baidu.com:8890/jsonrpc/vanilla/ProductAPI/queryInfo
- {"products":["183019611"],"uc_id":29844}

### 4.2 jsonrpc示例（有mcpack封装，只支持个别jsonrpc的api）

#### 4.2.1 OrderInnerAPI/daigouCreateOrder（代购api）

- http://st01-ecom-jn2.st01.baidu.com:8890/jsonrpc/send/OrderInnerAPI/daigouCreateOrder
- {"daigouOrder":{"daigouId":1,"passportId":"1245","productIds":"181835090","productCounts":"1","customer":"木根","mobile":"12383846326","payStyle":"DAIGOU","province":"山东","city":"济南市","district":"大观区","town":"白贤镇","districtId":"120001","detailAddress":"龙山路22号","needInvoice":"true","invoiceTitle":"百度中国"},"token":"daigou_token_26e588a503074"}

#### 4.2.2 PostageAPI/addTemplate（添加运费模板api）

- http://st01-ecom-jn2.st01.baidu.com:8890/jsonrpc/send/PostageAPI/addTemplate
- {"name":"运费模板名称","ucId":29844,"outerId":null,"assumer":"SELLER","valuation":"QUANTITY","consignAreaId":"110101","lastModify":"2014-01-02 19:06:01","shippingMethodList":[{"name":"COD","defaultStartStandards":"1","defaultStartFees":"10.00","defaultAddStandards":"1","defaultAddFees":"4.00","regionGroupList":[]}]}

#### 4.2.3 PostageAPI/deleteTemplate（删除运费模板api）

- http://st01-ecom-jn2.st01.baidu.com:8890/jsonrpc/send/PostageAPI/deleteTemplate
- 109

#### 4.2.4 DeliveryRegionsAPI/updateDeliveryRegions（更新配送范围api）

- http://st01-ecom-jn2.st01.baidu.com:8890/jsonrpc/send/DeliveryRegionsAPI/updateDeliveryRegions
- 0

# 总结

如有问题可以发送邮件至[lengyu@baidu.com](mailto:lengyu@baidu.com)。
