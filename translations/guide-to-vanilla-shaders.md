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

# 创建一个 post JSON

Post.JSON 文件应该放置在 `assets/minecraft/shaders/post` 目录当中，并且命名为：
- `creeper.json` 将在以苦力怕视角旁观时启用
- `invert.json` 将在以末影人视角旁观时启用
- `spider.json` 将在以蜘蛛视角旁观时启用
- `entity_outline.json` 将在屏幕中有具有发光状态效果的实体时启用

我们只能通过替换以上四个现存的着色器来使用自定义着色器。没有应用其他着色器的办法。

post 文件由两个数组构成：

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

可选的 `"auxtargets"` 数组提供了一系列补充的缓冲或图片，使得着色器程序能够**读取**它们。在 auxtargets 数组中的对象应当包含：
- `"id"` - 指定一个现存缓冲的名称（即定义在 `"targets"` 中的名称），**或者**是一张位于资源包 `minecraft/textures/effect` 目录下的图片的文件名。
- `"name"` - 能让你给该缓冲或图片分配任意的一个名称，使得程序的 GLSL 代码中能够访问它们。
- 如果指定的是一张**图片**，还必须指定以下参数：
    - `"width"` - 以像素为单位的图片宽度（*似乎没有实际效果？*）
    - `"height"` - 以像素为单位的图片高度（*似乎没有实际效果？*）
    - `"bilinear"` - 指定该图片被采样时使用的[缩放算法](TOT)

一个示例 auxtargets 数组如下，它使得着色器程序能够访问 `qux` 缓冲，以及一张叫作 `abc.png` 的图片：

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

除了普通的\[索引\]以外，**混合** `xyzw` 或 `rgba` 是种方便的获取某个向量的 1 个或多个元素的方式：

```glsl
vec3 v3 = vec3(0.1, 0.2, 0.3);
v3.x;     // == 0.1
v3.yzyz;  // == vec4(0.2, 0.3, 0.2, 0.3)
```

## 着色器程序工作流程

顶点着色器或分段着色器都有一个 `void main()` 函数，作为代码的起始点。该函数没有任何参数，也不返回任何东西，它只应更新一个现存的特殊变量（`gl_Position` 或 `gl_FragColor`，将在下方解释。）

由于 GLSL 代码是在无法任意写入内存的硬件上执行的，因此 GLSL 不支持递归操作。不过循环是可以的：

```glsl
for (int i = 0; i < 10; i++) { ... }
while (x < 20) { ... }
```

## 属性、全局量、传递变量

> 如果你选择了更高版本的 GLSL，该部分在 GLSL 140 及以上版本有所改动。见：https://www.khronos.org/opengl/wiki/Type_Qualifier_(GLSL)#Shader_stage_inputs_and_outputs。

全局变量可以被声明为 `attribute`（属性）、`uniform`（全局量）或 `varying`（TOT）。

`attributes`（属性）只能由顶点着色器读取，它自动包含了当前顶点的一些信息。对于 Minecraft 的着色器来说，它只包含了顶点的 `Position`（位置）属性。

`uniform`（全局量）可以被顶点着色器与分段着色器读取，对所有的顶点或像素都会保持恒定不变。它们的值可以通过 post JSON 文件传入，否则将会使用在着色器程序 JSON 文件中定义的默认值。

`varying`（传递变量）由顶点着色器声明并赋值，然后可在分段着色器中被读取。这些值会在顶点间**插值**计算出来。举个例子：

```glsl
varying vec2 texCoord;
```

