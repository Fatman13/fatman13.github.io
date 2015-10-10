---
layout: post
title: "最近写的一个小项目"
date: 2015-10-10 21:50
comments: true
categories: 
- 编程
- 技术
---
最近前同事`wgs`在谷歌搜索，搜到了我的一篇博客。博主大受鼓舞，连发一篇。最近做了一个实现性的3d模型展示的小项目。在[这里](https://github.com/Fatman13/bim)。项目主要为使用`three js`基于`webGL`和`html5 canvas`的一个实验性`obj`模型展示demo，主要实现了`obj`模型加载，旋转和放大缩小等等一些功能。现分享一些学习经验。

<!--more-->

# 实际效果

实际在我windows phone 8.1手机上用IE打开的运行效果如下。

![](http://i.imgur.com/fPA9Ihw.jpg)

再来一发。

![](http://i.imgur.com/kUoHSWU.jpg)

# 主流框架

现浏览器3d模型展视主流框架有**免费开源**的有像`mrdoob`先生的`three js`，微软的`babylon js`，等等。免费使用但是可能卖钱就要收费的有`unity web player`，`play canvas`，等等。还有需要收费功能更丰富的`Autodesk`的`A360`，更有一些国产`GIS`系统，如`超级地图`，等等。本demo使用了`three js`，是一个相对比较轻量级的`js`引擎。入门相对较快。

# 主要学习材料

- `three js` [主页](http://threejs.org/)里面的`example`
- `three js`的[Cook book](http://www.smartjava.org/content/all-80-recipes-threejs-cookbook-online)，非常实用，博主买了一本
- `StackOverflow`，`three js`2大贡献者`mrdoob`和`WestLanly`经常会亲自在里面回答问题，灰常亲切。
