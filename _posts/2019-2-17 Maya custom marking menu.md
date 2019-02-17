---
layout:     post
title:      Maya custom marking menu
subtitle:   marking menu
date:       2019-2-17
author:     wmc
header-img: img/post-bg-debug.png
catalog: true
tags:
- 3dmax
---


# Maya custom marking menu


![](http://mingchuan.wang/img/MayaMarkingMenu/Image 2.png)


mel：
C:\Users\wmc\Documents\maya\2018\prefs\markingMenus\menu_Yee

```
    menuItem
        -label "Hypershade" 
        -divider 0
        -subMenu 0
        -tearOff 0
        -command "HypershadeWindow" 
        -altModifier 0
        -optionModifier 0
        -commandModifier 0
        -ctrlModifier 0
        -shiftModifier 0
        -optionBox 0
        -enable 1
        -data 0
        -radialPosition "N" 
        -allowOptionBoxes 1
        -postMenuCommandOnce 0
        -enableCommandRepeat 1
        -image "menuIconWindow.xpm" 
        -echoCommand 0
        -italicized 0
        -boldFont 1
        -sourceType "mel" 
        -longDivider 1
        menuEditorMenuItem4;

    menuItem
        -label "      proc XHT_reloadAllFiles()\r{\r\tstring $RAF_list[] = `ls -typ..." 
        -divider 0
        -subMenu 0
        -tearOff 0
        -command "      proc XHT_reloadAllFiles()\r{\r\tstring $RAF_list[] = `ls -type \"file\"`;\r\tstring $RAF_fileName[];\r\r\tif (`size($RAF_list)` == 0)\r\t{\r\t\tprint (\"RELOAD ALL FILES : 377377377377377377377377377377377377377377 \\n\");\r\t}\r\telse\r\t{\r\t\tint $RAF_i;\r\t\tfor ($RAF_i=0; $RAF_i < size($RAF_list) ; $RAF_i++)\r\t\t{\r\t\t\tstring $RAF_textureName = `getAttr($RAF_list[$RAF_i]+\".fileTextureName\")`;\r\t\t\tsetAttr -type \"string\" ( $RAF_list[$RAF_i] + \".fileTextureName\") $RAF_textureName;\r\t\t\tprint ($RAF_list[$RAF_i] + \" reloaded \\n\");\r\t\t}\r\t\tprint (\"RELOAD ALL FILES : 377377377377377377377377377377377377377377377377 \\n\");\r\t}\r}\r\rXHT_reloadAllFiles;" 
        -altModifier 0
        -optionModifier 0
        -commandModifier 0
        -ctrlModifier 0
        -shiftModifier 0
        -optionBox 0
        -enable 1
        -data 0
        -radialPosition "NE" 
        -allowOptionBoxes 1
        -postMenuCommandOnce 0
        -enableCommandRepeat 1
        -image "commandButton.xpm" 
        -echoCommand 0
        -italicized 0
        -boldFont 1
        -sourceType "mel" 
        -longDivider 1
        menuEditorMenuItem8;

    menuItem
        -label "All" 
        -divider 0
        -subMenu 0
        -tearOff 0
        -command "ShowAll" 
        -altModifier 0
        -optionModifier 0
        -commandModifier 0
        -ctrlModifier 0
        -shiftModifier 0
        -optionBox 0
        -enable 1
        -data 0
        -radialPosition "E" 
        -allowOptionBoxes 1
        -postMenuCommandOnce 0
        -enableCommandRepeat 1
        -image "menuIconDisplay.xpm" 
        -echoCommand 0
        -italicized 0
        -boldFont 1
        -sourceType "mel" 
        -longDivider 1
        menuEditorMenuItem7;

    menuItem
        -label "History" 
        -divider 0
        -subMenu 0
        -tearOff 0
        -command "DeleteHistory" 
        -altModifier 0
        -optionModifier 0
        -commandModifier 0
        -ctrlModifier 0
        -shiftModifier 0
        -optionBox 0
        -enable 1
        -data 0
        -radialPosition "SE" 
        -allowOptionBoxes 1
        -postMenuCommandOnce 0
        -enableCommandRepeat 1
        -image "menuIconEdit.xpm" 
        -echoCommand 0
        -italicized 0
        -boldFont 1
        -sourceType "mel" 
        -longDivider 1
        menuEditorMenuItem19;

    menuItem
        -label "Freeze Transformations" 
        -divider 0
        -subMenu 0
        -tearOff 0
        -command "FreezeTransformations" 
        -altModifier 0
        -optionModifier 0
        -commandModifier 0
        -ctrlModifier 0
        -shiftModifier 0
        -optionBox 0
        -enable 1
        -data 0
        -radialPosition "S" 
        -allowOptionBoxes 1
        -postMenuCommandOnce 0
        -enableCommandRepeat 1
        -image "menuIconModify.xpm" 
        -echoCommand 0
        -italicized 0
        -boldFont 1
        -sourceType "mel" 
        -longDivider 1
        menuEditorMenuItem3;

    menuItem
        -label "Center Pivot" 
        -divider 0
        -subMenu 0
        -tearOff 0
        -command "CenterPivot" 
        -altModifier 0
        -optionModifier 0
        -commandModifier 0
        -ctrlModifier 0
        -shiftModifier 0
        -optionBox 0
        -enable 1
        -data 0
        -radialPosition "SW" 
        -allowOptionBoxes 1
        -postMenuCommandOnce 0
        -enableCommandRepeat 1
        -image "menuIconModify.xpm" 
        -echoCommand 0
        -italicized 0
        -boldFont 1
        -sourceType "mel" 
        -longDivider 1
        menuEditorMenuItem17;

    menuItem
        -label "UV Texture Editor" 
        -divider 0
        -subMenu 0
        -tearOff 0
        -command "TextureViewWindow" 
        -altModifier 0
        -optionModifier 0
        -commandModifier 0
        -ctrlModifier 0
        -shiftModifier 0
        -optionBox 0
        -enable 1
        -data 0
        -radialPosition "W" 
        -allowOptionBoxes 1
        -postMenuCommandOnce 0
        -enableCommandRepeat 1
        -image "menuIconWindow.xpm" 
        -echoCommand 0
        -italicized 0
        -boldFont 1
        -sourceType "mel" 
        -longDivider 1
        menuEditorMenuItem20;

    menuItem
        -label "Outliner" 
        -divider 0
        -subMenu 0
        -tearOff 0
        -command "OutlinerWindow" 
        -altModifier 0
        -optionModifier 0
        -commandModifier 0
        -ctrlModifier 0
        -shiftModifier 0
        -optionBox 0
        -enable 1
        -data 0
        -radialPosition "NW" 
        -allowOptionBoxes 1
        -postMenuCommandOnce 0
        -enableCommandRepeat 1
        -image "menuIconWindow.xpm" 
        -echoCommand 0
        -italicized 0
        -boldFont 1
        -sourceType "mel" 
        -longDivider 1
        menuEditorMenuItem18;

    menuItem
        -label "WireOnOff" 
        -divider 0
        -subMenu 0
        -tearOff 0
        -command "/*\r\n\t  Wireframe On/Off V0.1\r\n \r\n    Create by: leson lai(Lai ji)\r\n    Messenger: TNTII1981@hotmail.com\r\n  \r\n  \tPlease keep this title\r\n*/\r\nglobal proc Wireframeshow()\r\n{\r\n        string $currPanel = `getPanel -withFocus`;\r\n                int $WF_OnOff = `modelEditor -q -wos $currPanel`;\r\n                switch ($WF_OnOff)\r\n                {\r\n                        case 0: \r\n                                modelEditor -e -wos 1 modelPanel4;\r\n                                break;\r\n                        case 1: \r\n                                modelEditor -e -wos 0 modelPanel4;\r\n                                break;\r\n                        default: break;\r\n                }\r\n}\r\nWireframeshow;" 
        -altModifier 0
        -optionModifier 0
        -commandModifier 0
        -ctrlModifier 0
        -shiftModifier 0
        -optionBox 0
        -enable 1
        -data 0
        -allowOptionBoxes 1
        -postMenuCommandOnce 0
        -enableCommandRepeat 1
        -image "commandButton.xpm" 
        -echoCommand 0
        -italicized 0
        -boldFont 1
        -sourceType "mel" 
        -longDivider 1
        menuEditorMenuItem16;

    menuItem
        -label "DftMatOn" 
        -divider 0
        -subMenu 0
        -tearOff 0
        -command "modelEditor -e -udm 1 modelPanel4;\r\n// Result:modelPanel4//\r\neditMenuUpdate MayaWindow|mainEditMenu;" 
        -altModifier 0
        -optionModifier 0
        -commandModifier 0
        -ctrlModifier 0
        -shiftModifier 0
        -optionBox 0
        -enable 1
        -data 0
        -allowOptionBoxes 1
        -postMenuCommandOnce 0
        -enableCommandRepeat 1
        -image "commandButton.xpm" 
        -echoCommand 0
        -italicized 0
        -boldFont 1
        -sourceType "mel" 
        -longDivider 1
        menuEditorMenuItem13;

    menuItem
        -label "DftMatOff" 
        -divider 0
        -subMenu 0
        -tearOff 0
        -command "modelEditor -e -udm 0 modelPanel4;\r\n// Result:modelPanel4//\r\neditMenuUpdate MayaWindow|mainEditMenu;" 
        -altModifier 0
        -optionModifier 0
        -commandModifier 0
        -ctrlModifier 0
        -shiftModifier 0
        -optionBox 0
        -enable 1
        -data 0
        -allowOptionBoxes 1
        -postMenuCommandOnce 0
        -enableCommandRepeat 1
        -image "commandButton.xpm" 
        -echoCommand 0
        -italicized 0
        -boldFont 1
        -sourceType "mel" 
        -longDivider 1
        menuEditorMenuItem14;

    menuItem
        -label "GridOn" 
        -divider 0
        -subMenu 0
        -tearOff 0
        -command "modelEditor -e -grid 1 modelPanel4;\r\n// Result:modelPanel4//" 
        -altModifier 0
        -optionModifier 0
        -commandModifier 0
        -ctrlModifier 0
        -shiftModifier 0
        -optionBox 0
        -enable 1
        -data 0
        -allowOptionBoxes 1
        -postMenuCommandOnce 0
        -enableCommandRepeat 1
        -image "commandButton.xpm" 
        -echoCommand 0
        -italicized 0
        -boldFont 1
        -sourceType "mel" 
        -longDivider 1
        menuEditorMenuItem11;

    menuItem
        -label "GirdOff" 
        -divider 0
        -subMenu 0
        -tearOff 0
        -command "modelEditor -e -grid 0 modelPanel4;\r\n// Result:modelPanel4//" 
        -altModifier 0
        -optionModifier 0
        -commandModifier 0
        -ctrlModifier 0
        -shiftModifier 0
        -optionBox 0
        -enable 1
        -data 0
        -allowOptionBoxes 1
        -postMenuCommandOnce 0
        -enableCommandRepeat 1
        -image "commandButton.xpm" 
        -echoCommand 0
        -italicized 0
        -boldFont 1
        -sourceType "mel" 
        -longDivider 1
        menuEditorMenuItem12;

setParent -m ..;

```




版权声明：本文为博主原创文章，未经博主允许不得转载。


