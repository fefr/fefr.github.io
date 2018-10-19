---
title: 嵌入式linux模拟U盘
date: 2018-05-24 15:34:36
tags: 
    - USB
    - Linux
categories: 
    - Linux
---

又到项目为降成本改方案的时候(ー`´ー)，这次要砍掉WLAN那么原本通过ftp更新程序的方法没得用了，只能另辟路径。因为板子是使用USB口烧录的，那么就能利用这个USB口把某个分区当作U盘挂在PC上更换新程序了欧耶\(^o^)/

系统是openWRT，在命令行切换到项目目录后输入`make menuconfig`打开配置菜单，选择如下：

    Kernel modules  --->
        USB Support --->
            -*- kmod-usb-core
            <*> kmod-usb-f-mass-storage
            -*- kmod-usb-gadget
            -*- kmod-usb-lib-composite

选择`kmod-usb-f-mass-storage`后其他会默认选中，保存后`make`编译下载到板子上。

板子跑起来后向板子输入如下命令：

    insmod /lib/module/3.14.38/configfs.ko      #3.14.38是linux版本号，根据实际情况来
    insmod /lib/module/3.14.38/libcomposite.ko
    insmod /lib/module/3.14.38/usb_f_mass_storage.ko
    insmod /lib/module/3.14.38/g_mass_storage.ko file=/dev/mtdblock5 removable=1

