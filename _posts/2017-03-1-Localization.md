---
layout:     post
title:      UE4 本地化--多语言切换
subtitle:   Localization 本地化 多语言
date:       2017-02-16
author:     BY
header-img: img/post-bg-debug.png
catalog: true
tags:
    - Mac
    - 终端
    - Git
---


>并不适合阅读的个人文档。

# 本地化--多语言切换
## 1.设置Localization Dashboard面板 ( 此步骤中“勾选显示Localization Dashboard”需要自己设置，其他的已经设置好了，可以跳过这部分)
- 勾选显示Localization Dashboard ，可以Localization Dashboard工具

![](http://mingchuan.wang/img/local/1.png)

- 打开Localization Dashboard工具

![](http://mingchuan.wang/img/local/2.png)

- 配置Localization Dashboard 面板如下图所示（已经配置好了重新配置）。

![](http://mingchuan.wang/img/local/3.png)

![](http://mingchuan.wang/img/local/4.png)

![](http://mingchuan.wang/img/local/5.png)

## 2.文件中用宏添加FText类型变量

- 在.umap .uasset文件中 设置变量定义为Text类型，即可检测到 (可根据需求修改命名空间和key值)

![](http://mingchuan.wang/img/local/6.png)

- 在C++文件中

```
//"Namespace" :自己定义的命名空间（建议定义为文件名）
//"Key"：在当前命名空间下该内容的唯一标识,不可重定义.
//"TextValue"：翻译的内容
NSLOCTEXT("Namespace", "Key", "TextValue")
```

```
例子1：(直接定义访问)
建议：Value只使用一次的时候使用该方法
//直接添加宏转换成输出的value。如果"确定"按钮， 只在一个地方使用的情况,可使用这种写法（因为相同命名空间中key的值不能重复）
SNew(STextBlock).Text(NSLOCTEXT("MyTestCode", "Key_Tile", "确定"))
```

```
例子2：(间接访问)
建议：Value多次使用的时候使用该方法
先定义，统一定义在一个.cpp文件中。
void ULocalizationCultureManager::Initialize()
{
    //MT模块
	NSLOCTEXT("MyTestCode", "Key_Confirm", "确定");
	NSLOCTEXT("MyTestCode", "Key_Cancel", "取消");
        
    //MD模块
}
//可在SUIChangeMaterialWidget.cpp文件中通过命名空间和key获取得到Value值。
FText TileText;
FText::FindText("MyTestCode", "Key_Confirm", ConfirmText);
FText FulledTileText;
FText::FindText("MyTestCode", "Key_Cancel", CancelText);

slate 赋值
SNew(STextBlock).Text(FulledTileText).Font(FSlateFontInfo("Veranda", 18)).Justification(ETextJustify::Center)
```

- 执行第5步可添加翻译的语言(目前是中英文，需要加新语言的时候再执行这步操作)

- 根据第4步用NSLOCTEXT宏 添加要翻译的内容，执行6步 点击 Gather生成本地化配置文件，路径Game\Content\Localization\  

![](http://mingchuan.wang/img/local/6.png)

- 执行7步，在编译器中添加翻译内容如下（发现设置字体和字号保存不了，等ue4以后版本更新吧）

![](http://mingchuan.wang/img/local/7.png)


- 执行8步编译文件，生成对应翻译的配置文件，路径：Game\Content\Localization\NewTarget\zh,Game\Content\Localization\NewTarget\en  

![](http://mingchuan.wang/img/local/8.png)

.archive 为翻译的配置文件  .locres 为对应的二进制文件，运行读取的时候读的是二进制文件。

完成后重新启动客户端

## 3.打包设置

  - 在localization to package中把需要打包进去的语言勾选上,这里我们选上 "英文" 和 "中文"。
  - Internationalization Support选择 EFIGSCJK（多语言集合）
  - package default localization 设置为 zh

## 4.备注
- 配置文件提交

       Game\Content\Localization\ 路径下的文件都需要提交。 

- 乱码

![](http://mingchuan.wang/img/local/9.png)

设置如下：找到定义宏对应的C++文件

![](http://mingchuan.wang/img/local/10.png)

![](http://mingchuan.wang/img/local/11.png)








