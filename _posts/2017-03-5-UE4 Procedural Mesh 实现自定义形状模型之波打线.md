---
layout:     post
title:      UE4 Procedural Mesh 实现波打线效果
subtitle:   参数hau
date:       2017-02-16
author:     Trum
header-img: img/post-bg-debug.png
catalog: true
tags:
    - UE4
    - Procedural Mesh
---

# 波打线的实现思路
## 需求

- 1.围绕场景中所有的墙自动生成一圈波打线

- 2.在运行时实现可选中单个波打线（或全部）更换材质，删除功能。

![](http://mingchuan.wang/img/boundary/show_1.png)

- 3.波打线之间无缝连接，材质的方向统一朝向墙。

![](http://mingchuan.wang/img/boundary/boundary.png)

- 4.挖洞部分实现：

将一堵墙，根据洞的数目分隔成多段，每一段可以理解为新生成的一堵墙。并计算出新的段的顶点信息，作为新的一个面。

![](http://mingchuan.wang/img/boundary/Hole.png)


## 实现思路：
在一个ProceduralMesh上创建多个面，每一个面是一段波打线（减少Drawcall的调用）。 波打线的创建依赖于墙的位置和顶点信息，所以阅读此处代码最好先熟悉墙的创建规则.

- UI创建墙，传来墙的数据包含起始点和终止点，构成了一个向量，向量在空间中是有方向的唯一性。所以波打线根据位置分为Top，Bottom，Left，Right四个位置创建

## 生成波打线分为两步：
- 因为波打线是依付于墙的， 先根据墙体的顶点信息，生成Boundary基础的轮廓  此处的顶点序列需要查看墙体定义的结构进行调整，这样做在墙体缩放的时候也不会影响到波打线。
- 在Boundary基础的轮廓生成之后，根据顶点的信息进一步调整波打线的顶点，使得波打线何可和其他波打线无缝连接。相交的墙都在同侧的留下相交角度最小的墙  
- 两侧都有的取左右两侧各相交最小的角度的墙如下图  

![](http://mingchuan.wang/img/boundary/CreateBoundary.png)

- 材质的方向统一朝向墙

 基本原理是根据围绕墙的四个位置的波打线（Top Bottom Left Right）计算顶点旋转UV，生成对应方向的UV 

![](http://mingchuan.wang/img/boundary/UV.png)

## 最终看一下效果图

![](http://mingchuan.wang/img/boundary/show_2.png)

图片来源于Vidahouse
