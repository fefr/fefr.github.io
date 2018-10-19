---
title: 修改linux wifi固件路径
date: 2018-06-06 15:29:00
tags: 
    - wifi
    - Linux
categories: 
    - Linux
---

    [   12.186511] CONFIG-ERROR) dhd_conf_read_config: Ignore config file /system/etc/firmware/config.txt
    [   12.196629] dhdsdio_download_code_file: Open firmware file failed /system/etc/firmware/fw_bcm43455c0_ag.bin

系统是openWRT，在开机后运行`ifconfig wlan0 up`命令时显示上边的信息，此时wlan是不能正常运行的。由提示信息知道下载固件失败。
使用ls命令发现并没有`/system/etc/`这个文件夹，经寻找后发现`firmware/*`在`/lib`目录下(前人在不同项目使用相同库留下的坑)。经一番搜索后发现两个地方可以修改这个路径：

    - make kernel_menuconfig
        Device Drivers  --->
            [*] Network device support  --->
                [*] Wireless LAN --->
                    <M> Broadcom FullMAC wireless cards support
                    (/your/path/to/firmware/bin) Firmware path
                    (your/path/to/firmware/NVRAM) NVRAM path

    - target/linux/imx6ul/config-*
        CONFIG_BCMDHD_FW_PATH="/your/path/to/firmware/bin"
        CONFIG_BCMDHD_NVRAM_PATH="your/path/to/firmware/NVRAM"

    备忘：CONFIG_BCMDHD_NVRAM_PATH在本项目中使用了bcmdhd.cal
以上两种方法均可修改，当两种方法都使用时第二种方法可覆盖第一种方法。

