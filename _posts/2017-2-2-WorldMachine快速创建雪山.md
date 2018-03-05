---
layout:     post
title:      ProjectWing2 场景优化
subtitle:   场景优化
date:       2017-02-16
author:     BY
header-img: img/post-bg-debug.png
catalog: true
tags:
    - Mac
    - 终端
    - Git
---


# ProjectWing2性能优化
根据要求此文档记录ProjectWing2项目 性能优化历程：
## 优化前分析
ctril + shift + ，打开GPU检测工具对ProjectWing2项目检测。
平行光作为太阳在400000*400000*100000的 LightmassImportanceVolume作用下，发现Scene中ShadowDepths的消耗是最大的。所以这个项目中阴影和效率有很大的关系。普通来说不开灯光的阴影比开的阴影要便宜20倍左右，尤其是当你的物件数量都是一个倍乘的系数越多就越耗性能。继续跟进发现shadowDepths下面场景中的树的阴影消耗非常的大。

![](http://mingchuan.wang/img/ProjectWing/2.png)
