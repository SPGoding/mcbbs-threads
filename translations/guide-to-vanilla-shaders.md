# 指导到香草着色器

- 原　文: [Guide to vanilla shaders](https://docs.google.com/document/d/15TOAOVLgSNEoHGzpNlkez5cryH3hFF3awXL5Py81EMk/edit#)
- 原作者: SirBenet, with input from Godlander and Onnowhere
- 译　文: [指导到香草着色器](https://github.com/SPGoding/mcbbs-threads/blob/master/translations/guide-to-vanilla-shaders.md)
- 译　者: SPGoding
- 鸣　谢: 森林蝙蝠 - 提供了各种专业名词的专业机翻
- 截止翻译时，原文最后更新: 2019-09-22 for Minecraft 1.14.4

**已经原作者授权**
![image.png](https://i.loli.net/2019/09/23/pR95MBJQdq7Ooth.png)

# 前言

原版 Minecraft 有着许多的 GLSL（OpenGL Shading Language）着色器（译注：这些着色器经常会被称作光影）。这些着色器以前能通过选项中的「Super Secret Settings」来启用，但现在只在以下情景有应用了：
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

[Post](TOT) 文件（储存在 `assets/minecraft/shaders/post` 目录下）定义了一个由一系列「着色器程序」（program）组成的渲染管线（pipeline）。下图展示了由 `creeper.json` 定义的管线：

![image.png](https://i.loli.net/2019/09/24/Ewl8WJZUIaFPV7G.png)

每个[「着色器程序」](TOT)（例如 `color_convolve`）都是定义在另一个 JSON 文件当中的（这回储存在 `shaders/program` 目录下）。该文件通常包括：
- 一个要使用的「顶点着色器」（vertex shader）的路径（一个以 GLSL 语言编写的 `.vsh` 文件）
- 一个要使用的「片段着色器」（fragment shader）的路径（一个以 GLSL 语言编写的 `.fsh` 文件）

[「顶点着色器」](TOT)会对每个顶点起效，将顶点的位置作为输入，并产生一个经过变换的位置作为输出。一个扭曲屏幕的顶点着色器示例见下：

![image.png](https://i.loli.net/2019/09/23/suc3nB2gvitdZ6I.png)

[「片段着色器」](TOT)会对每个像素起效，并逐像素产生输出层。一个不改变任何内容的分段着色器将直接把输入的像素原样产生。一个交换红色和蓝色色道的分段着色器示例见下：

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

# 创建一个Post.JSON

Post.JSON 文件应该放置在 `assets/minecraft/shaders/post` 目录当中，并且命名为：
- `creeper.json` 将在以苦力怕视角旁观时启用
- `invert.json` 将在以末影人视角旁观时启用
- `spider.json` 将在以蜘蛛视角旁观时启用
- `entity_outline.json` 将在屏幕中有具有发光状态效果的实体时启用

我们只能通过替换以上四个现存的着色器来使用自定义着色器。没有应用其他着色器的办法。

岗位文件由两个数组构成：

```json
{
   "targets": [ … ],
   "passes": [ … ]
}
```

## Targets

`"targets"` 数组**声明**了一些**帧缓冲（frame buffers）** —— 把它们想象成我们可以往上面读取或写入像素的画布。该数组中的每一项都可以是以下两者之一：
- 一个有着 `"name"`（名称，字符串）、`"width"`（宽度，整型数字）、`"height"`（高度，整型数字）三个参数的对象。这三个参数描述了帧缓冲。
- 一个字符串名称。宽度和高度将会默认为游戏窗口的宽度和高度。

你可以在声明帧缓冲时随意混用两种方式：

```json
"targets": [
   "foo",
   "bar",
   {"name":"baz", "width":73, "height":10},
   "qux"
]
```

除了这些手动声明的缓冲层，还有一些特殊的、预先定义好的缓冲。这些缓冲里面会自带内容，不需要声明就可以直接使用。它们是：

- 为**发光着色器**准备的、预先填充好的特殊缓冲区

    ![image.png](https://i.loli.net/2019/09/23/nsHvkReqrzWlwAK.png)  
    `minecraft:main`  
    没有水、方块实体和其他一些东西

    ![image.png](https://i.loli.net/2019/09/23/kCLdXx63pQfqn51.png)  
    `final`  
    纯色的实体。颜色是该实体所在队伍的颜色

- 为**实体视角着色器**准备的、预先填充好的特殊缓冲区

    ![image.png](https://i.loli.net/2019/09/23/p2fI9tuVwUGJOng.png)  
    `minecraft:main`  
    所有内容都已渲染完成

## Passes

`"passes"` 数组定义了一系列的按顺序执行的步骤。每一个步骤都包含一个输入缓冲层（`"in_target"`），并运行一个程序（`"name"`），来将数据写入到输出缓冲层（`"out_target"`）当中。一个程序不能把数据输出到它读取的那个缓冲层。

```json
"passes": [
   {
       "name": "prog1",
       "intarget": "minecraft:main",
       "outtarget": "foo"
   },
   {
       "name": "prog2",
       "intarget": "foo",
       "auxtargets": [ … ],
       "outtarget": "bar"
   },
   {
       "name": "blit",
       "uniforms": { … },
       "intarget": "bar",
       "outtarget": "minecraft:main"
   }
]
```

`"blit"` 是一个不改变任何内容的程序，它只是简单地把数据从一个缓冲复制到另一个缓冲当中（注意：[在不同大小的缓冲之间复制数据时会有问题](TOT)）。

你想要显示的内容应当最终输出到 `"minecraft:main"` 缓冲当中（如果是发光着色器，还可以进一步修改 `final` 缓冲，该缓冲的内容[会覆盖到一切内容上面](TOT)）

### Passes.Auxtargets

可选的 `"auxtargets"` 数组提供了一系列补充的缓冲或图片，使得程序能够**读取**它们。在 auxtargets 数组中的对象应当包含：
- `"id"` - 指定一个现存缓冲的名称（即定义在 `"targets"` 中的名称），**或者**是一张位于资源包 `minecraft/textures/effect` 目录下的图片的文件名。
- `"name"` - 能让你给该缓冲或图片分配任意的一个名称，使得程序的 GLSL 代码中能够访问它们。
- 如果指定的是一张**图片**，还必须指定以下参数：
    - `"width"` - 以像素为单位的图片宽度（*似乎没有实际效果？*）
    - `"height"` - 以像素为单位的图片高度（*似乎没有实际效果？*）
    - `"bilinear"` - 指定该图片被采样时使用的[缩放算法](TOT)

一个示例 auxtargets 数组如下，它使得程序能够访问 `qux` 缓冲，以及一张叫作 `abc.png` 的图片：

```json
"auxtargets": [
   {"id":"qux", "name":"QuxSampler"},
   {"id":"abc", "name":"ImageSampler", "width":64, "height":64, "bilinear":false}
]
```

### Passes.Uniforms

可选的 `"uniforms"` 参数能够向着色器程序传递一个浮点数数组。下方的示例使用 uniforms 为 `blur` 程序指定了一个半径（radius）和方向（**dir**ection）：

```json
{
   "name": "blur",
   "intarget": "swap",
   "outtarget": "minecraft:main",
   "uniforms": [
       {
           "name": "BlurDir",
           "values": [ 0.0, 1.0 ]
       },
       {
           "name": "Radius",
           "values": [ 10.0 ]
       }
   ]
},
```

## 可运作的示例

以下是一个可以正常运作的完整的 post JSON 文件。它添加了一个 ["notch" 抖动效果](TOT)，并减少了颜色饱和度。

[assets/minecraft/shaders/post/spider.json](https://drive.google.com/file/d/1lEW6WRHa0xN041qNNhr2XvpspiXK7A6g/view?usp=sharing)

```json
{
   "targets": [
       "swap"
   ],
   "passes": [
       {
           "name": "notch",
           "intarget": "minecraft:main",
           "outtarget": "swap",
           "auxtargets": [
               {
                   "name": "DitherSampler",
                   "id": "dither",
                   "width": 4,
                   "height": 4,
                   "bilinear": false
               }
           ]
       },
       {
           "name": "color_convolve",
           "intarget": "swap",
           "outtarget": "minecraft:main",
           "uniforms": [
               { "name": "Saturation", "values": [ 0.3 ] }
           ]
       }
   ]
}
```

# 创建一个「着色器程序」JSON

着色器程序 JSON 文件应该放置在 `assets/minecraft/shaders/program` 文件夹中。 

可以使用任何名称（只要遵守正常资源包的文件命名规则即可，例如没有大写字母什么的）。

```json
{
   "blend": { … },
   "vertex": "foo",
   "fragment": "foo",
   "attributes": [ … ],
   "samplers": [ … ],
   "uniforms": [ … ]
}
```

`"vertex"` 指定了将要使用的[顶点着色器](TOT) `.vsh` 文件的文件名。

`"fragment"` 指定了将要使用的[分段着色器](TOT) `.fsh` 文件的文件名。

`"attributes"` 是一个字符串数组，指定顶点的哪些属性能够被顶点着色器访问到。目前只能写 `"Position"`（位置）。

```json
"attributes": [ "Position" ],
```

`"samplers"` 定义了分段着色器想要访问缓冲需要用到的变量名。`"DiffuseSampler"` 是被自动给予 `"intarget"` 中定义的缓冲的变量名。其他的任何名字都需要与你在 [post 文件的 `"auxtargets"`](#Passes.Auxtargets) 中指定的一致。

```json
"samplers": [
   { "name": "DiffuseSampler" },
   { "name": "DitherSampler" }
]
```

`"uniforms"` 定义了各种*全局量*（又译「统一量」。对每个顶点或像素都保持不变的值）的名称、类型和默认值。

```json
"uniforms": [
   { "name": "ProjMat", "type": "matrix4x4", "count": 16, "values": [ 1.0, 0.0, 0.0, 0.0, 0.0, 1.0, 0.0, 0.0, 0.0, 0.0, 1.0, 0.0, 0.0, 0.0, 0.0, 1.0 ] },
   { "name": "InSize",  "type": "float", "count": 2,  "values": [ 1.0, 1.0 ] },
   { "name": "OutSize", "type": "float", "count": 2,  "values": [ 1.0, 1.0 ] },
   { "name": "BlurDir", "type": "float", "count": 2,  "values": [ 1.0, 1.0 ] },
   { "name": "Radius",  "type": "float", "count": 1,  "values": [ 5.0 ] }
]
```

`"name"` 字符串定义了在 GLSL 代码中或者[在向该着色器程序的该全局量传值时](#Passes.Uniforms)用的名称。游戏自动给了一些特殊的全局量：

- `"Time"` - 0 到 1 的一个值，表示在一秒内的时间，每秒自动重置
- `"InSize"` - 以像素为单位的输入缓冲（input buffer）的宽度和高度
- `"OutSize"` - 以像素为单位的输出缓冲（output buffer）的宽度和高度
- `"ProjMat"` - 由顶点着色器使用的投影矩阵（projection matrix）

`"values"` 应为一个浮点数数组，而 `"type"` 则定义了这些浮点数会在 GLSL 代码中被解析为的[数据类型](TOT)：

- `"float"` - 一个 `float` 或 `vec2`/`vec3`/`vec4`，具体取决于在 `"values"` 中指定的数字个数
- `"matrix4x4"` - 一个由 `"values"` 中指定的 16 个值产生的 `mat4`
- `"matrix3x3"` - *可用的类型，但仍然需要输入 16 个值？*
- `"matrix2x2"` - *可用的类型，但仍然需要输入 16 个值？*

`"blend"` 理论上定义了着色器程序的输出应当怎样与目标缓冲（destination buffer）上已有的内容合并，*但目前好像没有效果。*

```json
"blend": {
   "func": "add",
   "srcrgb": "one",
   "dstrgb": "zero"
}
```

更多关于这些模式本应怎样运作的信息可以查看：khronos.org/opengl/wiki/Blending

## 可运作的示例

以下是一个可以正常运作的完整的着色器程序 JSON 文件。这是原版的 "wobble" 着色器，未经修改。

[assets/minecraft/shaders/program/wobble.json](https://drive.google.com/file/d/1GssoJB9wQrj-POC_DZ--Rewmfvz9QvEq/view?usp=sharing)

```json
{
    "blend": {
        "func": "add",
        "srcrgb": "one",
        "dstrgb": "zero"
    },
    "vertex": "sobel",
    "fragment": "wobble",
    "attributes": [ "Position" ],
    "samplers": [
        { "name": "DiffuseSampler" }
    ],
    "uniforms": [
        { "name": "ProjMat", "type": "matrix4x4", "count": 16, 
          "values": [ 1.0, 0.0, 0.0, 0.0, 
                      0.0, 1.0, 0.0, 0.0, 
                      0.0, 0.0, 1.0, 0.0, 
                      0.0, 0.0, 0.0, 1.0 ] },
        { "name": "InSize", "type": "float", "count": 2, 
          "values": [ 1.0, 1.0 ] },
        { "name": "OutSize", "type": "float", "count": 2, 
          "values": [ 1.0, 1.0 ] },
        { "name": "Time", "type": "float", "count": 1, 
          "values": [ 0.0 ] },
        { "name": "Frequency", "type": "float", "count": 2, 
          "values": [ 512.0, 288.0 ] },
        { "name": "WobbleAmount", "type": "float", "count": 2, 
          "values": [ 0.002, 0.002 ] }
    ]
}
```

# GLSL 基础

该部分假设读者已经熟悉了基本的编程概念（变量、函数、循环等），并且主要讲解 GLSL 特有的特性。如果你之前从来没有接触过编程，我不建议你用 GLSL 上手。

有关核心语言的更多信息：khronos.org/opengl/wiki/Core_Language_(GLSL)

有关所有可用函数的详细文档：docs.gl/sl4/all

> **注意**： 原版 jar 文件中的着色器是用的 GLSL 版本是 110，本文为保持一致也将使用该版本。不过，由于 [Minecraft 需要 OpenGL 4.4](https://help.mojang.com/customer/en/portal/articles/325948-minecraft-java-edition-system-requirements)，你可以放心地使用最高到 440 版本的 GLSL。版本需要在 GLSL 代码的开头声明（可以看示例）。

## 数据类型

> ### 标量
> 
> `bool`: 布尔值 `true` / `false`  
> `int` / `unit`: 有符号 / 无符号 32 位整型数字  
> `float` / `double`: 单精度 / 双精度浮点数
> 
> ### 向量
> 
> `bve`**_`n`_**、`ive`**_`n`_**、`uvec`**_`n`_**、`vec`**_`n`_**、`dvec`**_`n`_**: 分别代表由 > **_`n`_** 个 `bool`、`int`、`uint`、`float` 或 `double` 组成的向量
> 
> **_`n`_** 必须为 2、3 或 4。
> 
> ### 矩阵
> 
> `mat`**_`n`_**: 由 `float` 组成的矩阵，大小为 **_`n`_** × **_`n`_**  
> `mat`**_`n`_**`x`**_`m`_**: 由 `float` 组成的矩阵，大小为 **_`n`_** × **_`m`_**  
> 
> **_`n`_** 和 **_`m`_** 都必须为 2、3 或 4。
> 
> 矩阵是**列优先**的（3×2 = 3 列，2 行）。

在 GLSL 里最常处理的就是 `float` 了。

想要构造向量和矩阵的话，可以任意组合其他的标量 / 向量：

```glsl
vec2 v2 = vec2(0.4, 0.5);
vec3 v3 = vec3(v2, 0.6);
mat4 m3 = mat3(0.1, 0.2, 0.3,
               v3,
               v2, 0.7);
```

请注意，列优先的矩阵可能有点反直觉：

```glsl
mat2(a, b,   // 第一列（不是第一行！）
     c, d);  // 第二列
```
