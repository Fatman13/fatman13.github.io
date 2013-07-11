---
layout: post
title: "使用Octopress教程大合集"
date: 2013-06-24 12:11
comments: true
categories: 技术
---
小的消息闭塞，最近才开始使用`Octopress`。在这儿整理下各种实用的教程，以供大家参考！

<!--more-->

**1** 首先是官方`Octopress`教程在[这里](http://octopress.org/docs/)。

**2** 使用自定义域名在[这里](http://learnaholic.me/2012/10/10/deploying-octopress-to-github-pages-and-setting-custom-subdomain-cname/)。

**3** 使用`Octopress`各种皮肤在[这里](https://github.com/imathis/octopress/wiki/3rd-Party-Octopress-Themes)。

**4** 使用`greyshade`皮肤加上新浪微博在[这里](http://imallen.com/blog/2013/05/12/add-support-for-weibo-and-dribbble-to-greyshade.html)。

*如果你先装`greyshade`再装这个的话，代码会有点小错，`source/_includes/header.html`里面的以下这句注释掉。否则在右边会出现2遍博客名和简介。*
```
% include custom/header.html %
```
*在`_config.yml`里面设置`weibo_user:`的时候，别忘了在微薄里面设置好自己在微薄上的域名。否则会发生找不到id的情况。*

**5** `Octopress`中的表格没有边框？！请看[这里](http://programus.github.io/blog/2012/03/07/add-table-data-css-for-octopress/)。

**6** 在不同电脑上用`Octopress`写博客。请看[这里](http://blog.zerosharp.com/clone-your-octopress-to-blog-from-two-places/)。

**7** 如果你有很多博文，如何只生成最新写的那篇。参看[这里](http://robdodson.me/blog/2012/06/11/some-octopress-rake-tips/)。

**8** 如何删除一片博文在[这里](http://stackoverflow.com/questions/16762325/how-to-rename-or-delete-a-new-octopress-post)。

**9** 如何在`Octopress`里面增加自己的`About`页面在[这里](http://gangmax.me/blog/2012/05/04/add-about-page-in-octopress/)。

**10** 使用`rake gen_deploy`和`rake preview`之后可以在本地`localhost:4000`预览还没有上传的博文。修改博文后，`rake`会自动重新生成，刷新本地页面就能浏览更新的东西了。

**11** Octopress各种实用小技巧，包括修改字体，颜色，favicon等等。请看[这里](http://colors4.us/blog/2012/01/08/octopressbo-ke-cong-ling-kai-shi-iii/)

最后奉上能快速上手markdown的[作弊小抄](http://support.mashery.com/docs/customizing_your_portal/Markdown_Cheat_Sheet)。这里有更高阶的[作弊小抄](http://daringfireball.net/projects/markdown/syntax)

先写这点，以后有好de发现继续更新。:-)
