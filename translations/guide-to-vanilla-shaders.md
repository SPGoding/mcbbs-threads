# 原版着色器指南

- 原　文: [Guide to vanilla shaders](https://docs.google.com/document/d/15TOAOVLgSNEoHGzpNlkez5cryH3hFF3awXL5Py81EMk/edit#)
- 原作者: SirBenet, with input from Godlander and Onnowhere
- 译　文: [指导到香草着色器](https://github.com/SPGoding/mcbbs-threads/blob/master/translations/guide-to-vanilla-shaders.md)
- 译　者: SPGoding
- 截止翻译时，原文最后更新: 2019-09-22 for Minecraft 1.14.4

# 前言

原版 Minecraft 有着许多的 GLSL（OpenGL Shading Language）着色器。这些着色器以前能通过选项中的「Super Secret Settings」来启用，但现在只在以下情景有应用了：
- 以特定生物的视角旁观时（苦力怕、蜘蛛、末影人）
- 屏幕中有具有发光状态效果的实体时

着色器可以直接通过资源包更改，进而嵌在地图里，或者是在玩家进入服务器时自动启用。

原版的着色器系统有一些问题和限制。它们可以为原版地图带来很多新的可能性，但是并不能完全替代由模组带来的更棒的光影包。

## 能做到什么

着色器会在游戏已经渲染好画面以后起效。它们能够接受整个屏幕的像素作为输入，然后逐像素地输出。除了一些有限的特例以外，着色器所能接受到的唯一数据就是正显示在屏幕上的内容。

着色器以 OpenGL Shading Language（GLSL）语言编写，这是一个类似 C 的语言，支持浮点数、向量、矩阵，以及函数、条件、循环以及其他各种你能想到的编程语言应有的特性。

以下是一些原版自带的着色器示例（以前的「Super Secret Settings」）

![image.png](https://i.loli.net/2019/09/23/yBqZsxOk2SAgfPd.png)  
"Pencil"

![image.png](https://i.loli.net/2019/09/23/i5Kldntc4s8y9DE.png)  
"Bits"

![image.png](https://i.loli.net/2019/09/23/ebnrL8aI2V9sfUQ.png)  
"NTSC"

# 梗概：着色器的组成部分

「岗位」（post）文件（储存在 `assets/minecraft/shaders/post` 目录下）定义了一个由一系列「程序」（program）组成的管道（pipeline）。下图展示了由 `creeper.json` 定义的管道：

![image.png](https://i.loli.net/2019/09/23/81ODcYF4EhRJ9fW.png)

每个「程序」（例如 `color_convolve`）都是定义在另一个 JSON 文件当中的（这回储存在 `shaders/program` 目录下）。该文件通常包括：
- 一个要使用的「顶点着色器」（vertex shader）的路径（一个以 GLSL 语言编写的 `.vsh` 文件）
- 一个要使用的「分段着色器」（fragment shader）的路径（一个以 GLSL 语言编写的 `.fsh` 文件）

「顶点着色器」会对每个顶点起效，将顶点的位置作为输入，并产生一个经过变换的位置作为输出。一个扭曲屏幕的顶点着色器示例见下：

![image.png](https://i.loli.net/2019/09/23/suc3nB2gvitdZ6I.png)

「分段着色器」会对每个像素起效，并逐像素产生输出层。一个不改变任何内容的分段着色器将直接把输入的像素原样产生。一个交换红色和蓝色色道的分段着色器示例见下：

![image.png](https://i.loli.net/2019/09/23/QY4Net5fjFMHExJ.png)

# 开始

1. [建立一个资源包](https://minecraft-zh.gamepedia.com/%E6%95%99%E7%A8%8B/%E5%88%B6%E4%BD%9C%E8%B5%84%E6%BA%90%E5%8C%85)
2. 在里面建好以下文件夹：
    - `assets/minecraft/shaders/post`
    - `assets/minecraft/shaders/program`

如果你想要参考原版的着色器，可以直接从游戏 jar 文件里面解压：`%APPDATA%/.minecraft/versions/1.14.4/1.14.4.jar/assets/minecraft/shaders/`。

你可能还需要给你用的编辑器装一个 GLSL 的语法高亮插件。

打开**游戏日志** —— 任何着色器的错误都会显示在这里。

![image.png](https://i.loli.net/2019/09/23/5bxNJBGdg726zvn.png)

![image.png](https://i.loli.net/2019/09/23/5yjrHDIwMWt1fm2.png)
