---
title: 禁用transparent_hugepage
date: 2016-10-16 20:19:32
tags: 
    - MongoDB
    - Linux
categories: 
    - DataBase
---

运行mongod服务后，log里边会出现这两个提示

    2016-10-01T22:58:07.367+0800 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
    2016-10-01T22:58:07.367+0800 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
    2016-10-01T22:58:07.367+0800 I CONTROL  [initandlisten] 
    2016-10-01T22:58:07.367+0800 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
    2016-10-01T22:58:07.367+0800 I CONTROL  [initandlisten] **        We suggest setting it to 'never'

Transparent Huge Pages(THP)会影响数据库的性能，官方的[禁用THP功能](https://docs.mongodb.com/manual/tutorial/transparent-huge-pages/)的方法。

但官方的方法比较麻烦，我在网上找到例外一个比较[简单的方法](http://www.bictor.com/2015/07/19/disabling-transparent-hugepages-in-ubuntu-14-04-server-lts/)，编辑`/etc/default/grub`，在`GRUB_CMDLINE_LINUX_DEFAULT`中加入`transparent_hugepage=never`,即

    GRUB_CMDLINE_LINUX_DEFAULT='quiet splash transparent_hugepage=never'

然后运行`sudo update-grub`后重启即可，经验证后mongod的waring没有了。修改grub增加内核启动参数参考[这里](https://askubuntu.com/questions/19486/how-do-i-add-a-kernel-boot-parameter)。
至于这样修改是否有副作用，暂不清楚。



