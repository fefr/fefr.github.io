---
title: tslib
date: 2018-05-30 17:10:41
tags: 
    - USB
    - Linux
categories: 
    - Linux
---

    sudo apt-get install libtool automake autogen autoconf libsysfs-dev
    git clone https://github.com/kergoth/tslib.git
    cd tslib
    echo  "ac_cv_func_malloc_0_nonnull=yes"  > tmp.cache
    ./autogen.sh
    ./configure --host=arm-linux-eabi --cache-file=tmp.cache   --prefix=/opt/tslib CC=/opt/gcc-linaro-7.2.1-2017.11-x86_64_arm-linux-gnueabi/bin/arm-linux-gnueabi-gcc
    make  
    sudo make install  
