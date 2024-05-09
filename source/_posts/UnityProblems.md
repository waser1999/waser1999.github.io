---
title: UnityProblems
date: 2024-05-09 18:48:08
tags:
- unity
- 软件使用
---
最近试了用Unity引擎开发一款简单的游戏，Unity引擎总体而言还算非常好用的，但某些设置经常忘记，在此记下，以备查询。  
以后我主要投入的方向是Unity和C#，传统前后端将不再支持。

<!--more-->

## 关于Unity对Asprite的支持
Unity支持通过`2D Aseprite Importer`对`.aseprite`或`.ase`文件直接导入，[但并不支持导入Tilemap文件](https://docs.unity3d.com/Packages/com.unity.2d.aseprite@1.1/manual/AsepriteFeatures.html#unsupported-features) 。如果需要导出，则需先在Aseprite中导出Tilemap专用的文件，再导入到Unity中。并且：  
1. 导入模式选择：Sprite Sheet，否则Sprite Editor无法将组件切割成不同部分；
2. 需要调节Pixel Per Unit，该值指的是单位长度能容纳多少像素，因此越低越大。
打开Sprite Editor进行切割。  
左上角如下图所示的选单中:  
a. Sprite Editor决定怎么导入素材；  
b. Custom Outline决定如何识别组件轮廓；  
c. Custom Physical Outline决定组件的物理轮廓（如碰撞体）。  
![切换编辑器](../images/image-1.png)
> 有时会遇见即使安装此拓展也无法导入的问题，如遇此问题，请升级组建的版本。

## 关于Animator与Animation组件的关系
Animation组件[已经弃用](https://docs.unity.cn/cn/2021.3/Manual/MecanimFAQ.html#:~:text=%E6%8F%90%E4%BE%9B%E6%97%A7%E7%89%88%E5%8A%A8%E7%94%BB%E7%B3%BB%E7%BB%9F%E5%8F%AA%E6%98%AF%E4%B8%BA%E4%BA%86%E5%90%91%E5%90%8E%E5%85%BC%E5%AE%B9%E6%97%A7%E9%A1%B9%E7%9B%AE%EF%BC%8C%E4%B8%8E%E6%88%91%E4%BB%AC%E6%9C%80%E6%96%B0%E7%9A%84%E5%8A%A8%E7%94%BB%E7%B3%BB%E7%BB%9F%E7%9B%B8%E6%AF%94%EF%BC%8C%E5%AE%83%E7%9A%84%E5%8A%9F%E8%83%BD%E9%9D%9E%E5%B8%B8%E6%9C%89%E9%99%90%E3%80%82%E4%BD%BF%E7%94%A8%E5%AE%83%E7%9A%84%E5%94%AF%E4%B8%80%E7%90%86%E7%94%B1%E5%BA%94%E8%AF%A5%E6%98%AF%E4%B8%BA%E4%BA%86%E5%85%BC%E5%AE%B9%E4%BD%BF%E7%94%A8%E6%97%A7%E7%B3%BB%E7%BB%9F%E6%9E%84%E5%BB%BA%E7%9A%84%E6%97%A7%E7%89%88%E9%A1%B9%E7%9B%AE%E3%80%82)，不推荐使用Animation管理动画，除非有兼容性相关的考量。

## 改变相机背景颜色
点击Main Camera->Camera组件->环境->背景类型，如图：
![Main Camera下的组件](../images/camera.png)

## 与IDE的向性
Unity目前支持Visual Studio、Visual Studio Code和Rider，其中对于前两者的实现均需要安装Visual Studio Editor（注意，VS Code也是一样，之前专供VS Code的Unity拓展[已被弃用](https://marketplace.visualstudio.com/items?itemName=VisualStudioToolsForUnity.vstuc#:~:text=Note%3A%20the%20Visual%20Studio%20Code%20Editor%20package%20is%20a%20legacy%20package%20from%20Unity%20that%20is%20not%20maintained%20anymore.)，而VS、VS Code方面只需要安装Unity相关拓展或环境即可。  
如果VS Code未能识别，则可能需要手动将其导入，并最好点一下`Regenerate Project Files`，尤其是项目中没有`.sln`文件时。