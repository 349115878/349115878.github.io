---
layout:     post
title:      ProjectWing 性能优化
subtitle:   性能优化
date:       2017-5-20
author:     Trump
header-img: img/post-bg-debug.png
catalog: true
tags:
- UE4
- 性能优化
---

# ProjectWing 项目介绍
Project项目是为意大利 Agusta-westland 直升机厂商定制的 AW609型号直升机虚拟现实项目展示。

![](http://mingchuan.wang/img/ProjectWing/4.png)

官方网站： https://www.windriver.com/customers/customer-success/aerospace-defense/agusta-westland/

本项目由我和另一个同事合作完成，我主要负责飞机飞行模拟逻辑实现，舱内交互动画实现，Sequence动画实现，Bug修复，场景性能优化，VR功能实现。
项目的场景是用UE4官方Demo（用户也说OK），当时是2016年的11月份接到这个项目，当时我还没用或Worldmachine，在经历这个项目之后意识到一个高端的场景对应这样的项目是多么的重要，才那开始学习Worldmachine.

Demo视频展示(pc演示效果更好)： https://pan.baidu.com/s/1AUowCtVkLiLgB__AddRU1A

本文只介绍场景性能优化方面的经历。

# 概述

性能优化不是一成不变，根据优化的项目需求，平台资源的不同将会有所侧重，同时帧数过高过低都不是目标，这都需要根据项目的需要而定。ProjectWing这个项目定义为飞行演示，所以美术效果表现是首要的，其次指定的PC平台上帧数控制在30帧以上，演示的时候没有特别明显的卡顿就可以了。

首先明确优化为了达到Fps目标，利用毫秒描述可以更好的量化，更直观，比如：
该表显示了平均 fps 和 ms之间的关系

![](http://mingchuan.wang/img/ProjectWing/5.png)

启动有项目
- 烘焙资源（烘焙后资源将放在Game/Save/下，资源数据被引擎优化处理，防止被不必要的资源影响）；
- 设置为独立应用运行 (PIE)，这样可以让编辑器菜单逻辑和其他代码在分析记录中就不会出现。

现象1：在飞机起飞的时候帧率下降比较大如下图：

- 静止
![](http://mingchuan.wang/img/ProjectWing/7.png)

- 起飞
![](http://mingchuan.wang/img/ProjectWing/8.png)

从屏幕上的stat信息可以看出，GPU 时间 更接近 Frame time，所以游戏瓶颈是出在GPU渲染上。即使这样瓶颈是GPU渲染上，但是我也是出于好奇，看看GameThread
上到底发生了什么，可不可以进一步优化呢？所以在下面对CPU进行了"强行分析"...

CPU分析

启动项目，在这里我选择了在飞机起飞的时候一段采集数据，打开Session Frontend，点击Data Capture开始抓取数据，再次点击停止。

完成之后，点Load file（数据采集 文件保存在下列路径中 ...\UE4\Engine\Programs\UnrealFrontend\Saved\Profiling\UnrealStats\Received\...）查看结果如图：

![](http://mingchuan.wang/img/ProjectWing/6.png)

从上图看出如下信息：
- 1.landscapeMap（我们的场景Map文件）中的Grass_Update是消耗最多的（当然上面还有PoolThread 22.989ms，这个是等待线程，分析无意义）。这个UE4中的植被系统，在场景中一个如图棵树：

![](http://mingchuan.wang/img/ProjectWing/9.png)

UE4中的植被系统生成的植被会用了MeshInstance生成了一个实例（类似于Unity的批处理）。所以那就看LOD惊奇的发现树的Lod虽然设了但是ScreanSize
都是0.没用啊。这个树居然是UE4官方带的（也有可能是那个人故意这样设置的显示远景）。所以更改了一下设置。

- 2.图中PlayController下的BP_AW609v2_InpAxisEvent,是飞机起飞的时候螺旋桨转动事件。这是不可避免的。下面的MoveForward，LookUp等都是一样的。




# 


# ProjectWing性能优化
此文档记录ProjectWing2项目 性能优化历程：
## 优化前分析
ctril + shift + ，打开GPU检测工具对ProjectWing2项目检测。
平行光作为太阳在400000*400000*100000的 LightmassImportanceVolume作用下，发现Scene中ShadowDepths的消耗是最大的。所以这个项目中阴影和效率有很大的关系。普通来说不开灯光的阴影比开的阴影要便宜20倍左右，尤其是当你的物件数量都是一个倍乘的系数越多就越耗性能。继续跟进发现shadowDepths下面场景中的树的阴影消耗非常的大。

![](http://mingchuan.wang/img/ProjectWing/2.png)

飞机起飞后帧数又减少了一半。是因为DirectionLight对运动的飞机作用，
飞机(动态物体)必须要从距离场阴影贴图中集成世界场景的静态阴影。这是通过使用每个对象的阴影完成的。每个可移动的对象从DirectionLight(固定光源)创建两个动态阴影：一个用于处理静态环境世界投射到该对象上的阴影，一个处理该对象投射到环境世界中的阴影。DirectionLight 唯一的阴影消耗就来源于它所影响的动态对象飞机。但是飞机中的ChildenActor数量也还好，消耗可以接受。如果动态对象的数量很多，使用可移动光源会更加高效。
另外飞机内部也放置了动态的灯光也消耗了一部分性能，不过和场景中树的阴影比起来小很多。

## 优化户外
- 之前版本的树是没有阴影的，树添加阴影，场景更加逼真。调整DirectionLight 的 Dynamic Shadow Distance StationaryLight 为10000 (虽然这里的树不会动)，改变树的阴影效果还不错，帧率较少1-2帧可以接受。
- 优化前场景中的树大概有2000多棵，优化方法是减少树的数量，减少到500多棵。另外对场景中100多块石头的取消产生阴影。
- 重新Build光照贴图。发现性能有大幅度的提升！飞机静止状态帧数从30左右提升到60左右。 所以就不需要进一步用其他的方法的优化了。。。       结束 :)

##  优化飞机内
   因为场景中的帧率有了大幅的提升，飞机室内的效果当然是越逼真越好了，所以飞机内格外的添加了两个灯来照明座椅，调整光照强度和光照削减半径。尽量的达到照片级别！

## 最终效果(35Fps-70Fps)

![](http://mingchuan.wang/img/ProjectWing/3.png)

![](http://mingchuan.wang/img/ProjectWing/1.png)

