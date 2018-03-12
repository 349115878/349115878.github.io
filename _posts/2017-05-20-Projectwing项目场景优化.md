---
layout:     post
title:      ProjectWing 性能优化
subtitle:   性能优化
date:       2017-5-20
author:     Trump
header-img: img/post-bg-debug.png
catalog: true
tags:
- UE4
- 性能优化
---

# ProjectWing 性能优化

## ProjectWing 项目介绍
Project项目是为意大利 Agusta-westland 直升机厂商定制的 AW609型号直升机虚拟现实项目展示。

![](http://mingchuan.wang/img/ProjectWing/4.png)

官方网站： https://www.windriver.com/customers/customer-success/aerospace-defense/agusta-westland/

本项目由我和另一个同事合作完成，我主要负责飞机飞行模拟逻辑实现，舱内交互动画实现，Sequence动画实现，Bug修复，场景性能优化，VR功能实现。
项目的场景是用UE4官方Demo（用户也说OK），当时是2016年的11月份接到这个项目，那时我还没用过Worldmachine，在经历这个项目之后意识到一个高端场景对于这样的项目是多么的重要，从那开始了Worldmachine之旅，像喝了药一样（虽然我没喝过）停不下来了...

Demo视频展示(pc演示效果更好)： https://pan.baidu.com/s/1AUowCtVkLiLgB__AddRU1A

本文只介绍场景性能优化方面的经历。

## 概述

性能优化不是一成不变，根据优化的项目需求，平台资源的不同将会有所侧重，同时帧数过高过低都不是目标，这都需要根据项目的需要而定。ProjectWing这个项目定义为飞行演示，所以美术效果表现是首要的，其次指定的PC平台上帧数控制在30帧以上，演示的时候没有特别明显的卡顿就可以了。

首先明确优化为了达到Fps目标，利用毫秒描述可以更好的量化，更直观，比如：
该表显示了平均 fps 和 ms之间的关系

![](http://mingchuan.wang/img/ProjectWing/5.png)

## 启动项目
- 烘焙资源（烘焙后资源将放在Game/Save/下，资源数据被引擎优化处理，防止被不必要的资源影响）；
- 设置为独立应用运行 (PIE)，这样可以让编辑器菜单逻辑和其他代码在分析记录中就不会出现。

现象1：在飞机起飞的时候帧率下降比较大如下图：

- 静止

![](http://mingchuan.wang/img/ProjectWing/7.png)

- 起飞

![](http://mingchuan.wang/img/ProjectWing/8.png)

从屏幕上的stat信息可以看出，GPU 时间 更接近 Frame time，所以游戏瓶颈是出在GPU渲染上。即使这样瓶颈是GPU渲染上，但是我也是出于好奇，看看GameThread
上到底发生了什么，可不可以进一步优化呢？所以在下面对CPU进行了"强行分析"...

## CPU分析

启动项目，在这里我选择了在飞机起飞的时候一段采集数据，打开Session Frontend，点击Data Capture开始抓取数据，再次点击停止。

完成之后，点Load file（数据采集 文件保存在下列路径中 ...\UE4\Engine\Programs\UnrealFrontend\Saved\Profiling\UnrealStats\Received\...）查看结果如图：

![](http://mingchuan.wang/img/ProjectWing/6.png)

从上图看出如下信息：
- 1.landscapeMap（我们的场景Map文件）中的Grass_Update是消耗最多的（当然上面还有PoolThread 22.989ms，这个是等待线程，分析无意义）。这个UE4中的植被系统，在场景中一个如图棵树：

![](http://mingchuan.wang/img/ProjectWing/9.png)
![](http://mingchuan.wang/img/ProjectWing/10.png)

UE4中的植被系统生成的植被会用了MeshInstance生成了一个实例（类似于Unity的批处理）。所以那就看LOD惊奇的发现树的Lod虽然设了但是ScreanSize
都是0.没用啊。这个树居然是UE4官方带的（也有可能是那个人故意这样设置的显示远景）。所以更改了一下设置。

- 2.图中PlayController下的BP_AW609v2_InpAxisEvent,是飞机起飞的时候螺旋桨转动事件。这是不可避免的。下面的MoveForward，LookUp等都是一样的。

## GPU分析

发现Scene中ShadowDepths的消耗是最大的，这个项目中阴影和效率有很大的关系。

![](http://mingchuan.wang/img/ProjectWing/11.png)

原因是飞机室内添加了好多动态灯光，因为飞机飞的时候也需要人在里面行走演示。场景一共消耗13.66ms，室内灯光消耗占据了64%，其他的消耗BasePass，后处理等消耗不大，所以室内的灯光需要重点优化。

室内灯光优化：为了凸显室内物品材质的效果（这是需求的重点），优化过程中一是在不减弱效果的同时通过减少灯光数量，调整调整光照强度和光照衰减半径来优化，尽量的达到照片级别效果！


## 优化总结
- 面对低端的设备可以将UE4默认的延迟渲染改为正向渲染。虽然正向渲染会因为反射、照明和阴影而导致丧失视觉保真，但剩余的场景从视觉上看没有改变，而性能的提高。
- 为staticmesh 添加Lod。
- Mesh Instance（类似批处理） 合并场景中的数量多的网格，减少Drawcall。可以代码中生成，也可以通过UE4植被编辑器创建。
- Mesh Instance 的物品的任何部分被渲染，那么整个集合都会被渲染。如果任何部分离开镜头，那么这会浪费潜在的吞吐量。建议在更小的区域内使用单一实例化网格集。
- 根据需要调整级联阴影贴图的等级，一般5将低到3可以。
- 即使完全透明的游戏对象也会用到渲染绘制调用。为避免这些调用浪费，可设置引擎停止对它们的渲染。
- 对于动态的物体，平行光可以 UseInsetShadowsForMovableObjects 设为 false，提高性能但是阴影效果下降，不适用与本项目。


## 最终效果(40Fps-70Fps)

![](http://mingchuan.wang/img/ProjectWing/3.png)

![](http://mingchuan.wang/img/ProjectWing/1.png)

## 参见
- https://docs-origin.unrealengine.com/latest/INT/Engine/Performance/index.html
- https://software.intel.com/zh-cn/articles/unreal-engine-4-optimization-tutorial-part-1

图片来源于Vidahouse侵权删。
版权声明：本文为博主原创文章，未经博主允许不得转载。
