---
title: screen_mess
date: 2016-10-19
tags:
    - linux
    - screen
categories: 
    - Linux
---
##### 起因
我的系統是Windows 10，后来装了Linux Mint 18组成双系统后在进入win10时都会
花屏一段时间后才能开始进入win10的启动画面，而且有时还会花屏很久死机，
死机现象特别容易出现再需要使用’禁止强制驱动签名’功能重启时。

##### 原因
这篇[博客](http://www.cnblogs.com/litengyao/p/4630889.html)
提到因为linux只有集显驱动，系统启动时先加载Linux的显卡驱动来加载
选择系统的界面，导致进入Windows花屏。

##### 解决办法
在Linux系统下运行`sudo vim /etc/default/grub`后取消掉`GRUB_TERMINAL`的注
释，即：

    # Uncomment to disable graphical terminal (grub-pc only)
    GRUB_TERMINAL=console
然后记得运行`sudo update-grub`后才能正式生效。

##### 总结
至此已解决Linux和Windows双系统时进入Windows会花屏的问题，修改`grub`后一定要
记得执行`update-grub`才能正式生效，刚开始忘记了瞎搞了好一会，在花屏时还死机
了。虽然选择系统时界面时难看了点，但总比花屏还可能死机强多了。