![image.png](https://i.loli.net/2019/09/28/c7WE8yQUFjdZoDO.png)

# 编写一个顶点着色器

顶点着色器会在每个顶点上运行，以对顶点进行变换。通常来讲这指的是世界几何体的每一个顶点 —— 但是在 Minecraft 中，它指的只是缓冲四个角上的顶点。这使得目前的顶点着色器十分受限。不对顶点缓冲器做任何修改是很常见的做法（绝大多数原版的着色器程序都重复使用了 `sobel.vsh`）。

![image.png](https://i.loli.net/2019/09/28/xCswyYEjhgpQPAi.png)

顶点着色器的主要工作是用一个「投影矩阵」乘坐标。一般来讲这会把一个处于[视锥](https://upload.wikimedia.org/wikipedia/commons/0/02/ViewFrustum.svg)内的顶点转换到一个 2×2×2 的立方体中，这样能够更方便地拍扁到屏幕上：

![image.png](https://i.loli.net/2019/09/28/p39hGSMJB2VfcnX.png)

不过在 Minecraft 中，顶点着色器直接就从一个处于 3 维空间内的平面开始，并且直接变换四个角上的顶点，而不是变换任何实际上的几何体的顶点。Minecraft 使用 `ProjMat` 这样一个特殊的全局量用来计算，虽然这完全可以用 4 个硬编码（hardcoded）的特例来替代（因为这些顶点总是会被计算到相同的位置上）[*](TOT)。

![image.png](https://i.loli.net/2019/09/28/4EAisOGleYaLJHS.png)

顶点的初始位置可以从 `Position` 属性获取，而计算结果应当储存到特殊的 `gl_Position` 变量当中（这两者都是 `vec4`）。

顶点着色器还定义并赋值了一些有用的传递变量，以供片段着色器使用：
- `texCoord`: 范围从 `0,0`（左下角）到 `1,1`（右上角）的坐标。
- `oneTexel`: 在 `texCoord` 所表示的坐标中，一个像素所占的大小。例如：对于一个像素数为 4×2 的输入缓冲，其中每个像素的大小为 0.25×0.5（1/4 的高度，1/2 的宽度）

## 可运作的示例

`sobel.vsh` 顶点着色器，它被绝大多数原版的着色器程序所调用。它并没有什么特别涉及到 sobel（索伯算子？）的东西 —— 这么命名可能只是因为它是 Mojang 第一个使用的着色器程序。

```glsl
#version 110

attribute vec4 Position;

uniform mat4 ProjMat;
uniform vec2 InSize;
uniform vec2 OutSize;

varying vec2 texCoord;
varying vec2 oneTexel;

void main(){
    vec4 outPos = ProjMat * vec4(Position.xy, 0.0, 1.0);
    gl_Position = vec4(outPos.xy, 0.2, 1.0);

    oneTexel = 1.0 / InSize;

    texCoord = Position.xy / OutSize;
}
```

# 编写一个分段着色器

分段着色器会为每一个**输出**像素执行一次，可以读取它的输入缓冲中的任何像素。

每一个像素都是异步计算的，因此分段着色器不能读取输出缓冲中的其他像素，因为那些像素有可能还没有被计算 [*](https://stackoverflow.com/questions/16365385/explanation-of-dfdx/16368768#16368768)。

## Sampler

`texture2D` 函数允许你用一个 [sampler](#创建一个「着色器程序」JSON) 来获取一个缓冲中的像素。例如：

```glsl
vec4 centerPixel = texture2D(DiffuseSampler, vec2(0.5, 0.5));
```

在从缓冲中获取像素时，`0.0, 0.0` 是左下角，`1.0 1.0` 是右上角。

![image.png](https://i.loli.net/2019/09/28/pXHyUFJZ7gMx6C8.png)

这和顶点着色器设置的 `texCoord` 传递变量一致，十分方便。因此对于当前正在输出的像素，你可以用以下代码来获取相应的位于输入缓冲中的像素：

```glsl
texture2D(DiffuseSampler, texCoord)
```

该函数返回一个包含 `red`（红）、`green`（绿）、`blue`（蓝）、`opacity`（不透明度）的 `vec4` —— 这四者都由一个从 0 到 1 的 `float` 表示。

`oneTexel` 代表一个像素（在输入缓冲中）的大小，所以可以用它来偏移指定数量的像素。例如：获取往上数第 3 个像素的颜色：

```glsl
texture2D(DiffuseSampler, texCoord + vec2(0.0, 3 * oneTexel.y));
```

如果想要得到以像素为单位的坐标，而不是 0-1 的小数的话，用 `OutSize` 乘即可。例如：

```glsl
OutSize; // == vec2(1920, 1080)
texCoord; // == vec2(0.5, 0.5)
OutSize * texCoord; // == vec2(960, 540)
```

分段着色器应当把输出像素写入到 `gl_FragColor` 中 —— 另一个由 `red`（红）、`green`（绿）、`blue`（蓝）、`opacity`（不透明度）构成的 `vec4`。

## 可运作的示例

以下的着色器：
- 当距离中心的距离在 0.38 至 0.4 之间时，把这个像素变成橙色
- 当距离中心的距离在 0.38 至 0.4 之间时，读取放大过的（距离中心更近的）像素
- 当距离中心的距离在 0.38 至 0.4 之间时，正常读取对应的输入像素

```glsl
#version 110

uniform sampler2D DiffuseSampler;

varying vec2 texCoord;
varying vec2 oneTexel;

void main(){
    float distFromCenter = distance(texCoord, vec2(0.5, 0.5));

    if (distFromCenter < 0.38) {
        // Inside circle
        vec2 zoomedCoord = ((texCoord - vec2(0.5, 0.5)) * 0.2) + vec2(0.5, 0.5);
        gl_FragColor = texture2D(DiffuseSampler, zoomedCoord);
    } else if (distFromCenter >= 0.38 && distFromCenter < 0.4) {
        // Orange border
        gl_FragColor = vec4(0.7, 0.4, 0.1, 1.0); 
    } else {
        // Outside, normal pixels
        gl_FragColor = texture2D(DiffuseSampler, texCoord);
    }
}
```

![image.png](https://i.loli.net/2019/09/28/D5I9Pwc3EWtkj6U.png)

# 技巧与难题

## 发光颜色

发光的实体是一种不易被察觉的向 `entity_outline` 着色器传递数据的方式，这是因为该着色器可以读取 `final` 缓冲，并且使其不渲染可见的发光边框效果。

一个具有发光状态效果的实体的轮廓颜色由它所处**队伍的颜色**决定。实体本身具有的透明度会保留。这使得分段着色器可以读取并使用 16 种不同的颜色以及 256 种不同的透明度值。

![image.png](https://i.loli.net/2019/09/28/GdwBOAXL8JUfpIv.png)

（图中添加了棋盘背景以显示出透明度）

## 阴影

由于分段着色器可以读取像素，因此可以通过像素的颜色信息来传递信息。不过请小心，有许多因素会使你的材质变暗：

> ### 光照等级
>
> 远离光源并且不在太阳照射下的方块或实体会变暗。
>
> 可以用夜视效果或光源避免。
>
> ![image.png](https://i.loli.net/2019/09/28/eKDOWhgQfrw3UTS.png)
> 
> ### 面阴影
>
> 顶面是最亮的，其次是北面和南面，再其次是东面和西面，最暗的是底面。方块和实体都受到这一影响。
>
> 若想为方块禁用该阴影效果，可以在[模型](https://minecraft-zh.gamepedia.com/模型#方块模型)中将每个元素（element）设置为 `"shade": false`。
> 
> 不能为实体禁用！
>
> ![image.png](https://i.loli.net/2019/09/28/4PamBpFiklLwe97.png)
>
> ### 环境遮挡（「平滑光照」）
>
> 角落会变暗。只有方块受到这一影响。
>
> 若想为方块禁用该阴影效果，可以在[模型](https://minecraft-zh.gamepedia.com/模型#方块模型)中设置 `"ambientocclusion":false `。
>
> ![image.png](https://i.loli.net/2019/09/28/EYiX1bZIRCrGA6F.png)
>

## 编译器优化问题

GLSL 编译器会优化那些**它认为不会对着色器的输出有影响的代码与变量**（输出指的是 `gl_FragColor`、`gl_Position` 以及其他任何传递变量）。

GLSL 编译器找到一个没有被使用的 sampler 后会把它优化出去，但 Minecraft 仍然会传输同样的缓冲，这就导致了问题。所有的 sampler 都会读取前一个缓冲的内容。

![image.png](https://i.loli.net/2019/09/28/bCTycV2xi4zDwqa.png)

如果想避免让编译器优化出某个变量，你可以让编译器认为该变量有可能影响着色器的输出结果。例如，`texCoord.x` 永远不会是 `731031`，但编译器并不知道这一点：

```glsl
if (texCoord.x == 731031) { gl_FragColor = texture2D(DiffuseSampler, texCoord); }
```

## 跨帧储存信息

缓冲并不会被自动清空，因此着色器可以在不同帧之间传递信息。

想要获取在上一帧存储的数据很简单：
1. 在这一帧中，把数据写入到缓冲
2. 在下一帧中，在**新的数据被写入缓冲之前**读取它

为了让缓冲中的数据能跨刻存在，向一个缓冲写入的着色器程序必须要能让该缓冲中的数据不变。这就有些麻烦了，因为一个着色器程序**必须**要写入每一个像素，并且在这个过程中**不能**读取它正在写入的那个缓冲。

一个解决方案是，首先把信息复制到另一个缓冲当中。这时着色器程序就可以读取被复制出来的那个缓冲，同时把新数据写到原有的缓冲当中了，进而能够为每个像素写入它们在上一帧的值。

![image.png](https://i.loli.net/2019/09/28/4tYT5KzmVcIqvaP.png)

```json
"passes": [
   {
       "name": "blit",
       "intarget": "foo",
       "outtarget": "foo_copy"
   },
   {
       "name": "maybe_update",
       "intarget": "minecraft:main",
       "auxtargets": [ { "name": "BackupSampler", "id": "foo_copy" } ],
       "outtarget": "foo"
   }
]
```

## 着色器启用顺序

1. Minecraft 渲染普通方块和实体到 `minecraft:main`
2. Minecraft 渲染纯色的发光实体到 `final`
3. 运行发光着色器（`entity_outline.json`），其能够访问 `minecraft:main` 与 `final` 缓冲
4. Minecraft 渲染其他的东西（手、水、方块实体）到 `minecraft:main`
5. Minecraft 把 `final` 覆盖到 `minecraft:main` 上
6. 运行实体视角着色器，其能够访问 `minecraft:main` 缓冲

通过发光着色器向 `minecraft:main` 写入数据（第 3 步）似乎会破坏掉有关深度的信息，使得其他的东西（第 4 步）被渲染到 `minecraft:main` 的所有东西之上：

![image.png](https://i.loli.net/2019/09/28/crdONlsXaYCnASM.png)

## Blit 缩放问题

原版默认的 blit 着色器程序在相同大小的缓冲间复制数据时能正常运作，但是会在缓冲大小不一样时出问题。

这是因为顶点着色器 `blit.vsh` 没有匹配整个输出缓冲。例如，如果输出缓冲是输入缓冲一半的大小，只有一半的输出缓冲会被写入数据：

![image.png](https://i.loli.net/2019/09/28/WZ8FD1H9o53JzUj.png)

下方链接是一个 “Clone” 着色器，能够正确地拉伸输入，使其完全匹配到输出缓冲当中：

![image.png](https://i.loli.net/2019/09/28/mMegGWOHfnsDip2.png)

下载：[着色器程序 JSON 文件](https://drive.google.com/file/d/1LIYFv588QcbjmFHObk4sgWqCc1I5CDzq/view?usp=sharing)，[.vsh GLSL 文件](https://drive.google.com/file/d/1_pjORoiga0TT_KB5UrYwMCHIB9mvDmQd/view?usp=sharing)

## 读取玩家输入

玩家的 `Motion` 不会在服务端更新，但如果玩家尝试在骑着猪或者矿车时移动的话则会更新，尽管玩家事实上并没有移动。通过让玩家骑着猪或矿车，命令能够读取玩家的 `Motion`，并（结合玩家的视角方向）确定玩家当前正按下的方向键。

*TODO：示例命令*

左键或右键的检测和平时一样（击打生物、右键使用胡萝卜钓竿或与村民交谈）。

任何由命令获取到的信息（例如玩家目前正按着的按键）可以通过[一个发光实体的队伍的颜色传递给着色器](#发光颜色)。

*TODO：Rotation*

# 工具和参考

## GLSL

文档：  
http://docs.gl/sl4/all

教程：  
https://www.shadertoy.com/view/Md23DV  
https://thebookofshaders.com/

非 Minecraft 的示例：  
https://www.shadertoy.com/

## SpiderEye

![image.png](https://i.loli.net/2019/09/28/KVNgSjClo3XJrMI.png)

使用 GLIntercept 做的小工具，能让你查看每个缓冲的数据。

**下载**：https://drive.google.com/open?id=1p_FOalR0LzKmQu9negjtaVzobO9YSeiu

v0.1：初步发布

由于该软件的运作会操作游戏的 OpenGL32.dll 文件，[可能会被杀软误报](https://www.virustotal.com/gui/file/ff4e4037cf7dabcb79cbfc4afa7a52bb05e2e11eabe249a3511614cd58b6420c/detection)。

## 着色器示例

![image.png](https://i.loli.net/2019/09/28/JfRG8SFdto9hVWu.png)

[可以看过去的传送门](https://www.reddit.com/r/Minecraft/comments/b15dho/vanilla_portal_gun_in_latest_snapshot_with/)

从一个角度捕获画面并显示

*完整、复杂的着色器运用*

---

![image.png](https://i.loli.net/2019/09/28/uYLwyUVnJz8HGx9.png)

[调试文字](https://drive.google.com/open?id=1n-SjTRv6D7CnYXok7A4cB9sqnSw00wGS)

通过着色器向屏幕上写入文字或数字

*用来调试着色器的工具*

---

![image.png](https://i.loli.net/2019/09/28/APdCim8rUMI1pTz.png)

[屏幕方块](https://www.reddit.com/r/Minecraft/comments/9hj1l3/a_screen_that_displays_what_youre_seeing_in/)

简单的着色器，将 `minecraft:main` 缓存显示在一个方块上

*易于理解的着色器*
