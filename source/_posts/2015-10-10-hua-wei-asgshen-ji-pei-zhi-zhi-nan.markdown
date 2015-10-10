---
layout: post
title: "华为ASG审计配置指南"
date: 2015-10-10 21:50
comments: true
categories: 
- IT
---
继续受到前同事`wgs`的鼓舞。再来一发。本文将简单介绍华为`ASG`上网行为管理的上网审计功能配置。

<!--more-->

# ASG介绍

华为ASG是华为上网行为管理的一款产品。它同时也集成了部分防火墙和路由器的功能，如中小企业直接用ASG感觉也是阔以的。本文中的配置适用于华为`ASG 2200`款，针对部门进行审计。

# 审计配置

- 打开浏览器，输入`ASG`的ip地址，进入到`ASG`的web管理界面，并以管理员身份登陆。
- 用户管理（左侧导航）-> 部门/用户-> 新建 （下图）
![](http://i.imgur.com/04NSLrB.jpg)
- 填入适当的`部门名称`和`描述`-> 点击确定
- 认证策略-> 填写名称，描述 -> 在新用户认证规则中，点选`添加到指定的部门、组中`，并选择刚刚添加的部门-> 点击确定
![](http://i.imgur.com/Tq4b24Q.jpg)
- 管理策略（左侧导航）-> 上网策略-> 新建-> 填写名称，描述，并选择需要的审计策略配置
![](http://i.imgur.com/1zbSCja.jpg)
- 切换到部门和用户标签页-> 添加需要审计的部门-> 点击确定
![](http://i.imgur.com/tlw9lmF.jpg)
- 系统（左侧导航）-> 配置-> ASG管理中心-> 配置ip，端口和共享密钥 （注：在网络中随便一台PC安装华为光盘中的ASG manager，只要那台PC能够ping通ASG，就能作为ASG管理中心使用）
![](http://i.imgur.com/wxjUO6F.jpg)
- 在浏览器中，登录ASG管理中心的web界面-> 设备标签页-> 设备管理-> 设备列表-> 添加-> 输入ASG 2200的ip地址和之前配置的共享密钥-> 点击确定
![](http://i.imgur.com/6FrVtwk.jpg)
- 采集器管理-> 添加-> 填写采集器名称和ip地址-> 点击确定 （注：在网络中随便一台PC安装华为光盘中ASG logger，只要那台PC能够ping通ASG管理中心，就能作为日志采集器使用）
![](http://i.imgur.com/nPZORo4.jpg)
- 点击操作栏中第一个图标-> 添加-> 选取刚刚添加的ASG设备。
![](http://i.imgur.com/J8Pdiu2.jpg)
- 报表标签页-> 普通报表任务-> 创建 （注：这样就能每天生成报告并发送至指定邮箱了）
![](http://i.imgur.com/NzKDvtd.jpg) 

# 附

生成报告的截图，仅供参考。

![](http://i.imgur.com/L5TngTn.jpg)

华为ASG能号称监控QQ，赶脚挺厉害的。。。

![](http://i.imgur.com/gLvAPe7.jpg)
