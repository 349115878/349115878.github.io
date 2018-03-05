---
layout:     post
title:      UE4 本地化--多语言切换
subtitle:   Localization 本地化 多语言
date:       2017-03-3
author:     Trum
header-img: img/post-bg-debug.png
catalog: true
tags:
    - UE4
    - Localization
---


>并不适合阅读的个人文档。

# 本地化--多语言切换

## 本地化系统概述

文本 (Text)

文本是本地化的基本元素。文本是由命名空间、键名、源字符串和显示字符串共同组成。通过命名空间和键名便能为文本定义唯一的标识索引。 通过使用不同的命名空间，可以帮助在不同上下文的环境下同样的文本单来表达不同的意思。键提供了与文本有关的特定上下文。 源字符串是尚未翻译原文形式的字符串。显示字符串是将要呈现的字符串形式，通常是根据源字符串翻译而来。

例如，对话框可显示为英语或西班牙语。对话框中可能有一条消息，一个 “OK” 按钮和一个 “CANCEL” 按钮。所有三个文本可能使用了同一个命名空间 “MyProject”。消息文本可以使用一个 “MyMessage” 键，“OK” 文本可以使用一个 “DialogBox.AffirmativeButtonLabel” 键，“CANCEL” 文本可使用一个 “DialogBox.NegatoryButtonLabel” 键。根据命名空间和键，每个文本就可以获得唯一的标识和翻译。

目标 (Targets)

目标指的是经过命名的独立的本地化数据模块。目标中的文本是从一组特定来源中收集，存储在 Manifest 文件内，在针对特定语言 (Culture) 的存档文件中翻译，编译成针对特定语言的本地化资源文件，然后由系统加载以供显示。

一个项目可以只有一个目标以便简化实施，也可以有多个目标以便将项目的本地化数据分成各个独立部分。Unreal 编辑器就有一个独立于虚幻引擎其他部分的目标，因此可以在对编辑器进行本地化的同时，避免将编辑器的本地化数据随游戏一起分发。通常来讲，游戏会用一个目标用于游戏本体的本地化数据，并使用额外的目标来实现游戏的扩展包内容。

语言 (Cultures)

语言 (Cultures，以下使用 Culture 来指代避免语义混淆) 也被理解为语言环境 (locale)，负责定义文字、脚本和地区之类的细节信息。Culture 是特定编码格式的字符串，必需符合语言编码（ISO-639 标准）、可选脚本编码（ISO-15924 标准）和可选地区编码（ISO-3166 标准），以横线或下划线进行分隔。

Culture 编码的举例：“en”（英语）、“es-MX”（西班牙语，墨西哥地区）、“zh-Hans-CN”（中文，简体，中国地区）。

清单 (Manifests)

清单文件是一种可读的 JSON 格式文本文件，保存了由命名空间和键的映射下整理的文本 (Text) 。清单文件由一个特定的 commandlet 通过收集文本生成。清单文件每次都完全重新创建，不应当以手动方式更改。

存档 (Archives)

存档用于存储源字符串及其翻译，由命名空间映射，并采用可读的 JSON 格式。存档由一个 commandlet 生成，它将从指定的清单中去除所有条目。由于存档中的条目没有键，因此清单中共享命名空间内同一个源的所有条目将合并为一个存档条目，如果文本之间只有键的差别，就会被认为它们在字面上是一样的，并将使用相同的翻译。已经存在的存档将被更新而非截断。存档应该按原样提供，或是转换为其他格式提供给翻译人员进行处理，并返回译文取代空白存根条目。

本地化资源 (LocRes)

LocRes 可将翻译后的文本存储为二进制格式并由系统加载。LocRes 由一个 commandlet 生成，它能编译一个指定的清单和一些指定存档。

系统会根据项目设置和当前 Culture 选择加载 LocRes 文件。LocRes 中对应当前文化的本地化文本可以和 LocRes 中对应当前所有文化的父文化的本地化文本一起使用。这样既可以针对某一语言提供一般性翻译，同时还能针对某一区域提供更加具体的翻译。举一个基本的例子，假设有一个目标中包含文本 “Color” 并支持 “en”（英语）和 “en-UK”（英语，英国地区）的 Culture，那么 “en” LocRes 可能会将 “Color” 本地化成 “Color”，而 “en-UK” LocRes 可能会采用 “Colour” 这一本地化文本。如果用户切换到 “en-CA”（英语，加拿大地区），但 “en-CA” 的 LocRes 缺少对于 “Color” 的本地化信息，那么将会使用 “en” LocRes 里的 “Color”。

上面概述 From： https://docs-origin.unrealengine.com/latest/CHN/Gameplay/Localization/Overview/index.html

## 本地化流程

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








