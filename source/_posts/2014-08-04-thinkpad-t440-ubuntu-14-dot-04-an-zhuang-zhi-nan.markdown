---
layout: post
title: "Thinkpad T440 Ubuntu 14.04 安装指南"
date: 2014-08-04 20:38
comments: true
categories: 
- Ubuntu
- Thinkpad
---
前几周买了一台`Thinkpad t440 20B6002XCD`。`新蛋`链接在[这里](http://www.newegg.cn/Product/A36-125-C0K.htm)。记录一下改装配置、简单评测、以及在这台电脑上曲折的安装`Ubuntu`的经历。

<!--more-->

# 我的t440配置

原计划是要陪朋友去买电脑的，后来没想到我自己先买了一台`t440`。徐汇商圈电脑硬件销售竞争非常激烈，销售员也非常主动。最后我把内存加到`8G`（`t440`好像只有一个内存条插槽），把本来的机械硬盘换成了`256G`的固态硬盘。最后价格从`6199元`升至`7800元`。

# t440第一感觉

装好固态硬盘之后非常快。在这里强烈推荐大家升级固态硬盘。预算允许的话，可以尝试纯固态。`t440`的键盘很舒服。触控板有点失望，过于敏感。在使用`Thinkpad`小红点，尝试按下触控板上的鼠标键时，没有像`x220`那样机械按键精准。`Thinkpad`的屏幕工艺比较特别。`AntiGlare`技术感觉黑屏的时候，容易看见屏幕边角区域大片白色光晕（美版好像有升级至`1600x900`分辨率的选项 +$50）。内置3芯电池，更持久续航时间 （Ubuntu识别了第2块电池。但是我有一次没关机，回家后看电脑已经没电，接入电源启动后`Ubuntu`报了错。不清楚是不是`Ubuntu`不知道怎么切换电池？）。整机的做工非常漂亮，而且比一般的`Thinkpad`14寸的机器要轻薄很多。开机时机屏幕背面`Thinkpad`上的小红点会亮，感觉非常炫酷。关于t440更详细的评测，请参见[这里](https://www.youtube.com/watch?v=L6GvbQPwJuo)。

# 安装Ubuntu

本来想试试看`Ubuntu`专为中国用户打造的[麒麟](http://www.ubuntu.com/desktop/ubuntu-kylin)版。后来想了一想还是装普通版吧。不过`金山`，`搜狗`公司等等针对[麒麟](http://www.ubuntu.com/desktop/ubuntu-kylin)版开发的软件值得关注（业界良心？）。

- 下载`Ubuntu 14.04 LTS`的[镜像](http://www.ubuntu.com/download/desktop/)。
- 使用[pendrivelinux](http://www.pendrivelinux.com/)把官网上下载镜像刷到`u盘`上，具体步骤参考[这里](http://www.ubuntu.com/download/desktop/create-a-usb-stick-on-windows)。
- 把`u盘`插入`t440`。重启`t440`，不停点`F1`，进入`BIOS`。
- `startup`--->`Boot`，按`+`（其实按的是`shift + 等于键`）把你插入`u盘`的那个`usb`端口的`Boot`顺序排在第一位。
- `Esc`--->`Exit and save changes`--->`yes`
- 重启后，按照`Ubuntu`的提示一步一步安装。

# 无线网卡问题

问题来了，你也有可能注意到了。`Ubuntu`安装的时候因为没有驱动，无法检测到`t440`的无线网卡。这也印证了一句古话，电脑中`Intel`的芯片越多，就越容易装`Ubuntu`。使用`sudo lshw`或者`lspci -nnk | grep -iA2 net`，发现`t440`的网卡芯片是`RealTek rtl8192ee PCIe`。而且`Ubuntu 14.04`没有对应的驱动。

- 确认路由器的加密方式为`WPA2`。（参考[这里](http://ubuntuforums.org/archive/index.php/t-2003972.html)）
- 根据在`Ubuntu bugs`中[这个](https://bugs.launchpad.net/ubuntu/+source/linux/+bug/1239578)帖子里的讨论，`RealTek`已经把`rtl8192`驱动的代码发给了`Larry Finger`，他已经把驱动合入了`Linux kernel 3.16 rc2`。所以大家可以参考[这里](http://linuxg.net/how-to-install-kernel-3-16-rc2-on-ubuntu-linux-mint-pinguy-os-and-other-ubuntu-derivatives/)的步骤安装新的`Linux`内核。值得一提的是`Ubuntu 14.10 Utopic Unicorn`已经使用`3.16 rc2`为默认内核了。
- 到这里`t440`应该可以连上`Wifi`了。如果还是不行的话可以参考附录2。

# 附录1

> Also go into your router settings and change wpa to just wpa2 if you have that option it works best with ubuntu.
> -- [Wild man](http://ubuntuforums.org/archive/index.php/t-2003972.html)

# 附录2

如何设置[静态ip](http://superuser.com/questions/564519/ubuntu-cant-connect-to-internet)呢？

# 附录3

Install [Sublime Text 2/3](http://askubuntu.com/questions/172698/how-do-i-install-sublime-text-2-3) on Ubuntu. 

# 附录4

关于[Bitcoin](https://www.khanacademy.org/economics-finance-domain/core-finance/money-and-banking/bitcoin/v/bitcoin-overview)。
