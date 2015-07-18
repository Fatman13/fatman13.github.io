---
layout: post
title: "如何使用Outlook链接Zimbra邮箱"
date: 2015-07-18 13:10
comments: true
categories: 
- IT
---
本文简单介绍如何使用`Outlook`客户端连接`Zimbra`邮箱系统。原文在[这里](http://www.upenn.edu/computing/email/help/zimbra/outlook/outlook_config.html)。OP还是比较推荐使用`Zimbra`的`Web`客户端。

<!--more-->

# 连接Outlook步骤

1 `工具` -> `账户设置`

![](http://i.imgur.com/Ez7Tcje.jpg)

2 点击`新建`。

![](http://i.imgur.com/XySXE0u.jpg)

3 点击`下一步`。（Microsoft Exchange，POP3，IMAP或HTTP）

![](http://i.imgur.com/Np7kAk1.jpg)

4 勾选`手动配置服务器设置或其他服务器类型`。

![](http://i.imgur.com/87QVOQd.jpg)

5 点击`下一步`。（Internet电子邮件）

![](http://i.imgur.com/wIkOsur.jpg)

6 您的姓名：你的姓名，电子邮件地址：xyz@shtowercbre.com，账户类型：POP3，接受邮件服务器：mail.shtowercbre.com，发送邮件服务器：mail.shtowercbre.com，用户名：你的用户名，登陆密码：你的登陆密码。

![](http://i.imgur.com/Bdtb1nn.jpg)

7 点击`其他设置` -> `发送服务器` -> 勾选`我的发送服务器需要验证`。

![](http://i.imgur.com/Umk1IjM.jpg)

8 还是在`其他设置` -> `高级` -> 勾选`此服务器要求加密连接（SSL）`，发送服务器：25,使用以下加密连接类型：TLS。

![](http://i.imgur.com/HLvkF7y.jpg)

8.5 如果需要在服务器上保存邮件副本的话，需更改`Outlook`默认设置。在上文第7步中，`其他设置` -> `高级` -> 勾选`在服务器上保留邮件副本`。

![](http://i.imgur.com/aA284uj.jpg)

9 点击`确定`，点击`测试账户设置`。

![](http://i.imgur.com/8BANwip.jpg)

# 日常维护小技巧

1. 添加完新成员后，新邮件填写收件人时，没有自动补完新成员名字？`配置`->`域名`->双击右边的域名->`GAL`->`内部GAL轮询间隔时间`改为1分钟或者更低。稍等片刻后，新成员的名字应该就可以自动补完了。
2. 如何添加会议室？`管理`->`资源`->点击右上角齿轮图标->`新建`。（注意：添加完成后需要GAL更新后才会在用户的新约会中生效）
3. 使用多个身份发送邮件？`管理`->选择用户->给该用户添加`别名`。（注意：添加完成后需要GAL更新后别名才会生效）登录该用户账号，`首选项`->`账户`->`添加身份`->填入`帐户名称`->填入`发件人：选择邮件发件栏中显示名称`->下拉菜单中选择新添加的别名->点击左上角`保存`。这样该用户在写新邮件时就可以选择使用不同身份发送了。

# 附 1

之前公司网络神秘得把TLS加密给屏蔽了。后来经过多方排查，原来是公司网络行为管理服务器，华为ASG2200，中的`病毒过滤`功能启用后，把TLS过滤掉了。直接禁用`病毒过滤`后，问题解决。

# 附 2

`Zimbra`管理员操作[手册](https://www.zimbra.com/docs/os/8.6.0/administration_guide/wwhelp/wwhimpl/js/html/wwhelp.htm#href=860_admin_os.Zimbra_Collaboration.html)。
`Zimbra`安装[手册](https://www.zimbra.com/docs/ne/8.6.0/single_server_install/wwhelp/wwhimpl/js/html/wwhelp.htm#href=860_SingleServer_Install_NE.Performing_a_Single_Server_Installation.html)。

# 附 3

华为的一些免费网络[课程](http://support.huawei.com/learning/nodeQueryAction!loadTrainProjectInfo?lang=zh&pbiPath=&courseId=Node1000007759)。






