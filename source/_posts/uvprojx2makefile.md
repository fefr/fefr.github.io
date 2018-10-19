---
title: 把uvprojx转换成makefile
date: 2018-04-25 16:08:10
tags:
    - CI
    - Makefile
categories: 
    - CI
---

以NRF52832为例

![output](https://i.loli.net/2018/04/25/5ae03dbec8d00.png)

![debug](https://i.loli.net/2018/04/25/5ae0412816468.png)

用于生成调试信息的参数是`-g`，在`.uvprojx`里`<DebugInformation>`为1时会使用这个参数，0时不使用。

![browse](https://i.loli.net/2018/04/25/5ae04198d8bf2.png)

同样是`armcc`但好像会有两个版本或以上，如ARM Compiler for μVision的`armcc`有一点特殊的命令`--omf_browse`。在`.uvprojx`里`<BrowseInformation`为1时会使用这个参数，0时不使用。

![micro lib](https://i.loli.net/2018/04/25/5ae0425fdb924.png)

![define](https://i.loli.net/2018/04/25/5ae0436df30f4.png)

当勾选Use Micro LIB时会使用`-D__MICROLIB`这个宏定义。

当Floating Point Hardware是Not Used时对应`.uvprojx`里的`<RvdsVP>1</RvdsVP>`且`armcc`的编译参数变为`--cpu Cortex-M4`，Use Single Precision对应的是`<RvdsVP>2</RvdsVP>`且`armcc`的编译参数变为`--cpu Cortex-M4.fp`。

![C/C++](https://i.loli.net/2018/04/25/5ae044ae09b97.png)

上图中的Optimization下拉选项是优化级别即`-O`参数，<default>对应`.uvprojx`里的`<Optim>0</Optim>`，`-O0`对应的是`<Optim>1</Optim>`，以此类推。

当勾选One ELF Section per Function时会使用参数`--split_sections`，对应`.uvprojx`里的`<OneElfS>1</OneElfS>`，不勾选时对应的是`<OneElfS>0</OneElfS>`。

当勾选C99 Mode时会使用参数`-c99`，对应`<uC99>1</uC99>`，不勾选时对应`<uC99>0</uC99>`。

至于`--apcs=interwork`参数无法在`.uvprojx`找到对应的tag，但是根据ARM官方文档ARMv5T后都默认使用interwork了。
