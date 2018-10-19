---
title: linux BBT
date: 2018-05-11 13:46:39
tags: 
    - BBT
    - Linux
categories: 
    - Linux
---

最近项目中遇到一个很奇怪的问题（如下图），一开始以为是NAND Flash出现的坏块，然后发现好几个板子也出现同样的问题就开始怀疑是分区表大小超出实际大小。
![badblock](https://i.loli.net/2018/05/11/5af5325ed283b.jpg)

分区大小在dts文件中可以查询得到，排查后发现不是这个原因导致。接着只能看看dts中关于分区的其他[参数意义](https://www.kernel.org/doc/Documentation/devicetree/bindings/mtd/gpmi-nand.txt)，留意到其中的`nand-on-flash-bbt`，字面意思就是坏块管理表（Bad Block Table, BBT)保存在flash中。既然要保存到flash那么就肯定要消耗flash的空间，经过一番google后终于找到问题所在。
参考[这里](http://cmchao.logdown.com/posts/61616-nand-flash-support-in-linux)留意到图上maxblocks占用了最后4个block用作BBT，这不就和出现的问题符合嘛。本着
认真负责的态度去查看我们的代码，根据那篇文章中图片的提示搜索bbt_main_descr(KDE的Dolphin真好用哈哈，可以搜索文件夹内的所有文件的内容，又快又好)，在linux内核下的`/drivers/mtd/nand/nand_bbt.c`下可以找到：

    static struct nand_bbt_descr bbt_main_descr = {
        .options = NAND_BBT_LASTBLOCK | NAND_BBT_CREATE | NAND_BBT_WRITE
            | NAND_BBT_2BIT | NAND_BBT_VERSION | NAND_BBT_PERCHIP,
        .offs = 8,
        .len = 4,
        .veroffs = 12,
        .maxblocks = NAND_BBT_SCAN_MAXBLOCKS,
        .pattern = bbt_pattern
    };

`NAND_BBT_LASTBLOCK` 和 `NAND_BBT_SCAN_MAXBLOCKS`均在`/include/linux/mtd/bbm.h`下定义，`NAND_BBT_LASTBLOCK`表示BBT在flash最后的几个block，`NAND_BBT_SCAN_MAXBLOCKS`被定义为4刚好和出现的问题现象相符合。
折腾了几天终于找到问题所在了。。并且最后在一篇[文档](http://www.linux-mtd.infradead.org/tech/mtdnand/x144.html)中找到这个描述:

> - Store bad block table per chip
- Use 2 bits per block
- Automatic placement at the end of the chip
- Use mirrored tables with version numbers
- Reserve 4 blocks at the end of the chip