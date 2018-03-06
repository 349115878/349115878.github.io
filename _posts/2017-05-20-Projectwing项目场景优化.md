---
layout:     post
title:      ProjectWing2 场景优化
subtitle:   场景优化
date:       2017-5-20
author:     Trum
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

本项目由我和另一个同事合作完成，我主要负责飞机飞行模拟逻辑实现，舱内交互动画实现，Sequence动画实现，Bug修复，场景性能优化，VR功能实现。本文只介绍场景性能优化方面的经历，OK，那就开始吧！

# ProjectWing2性能优化
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

