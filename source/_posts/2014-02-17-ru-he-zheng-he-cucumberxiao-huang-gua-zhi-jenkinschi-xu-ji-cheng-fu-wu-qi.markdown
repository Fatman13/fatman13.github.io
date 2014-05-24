---
layout: post
title: "如何整合Cucumber小黄瓜至Jenkins持续集成服务器"
date: 2014-02-17 21:18
comments: true
categories: 
- 技术
- Cucumber
- Ruby
- Jenkins
---
本文将简单介绍如何将`Cucumber`小黄瓜的测试case整合到你的`Jenkins CI服务器`中。

<!--more-->

# 软件安装

以Windows工作机为例  
- JDK或者JRE（记得设置JAVA_HOME这个环境变量）  
- git（如果你已经安装了[RailsInstaller](http://railsinstaller.org/)，里面是包含git的）  
- 下载最新稳定版的[Jenkins](http://mirrors.jenkins-ci.org/war/latest/jenkins.war)

# 本地建立git代码库

下载，解压源代码，打开命令行，cd至刚解压的文件夹中，运行以下命令。

```
git init
git add .
git commit -m "initial commit"
```

# 本地配置和运行Jenkins

- 拷贝`Jenkins`的`war包`至你想要的文件夹，打开命令行，cd至那个文件夹。  
- 运行`java -Dfile.encoding=UTF-8 -jar jenkins.war`。  
- 在网页浏览器中打开`http://localhost:8080/configureSecurity/`，按下图中的配置以后点击`Save`。

![](http://i.imgur.com/ea5IK3k.jpg)

- 打开`http://localhost:8080/pluginManager/available`，勾选`Source Code Management`下的`Git Plugin`和`Build Tools`下的`Rake plugin`。点击`Download now and install after restart`。
- 等待Jenkins安装插件，失败的话再重新装。安装完成后`ctrl+c`杀掉进程，并重新运行`Jenkins`。
- 打开`http://localhost:8080/view/All/newJob`。填入你想要的Job名称，选择`Build a free-style software project`。点击`OK`。
- 打开`http://localhost:8080/job/[job名称]/configure`。
- 在`Source Code Management`下，选择`Git`。并在`Repository URL`中填上小黄瓜case所在的文件夹地址。如下图。

![](http://i.imgur.com/OVjhttE.jpg)

- 在`Build Triggers`中勾选`Trigger builds remotely`。填一个你喜欢的token。这样这个job就可以被远程执行了。以下图为例，在浏览器中输入`http://localhost:8080/job/wg_merchant_oc_regression/build?token=lengyu`就能执行这个job了。

![](http://i.imgur.com/YhfCOzS.jpg)

- 在`Build`下，点击`Add Build Step`，并选择`Invoke Rake`。点击`Advanced`。配置如下图。`features`是一个`rake task`，需要在前面`Repository URL`目录中配置`Rakefile`，参见上传的代码。

![](http://i.imgur.com/V7AfNPc.jpg)

# 总结

在`git plugin`和`rake plugin`的帮助下，轻轻松松就能将小黄瓜整合到`jenkins`中。如有问题可发送邮件至[lengyu@baidu.com](mailto:lengu@baidu.com)。

