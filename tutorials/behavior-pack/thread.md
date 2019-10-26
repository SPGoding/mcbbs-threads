[index]  
[#1]简介  
[#2]准备  
[#3]开始制作  
[#4]战利品表  
*[#5]附录：函数  
*[#6]附录：条件  
[#7]交易表  
*[#8]附录：函数  
[#9]生成规则  
*[#10]附录：条件  
[#11]实体  
*[#12]附录：滤器  
*[#13]附录：组件  
**[#14]触发器  
**[#829760,13506665]属性  
**[#829760,13501483]行为  
**[#829760,13536864]其他  
*[#15]附录：事件  
[#16]物品  
*[#17]附录：组件  
[#18]配方  
*[#19]有序合成配方  
*[#20]无序合成配方  
*[#21]熔炼配方  
*[#22]混合酿造配方  
*[#23]换容酿造配方  
[#24]方块  
[#25]结构  
[#26]生物群系  
[#27]函数  
[#28]脚本（WIP）  
[#29]发布  
[#30]参考资料  
[#31]后语  
友情提示：点击论坛页面右侧偏下的粉红小书签能快速回到本目录  
[/index]

# 本帖目前仍在进行大幅变动，请几天后再看

# 简介

行为包（behavior pack），是 Minecraft 基岩版（本帖后文除非特别标注，Minecraft 一概指代基岩版）所独有的特性。利用行为包，可以在不改变游戏源代码的情况下修改许多东西，包括：生物群系、方块、结构、物品、配方、实体行为等。行为包与 Add-on 中的另一者「资源包」的区别在于，行为包修改的是服务端层面的内容，而资源包修改的则是客户端层面的内容，因此行为包只需要在服务器中启用，而资源包必须自己安装才能看到效果。

可以看出，行为包是一个非常复杂的机制。我不指望、也不可能在这一帖中用仅仅十几万的字符就彻底讲明白这到底是个什么东西。看完这一帖后我能保证你会对行为包有一个大致的了解，知道它能做到什么，以及怎么做。不过再往后的内容，例如怎么在现有框架内运用各种黑科技做出惊人的效果，就不是我这种大篇幅的教程能教的了，还是需要靠自己研究。研究结果可以直接发布在[基岩版技巧教程版](https://www.mcbbs.net/forum-pe-1.html)，让这个版能有点儿仙气。本萌新在这里期待各位 dalaoes 的成果！

本教程顶部目录可点。本教程同时也在 [GitHub](https://github.com/SPGoding/mcbbs-threads/blob/master/tutorials/behavior-pack/thread.md) 上发布，那边没有目录，不过格式可能更好看一些，看你的喜好了。

本教程使用版本为 1.14.0.2，将来版本可能会在细节上有细微变化，建议看教程时自己对照当时最新版本的原版行为包的格式（通常可以在[英文 Wiki 的 Add-on 页面](https://minecraft.gamepedia.com/Add-on)找到原版行为包的下载地址）。

本教程采用 CC BY-NC-SA 3.0 协议发布，因为其中使用到了不少英文 Wiki 的内容。

[page]

# 准备

## 文本编辑器

行为包中涉及到了大量的 JSON 文件，个人并不推荐使用系统自带的记事本进行编辑，主要是因为没有语法高亮以及缩进，很容易出错。此外旧版本的记事本还有着编码问题。建议下载一个专门的文本编辑器。

个人偏爱微软的 [VSCode](https://code.visualstudio.com/)，如果你已经习惯了诸如 [Notepad++](url=https://notepad-plus-plus.org/download/) 之类的文本编辑器也是可以的。

在编辑文件时，请使用 `UTF-8 无 BOM` 编码，否则可能会出现一些诡异的问题，例如中文乱码、不能读取文件等。目前的文本编辑器都会默认设置为 `UTF-8`，通常不需要手动设置，不过这里还是给出两种编辑器的设置方式。

### VSCode 设置编码方法

输入 `Ctrl + Shift + P`，输入 `>Change File Encoding`，选择 `Save With Encoding`，选择 `UTF-8`。

### Notepad++ 设置编码方法

在上方展开「格式」，选择「转为 UTF-8 编码格式」。

## 显示文件拓展名

为了方便以后处理，强烈建议开启文件拓展名显示。点击「查看」，勾选「文件拓展名」。

![](https://i.loli.net/2018/10/19/5bc9dd909e73d.png)

什么？你不是 Win10 系统？那你看这个教程做什么（

## 预备知识

这里有一些将来会用到的预备知识。

### JSON 文件格式

行为包中涉及了大量 JSON 文件。所谓的 JSON，其实就是一种人和计算机读起来都比较方便的格式，Minecraft 选择了这种格式来定义许多东西。

举个例子（图片中为 VSCode 的默认配色方案）: 

![](https://i.loli.net/2018/10/18/5bc8aa23509a9.png)

JSON 格式包含五种结构，分别是对象、字符串、布尔值、数字、数组。

对象被花括号 `{}` 包裹，里面由**键**和**值**构成，图中浅蓝色高亮的就是键，键**必须使用半角双引号 `""` 包裹，并且 Minecraft 规定所有的键必须使用全小写英文字母与一些特定符号**，值可以是五种结构中的任意一种。

字符串是图中橙色高亮的部分，也就是一串字符，必须使用半角双引号 `""` 包裹。

布尔值是图中深蓝色高亮的部分，可以简单理解为开关，`true` 为开，`false` 为关，没有双引号。

数字是图中浅绿色高亮的部分，没什么好解释的，没有双引号。

数组是用中括号 `[]` 包裹的一系列值，值可以是五种结构（对象、字符串、数字、布尔值、数组）中的任意一种，如图中的 `"family"` 的值就是数组。

顺带一提，图中深绿色高亮的部分是注释。注释会在读取文件时被忽略，可以用来给自己或者其他查看 JSON 的人做备注。然而正经的 JSON 格式其实是不支持注释的，红色波浪线表明这是「错误」，不过 Minecraft 特意支持了注释，游戏读取时并不会真的出错。如果你使用的编辑器是 VSCode，可以点击右下角的「JSON」，在弹出的列表中选择「JSON with Comments (jsonc)」，就不会将注释标记为错误了。

[page]

# 开始制作

`%localappdata%\Packages\Microsoft.MinecraftUWP_8wekyb3d8bbwe\LocalState\games\com.mojang\behavior_packs` 为所有行为包的储存路径。但是，我们**并不在这里**建立我们自己的行为包。

打开 `%localappdata%\Packages\Microsoft.MinecraftUWP_8wekyb3d8bbwe\LocalState\games\com.mojang\development_behavior_packs`，这里放置的是正在开发的行为包。在该目录下新建一个文件夹，它将是你的第一个行为包。Mojang 建议文件名采用英文小写字母 + 下划线分割单词，总长度控制在 10 个字符以内，本教程以 `spg_best` 命名该文件夹。

之所以在 `development_behavior_packs` 内建立我们的行为包，而不是在 `behavior_packs`，是因为游戏会在加载行为包时自动把外部的行为包在存档内做一个缓存。这会导致你对行为包的改动不能实时反应到游戏里，造成调试困难。当你把行为包建立在 `development_behavior_packs` 里面以后，游戏不会进行缓存，想要重载行为包只需要退出世界再重进即可。很遗憾，Minecraft 基岩版没有提供方便的在世界内完全重载行为包的方式。

接下来你需要在行为包内的根目录放置一个 `manifest.json` 文件，否则游戏不会把它当作行为包。

## manifest.json 格式

- `format_version`：（数字）必须。格式版本，只能为 2。
- `header`：（对象）必须。头部信息。
    - `name`：（字符串）可以是任意字符，会显示在游戏的列表里。
    - `description`：（字符串）可以是任意字符，会在列表里显示在名称的下方。建议长度保持在 1-2 行。
    - `uuid`：（字符串）该附加包的通用唯一识别码，被 Minecraft 用来识别该附加包，因此不能与其他任何附加包重复。该字符串的格式为 `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`，其中 `x` 表示一个 16 进制的数字。可以在[该网站](https://1024tools.com/uuid)生成。
    - `version`：（数组 `[a, b, c]`）该附加包的版本，分为主版本号、次版本号、修正版本号三部分。如写为 `[0, 0, 1]` 会在游戏中以 `0.0.1` 的形式显示。当导入一个之前被导入过的附加包时，如果正在导入的附加包的版本号比已被导入的版本号高，则会用正在导入的附加包替换掉原先导入的；如果版本号一致或更低，则会被忽略。
    - `min_engine_version`：（数组 `[a, b, c]`）该附加包所适用的最低游戏版本，分为主版本号、次版本号、修正版本号三部分。如写为 `[1, 14, 0]` 表明该附加包只能 1.14.0 及以上版本的游戏中使用。
- `modules`：（数组）必须。组成该附加包的各个模块。
    - *（对象）一个模块*
        - `uuid`：（字符串）该模块的通用唯一标识码。格式与 `header` 中的 `uuid` 一致。该值需要与 `header` 中以及其他各模块中的定义的值不同。
        - `description`：（字符串）对该模块的描述。目前不会显示在任何地方，不过可以提醒你自己这是干什么的。
        - `version`：（数组 `[a, b, c]`）模块版本。格式与 `header` 中的 `version` 相同。可以通过修改它来更精确地描述你的附加包变动了哪些模块。
        - `type`：（字符串）模块类型。`data` 为行为包（此外还能填写 `resources`、`client_data`、`interface` 和 `world_template`，与本教程关联不大）。
- `dependencies`：（数组）可选。被该附加包依赖的包。如果设置了这一项，表明你的附加包需要在这些依赖包存在的情况下才能正常运作。当你把你的附加包添加到世界中时，如果游戏目录下有依赖包，游戏会自动把依赖包也添加进去；如果没有找到依赖包则会报错。
    - *（对象）一个依赖包*
        - `uuid`：（字符串）依赖包的通用唯一标识码。需要与依赖包的 `manifest.json` 文件中 `header` 内定义的完全一致。
        - `version`：（数组 `[a, b, c]`）你的附加包所依赖的包的版本。
        - `type`：（字符串）模块类型。`data` 为行为包（此外还能填写 `resources`、`client_data`、`interface` 和 `world_template`，与本教程关联不大）。
- `metadata`：（对象）可选。用于为你的附加包添加更多信息。
    - `authors`：（数组）该附加包的作者（们）的名字。
        - *（字符串）一个作者的名字*
    - `license`：（字符串）该附加包的协议。
    - `url`：（字符串）有关该附加包的网站链接。

下面是一个行为包的示例 manifest.json 格式。

```json
{
    "format_version": 2,
    "header": {
        "name": "SPGoding Best",
        "description": "spg 最棒了",
        "uuid": "b64e9ef0-f275-454f-a06f-d023185a29f3",
        "version": [0, 0, 1],
        "min_engine_version": [1, 14, 0]
    },
    "modules": [
        {
            "uuid": "243b7fd8-76a2-407b-afb5-f7d6db375c89",
            "description": "这个是行为包",
            "version": [0, 0, 1],
            "type": "data"
        }
    ],
    "metadata": {
        "authors": [
            "SPGoding"
        ],
        "license": "WTFPL",
        "url": "https://www.mcbbs.net/thread-829760-1-1.html"
    }
}
```

保存文件以后你的行为包就会显示在列表中了。

![image.png](https://i.loli.net/2019/10/23/T1hLlkHqYSIAsBK.png)

可以看到图标是一个紫黑块。我们这就加上自己的图标。

## pack_icon.png 文件

在我们的行为包根目录再放一个 pack_icon.png 图片文件，大小为 64*64。我比较懒，就把默认行为包的图标垂直翻转了一下（这个其实还是旧版本的默认图标，新版已经改成一个立体的草方块了。没事儿，问题不大）。

![](https://i.loli.net/2018/10/19/5bc9ef510185b.png)

重启游戏，现在看起来好多了（并没有

![image.png](https://i.loli.net/2019/10/23/CQU83E4pfsLNVuK.png)

## 应用到一个世界

在世界选项中可以选择我们自己的行为包并启用。

![image.png](https://i.loli.net/2019/10/23/FcY5KL4OPD1rCei.png)

[page]

# 战利品表

战利品表可以用来定义宝箱/实体/游戏行为等各种东西所能给玩家带来的物品，以及指定实体所能穿的装备等。

原版放置战利品表的位置是在行为包根目录下的 `loot_tables` 文件夹内。事实上，你可以放在行为包里任何你想要的地方，但建议大家按传统习惯行事。

在 Java 版中同样也有战利品表，隔壁的教程也是我写的，有兴趣可以对比一下：[Java 版战利品表：从入门到重新入门](https://www.mcbbs.net/thread-831542-1-1.html)。

## 简述

![image.png](https://i.loli.net/2019/10/23/4b2a8QiIMvVWFx6.png)

## JSON 格式

- `pools`：（数组）随机池们。游戏会从每一个随机池中**逐个**抽取物品。
    - *（对象）一个随机池*
        - `conditions`：（数组）可选。使用该随机池所需满足的条件列表。
            - *（对象）一个条件（有关条件的具体内容见下方介绍）*
        - `tiers`：（对象）可选。如果指定，将会忽略掉下方的 `rolls` 以及各项中的 `weight` 和 `quality`，把 `entries` 中的每一项看作一个层级。`entries` 中的第一个对象为第一层，第二个对象为第二层，以此类推。这几行的地方可能讲不明白，如果看不懂下面这三个参数的描述的话，下面有例子。
            - `initial_range`：（数字）初始的选取范围。游戏将会默认从第 1 层一直到这一参数中指定的层中随机抽取一项。例如，把该参数设置为 `2`，则游戏会默认从 `entries` 中的第 1 项和第 2 项中随机抽取一个选择。由于设置了 `tiers` 后会忽略掉 `weight` 和 `quality`，每一项被选中的几率都是 50%。
            - `bonus_chance`：（数字）将被选取的层级数再加上定义在 `bonus_rolls` 中的奖励的层数的几率。取值应在 [0.0, 1.0] 中。
            - `bonus_rolls`：（数字）奖励层数。必须大于等于 `1`。当游戏根据 `initial_range` 定义的范围从 `entries` 中选取一层以后，有一定几率（该几率定义在 `bonus_chance` 中）把已经选取的层数再加上这个数字。
        - `rolls`：（整数）可选。从该随机池中抽取物品的次数。
        - `entries`：（数组）可以从该随机池中抽取的所有项。如果指定了 `rolls`，游戏会从这些项中随机抽取**一个**；如果指定了 `tiers`，则会按照层级的特性随机抽取**一个**。
            - *（对象）一项*
                - `type`：（字符串）该项的类型，用于决定下面 `name` 参数的解析方式。`item` 表示是个物品，`loot_table` 表示是另一个战利品表，`empty` 表示什么都没有。
                - `name`：（字符串）名字。会根据上面 `type` 的值来解析。例如 `type` 为 `item`，则会将此字段按照物品名解析。
                - `weight`：（整数）该项的权重。从随机池中抽到该项的几率为 `weight / (该随机池中所有满足条件的事物的 weight 的和)`。
                - `quality`：（整数）可选。根据玩家的幸运等级影响该 `weight`。计算公式为，`floor(weight + (quality * <幸运等级>))`，其中 `floor` 为向下取整。
                - `pools`：（数组）可选。该项的子随机池。在抽出该项后，游戏会从该项的这个子随机池中继续抽取物品。
                    - *（对象）一个随机池。格式和根对象中随机池的格式完全一致。*
                - `conditions`：（数组）可选。抽出该项所需满足的条件列表。
                    - *（对象）一个条件（有关条件的具体内容见下方介绍）*
                - `functions`：（函数）可选。抽出该项时对此项执行的战利品表函数（请与命令的函数做区分）。
                    - *（对象）一个函数*
                        - `function`：（字符串）该函数的名称（有关函数的具体内容见下方介绍）。
                        - `conditions`：（数组）执行该函数所需满足的条件列表。
                            - *（对象）一个条件（有关条件的具体内容见下方介绍）*

## 学习原版

以下是原版中僵尸的战利品表 `./loot_tables/entities/zombie.json`。注释是我自己加的。

```json
{
    "pools": [
        {
            // 第一个随机池。
            "rolls": 1, // 指定了 rolls，因此会从下面的 entries 中随机抽取一项。
            "entries": [
                {
                    // 第一个随机池里的第一项。
                    "type": "item", // 是个物品。
                    "name": "minecraft:rotten_flesh", // 物品 ID 是腐肉的。
                    "weight": 1, // 权重是 1。这个是几其实无所谓，因为这个随机池里面只有这一项，肯定会抽中这一项的。
                    "functions": [
                        {
                            // 对这个腐肉执行的第一个函数。
                            "function": "set_count", // 这个函数的作用是设置物品数量。详细函数列表可以看下面。
                            "count": { // 数量随机设置为 0-2 中的一个。如果设置为 0，表明不会掉落。
                                "min": 0,
                                "max": 2
                            }
                        },
                        {
                            // 对这个腐肉执行的第二个函数。
                            // 这个函数的作用是，玩家的武器每有一级抢夺附魔，就多掉落 0-1 个物品。
                            "function": "looting_enchant", 
                            "count": {
                                "min": 0,
                                "max": 1
                            }
                        }
                    ]
                }
            ]
        },
        {
            // 第二个随机池
            "conditions": [
                // 这个随机池设定了条件。只有以下条件全部满足时，才会从这个随机池中抽取物品。
                {
                    // 条件 1：僵尸是被玩家杀死的。
                    "condition": "killed_by_player"
                },
                {
                    // 条件 2：有 2.5% 的概率达成。玩家的武器每有一级抢夺附魔，就加 1% 的概率。
                    "condition": "random_chance_with_looting",
                    "chance": 0.025,
                    "looting_multiplier": 0.01
                }
            ],
            "rolls": 1,
            "entries": [
                // 当上述两个条件全部满足后，从这三项里随机抽取一个掉落。
                // 这三项的权重 weight 都是 1，因此这三个掉落的概率相等。
                // 这三项结构很简单，就不写注释了。
                {
                    "type": "item",
                    "name": "minecraft:iron_ingot",
                    "weight": 1
                },
                {
                    "type": "item",
                    "name": "minecraft:carrot",
                    "weight": 1
                },
                {
                    "type": "item",
                    "name": "minecraft:potato",
                    "weight": 1
                }
            ]
        }
    ]
}
```

这整个战利品表实现了僵尸的物品的掉落，简单描述为：一般会掉落腐肉，偶尔会再多掉一个铁锭/胡萝卜/马铃薯。与我们平时认知的一样。

## Tiers 层级到底是什么

这是上面僵尸的战利品表，经过我修改后的样子，使用到了 `tiers`：

```json
{
    "pools": [
        {
            "tiers": {
                "initial_range": 2,
                "bonus_rolls": 1,
                "bonus_chance": 0.01
            },
            "entries": [
                {
                    "type": "item",
                    "name": "minecraft:rotten_flesh"
                },
                {
                    "type": "item",
                    "name": "minecraft:carrot"
                },
                {
                    "type": "item",
                    "name": "minecraft:iron_ingot"
                }
            ]
        }
    ]
}
```

`entries` 中定义的这三项变成了三个层级。`rotten_flesh`（腐肉）是第 1 层，`carrot`（胡萝卜）是第 2 层，`iron_ingot`（铁锭）是第 3 层。

当游戏执行这个战利品表时，会首先从第 1 层到第 `initial_range` 层中随机抽取一层。以这个 JSON 为例，则是随机从 `rotten_flesh`（腐肉）和 `carrot`（胡萝卜）中随机抽取一个，这两个被抽到的概率都是 50%。

当抽取好以后，又有 `bonus_chance` 的几率把这一层数加上 `bonus_rolls`。以这个 JSON 为例，有 1% 的几率把层数加 `1`。例如，原先抽取到了 `rotten_flesh`（腐肉），那么有 1% 的几率会返回 `carrot`（胡萝卜）；原先抽取到了 `carrot`（胡萝卜），那么有 1% 的几率会返回 `iron_ingot`（铁锭）。

Tiers 这一特性原版主要将其用于生成怪物的盔甲。这是 `loot_tables/entities/skeleton_gear.json` 的一部分，它用于生成骷髅的盔甲：

```json
{
    "tiers": {
        "initial_range": 2,
        "bonus_rolls": 3,
        "bonus_chance": 0.095
    },
    "entries": [
        {
            "type": "loot_table",
            "name": "loot_tables/entities/armor_set_leather.json"
        },
        {
            "type": "loot_table",
            "name": "loot_tables/entities/armor_set_gold.json"
        },
        {
            "type": "loot_table",
            "name": "loot_tables/entities/armor_set_chain.json"
        },
        {
            "type": "loot_table",
            "name": "loot_tables/entities/armor_set_iron.json"
        },
        {
            "type": "loot_table",
            "name": "loot_tables/entities/armor_set_diamond.json"
        }
    ]
}
```

这一战利品表使得骷髅的盔甲通常为 `armor_set_leather`（皮革）或 `armor_set_gold`（金甲），但有 9.5% 的几率会变为 `armor_set_iron`（铁甲）或 `armor_set_diamond`（钻石甲）。

Tiers 这个略为复杂的东西扯完了。

[spoiler]以下是我自己用的东西，放别的地方怕被我弄丢了。没什么用，折叠吧。
```typescript
import * as fs from 'fs'
import { join } from 'path'

const walker = (path: string, cb: (content: string) => any) => {
    const files = fs.readdirSync(path)
    for (const file of files) {
        const child = join(path, file)
        if (fs.statSync(child).isDirectory()) {
            walker(child, cb)
        } else if (child.endsWith('.json')) {
            cb(fs.readFileSync(child, { encoding: 'utf8' }))
        }
    }
}

const arr: string[] = []

walker(__dirname, (content: string) => {
    const trim = (val: string, prefix = 'minecraft:') => val.startsWith(prefix) ? val.slice(prefix.length) : val
    const regex = /"function": "(.*?)"/g
    let match = regex.exec(content)
    while (match) {
        if (arr.indexOf(trim(match[1])) === -1) {
            arr.push(trim(match[1]))
        }
        match = regex.exec(content)
    }
})

console.log(arr.length)
console.log(arr.sort().join('\n'))
```
[/spoiler]

## 自造轮子

我想让僵尸在被烧死的时候掉烈焰粉！

所有你需要的战利品表函数、条件都可以在附录里找到。不过如果找不到，可以考虑用别的方案替代。

首先，我们要修改的是原版就有的僵尸的战利品表，比较便捷的做法是直接在你的行为包中用同名同路径的文件把它覆盖掉。创建 `loot_tables/entities/zombie.json`，把原版战利品表中的内容复制进去（你可以直接从我开头「学习原版」部分的代码框里复制，注释删不删无所谓，因为就像我开篇说的那样，Minecraft 会自动忽略掉那些注释）。

接下来就可以加烈焰粉了。在原来的两个随机池下面多加一个随机池（注意逗号），设置条件为 `entity_properties`，指定 `entity` 为 `this`（即探测僵尸自身），指定 `on_fire` 为 `true`（即需要被探测的实体着火）。设置给予的物品为 `minecraft:blaze_powder`（烈焰粉），数量从 1 至 2 随机。即: 

```
{
    "pools": [
        { ... }, 
        { ... }, // 原有的两个随机池，这里省略不写。
        {
            "conditions": [
                {
                    "condition": "entity_properties",
                    "entity": "this",
                    "properties": {
                        "on_fire": true
                    }
                }
            ],
            "rolls": 1,
            "entries": [
                {
                    "type": "item",
                    "name": "minecraft:blaze_powder",
                    "weight": 1,
                    "functions": [
                        {
                            "function": "set_count",
                            "count": {
                                "min": 1,
                                "max": 2
                            }
                        }
                    ]
                }
            ]
        }
    ]
}
```

退出再重进世界。

可以看到僵尸被烧死后产生了烈焰粉。

![](https://i.loli.net/2018/10/20/5bca0ae304212.gif)

（这张动图是上次写这篇教程时候录的，大体效果没错，就不重新录制了）

好的，一个悲伤的故事是，僵尸就算不是被烧死的也会掉烈焰粉。这并不是我的问题，我已经将该漏洞反馈：[MCPE-54162](https://bugs.mojang.com/browse/MCPE-54162)。哦对了，顺带一提，本教程所有「自造轮子」节的内容最后都会打包到一个行为包里面，行为包会放置在「发布」章节，你需要的话可以先去那个章节下载。

## 课后习题

让玩家钓鱼能钓上来有随机附魔的钻石剑吧！

提示: 可以在原版的行为包里找钓鱼相关战利品表的位置（找不到的话我告诉你，主钓鱼战利品表是 `loot_tables/gameplay/fishing.json`，它又引用了别的战利品表。带有随机附魔的钻石剑可以被认为是宝物，因此你需要修改的是钓鱼出宝物的战利品表，即 `loot_tables/gameplay/fishing/treasure.json`），在本人整理的附录中找到你需要的函数。

本节完，点击论坛右侧的书签可以快速回到顶部。

[page]

# 附录：函数

下面是战利品表中可用的全部函数。这些函数可以根据需求添加到每一项的 `functions` 数组中。函数的名称可以带 `minecraft:` 前缀，也可以不带，看你的喜好。例如，`set_count` 和 `minecraft:set_count` 都是表示设置物品数量的函数，执行起来没有区别。

在开始介绍前，先简单说明一下「数字或范围」这一格式的意思。这一格式在之后的章节中也有可能会出现，到时就不再赘言了。

### 数字或范围

很简单。当一个参数的类型被我写为「数字或范围」时，表明它既可以是一个精确的数字，也可以是一个形如 `{ "min": 最小值, "max": 最大值 }` 的表明随机数的对象。

### enchant_random_gear

对装备随机附魔。Minecraft 使用战利品表来决定实体在生成时所装备的物品，每个生物具体使用哪个战利品表，可以在实体行为中定义（有关实体行为的具体内容，见「实体行为」章节）。

- `chance`：（数字）进行附魔的概率。例如 `0.25` 表示 `25%`。

示例（`loot_tables/entities/armor_set_chain.json`）：
```json
{
    "function": "enchant_random_gear",
    "chance": 0.25
}
```

### enchant_randomly

对物品进行随机附魔。

- `treasure`：（布尔值）是否给予宝藏魔咒。

示例（`loot_tables/chests/pillager_outpost.json`）：
```json
{
    "function": "minecraft:enchant_randomly"
}
```

### enchant_with_levels

对物品进行相当于使用了指定经验等级的附魔。

- `treasure`：（布尔值）是否给予宝藏魔咒。
- `levels`：（数字或范围）要设置的等级。

示例（`loot_tables/chests/end_city_treasure.json`）：
```json
{
    "function": "enchant_with_levels",
    "treasure": true,
    "levels": {
        "min": 20,
        "max": 39
    }
}
```

### exploration_map

将该物品变为探险地图。指定的物品需要是地图。

- `destination`：（字符串）地图的目的地。需要是一个结构的 ID。全部结构的 ID 可以在 [Wiki](https://minecraft-zh.gamepedia.com/%E5%91%BD%E4%BB%A4/locate) 上查看。

示例（`loot_tables/chests/shipwreck.json`）：
```json
{
    "function": "exploration_map",
    "destination": "buriedtreasure"
}
```

### furnace_smelt

对该物品进行熔炼。得到的结果与将物品放置熔炉中熔炼产生的结果一致。为该函数添加 `entity_properties` 条件可以做到实体被烧死时掉落熟食。

示例（`loot_tables/entities/chicken.json`）：
```json
{
    "function": "furnace_smelt"
}
```

### looting_enchant

设置抢夺魔咒对数量的影响。

- `count`：（数字或范围）每一级抢夺魔咒增加的数量。
- `limit`：（数字）抢夺魔咒所增加的数量的上限。

示例（`loot_tables/entities/blaze.json`）：
```json
{
    "function": "looting_enchant",
    "count": {
        "min": 0,
        "max": 1
    }
}
```

### random_aux_value

随机生成一个辅助数值。不明。

- `values`：（数字或范围）要设置的值。

示例（`loot_tables/chests/shipwrecksupply.json`）：
```json
{
    "function": "random_aux_value",
    "values": {
        "min": 0,
        "max": 6
    }
}
```

### set_banner_details

设置旗帜的图案。指定的物品需要是旗帜。

- `type`：（数字）旗帜图案的类型。已知数值只有 `1`，代表掠夺者首领头上的旗帜。

示例（`loot_tables/entities/pillager_captain_equipment.json`）：
```json
{
    "function": "set_banner_details",
    "type": 1
}
```

### set_count

设置物品的数量。

- `count`：（数字或范围）要设置的数量。

示例（`loot_tables/chests/buriedtreasure.json`）：
```json
{
    "function": "set_count",
    "count": {
        "min": 1,
        "max": 5
    }
}
```

### set_damage

设置物品的损坏率。

- `damage`：（数字或范围）要设置的损坏率。取值应在 [0, 1] 之间。

示例（`loot_tables/entities/drowned.json`）：
```json
{
    "function": "set_damage",
    "damage": {
        "min": 0.2,
        "max": 0.9
    }
}
```

### set_data

设置物品的数据值。

- `data`：（数字）要设置的数据值。

示例（`loot_tables/chests/buriedtreasure.json`）：
```json
{
    "function": "set_data",
    "data": 19
}
```

### set_data_from_color_index

根据当前实体的颜色设置物品的数据值。只有在当前实体为羊且物品具有颜色（如羊毛、混凝土等），或当前实体为哞菇且当前物品为蘑菇时有用。

示例（`loot_tables/entities/sheep.json`）：
```json
{
    "function": "minecraft:set_data_from_color_index"
}
```

[page]

# 附录：条件

当你指定的条件全部达成时，才有可能会使用你所设置的随机池（`pool`）、项（`entry`）或是函数（`function`）。下面是战利品表中可用的全部条件。

## entity_properties

指定实体的属性匹配时达成条件。


- `entity`：（字符串）要检测的实体。`this` 指代死亡的这个实体，`killer` 指代杀了这个实体的实体，`killer_player` 指代杀了这个实体的玩家。
- `properties`：（对象）要检测的属性。
    - `on_fire`：（布尔值）指定实体是否着火。

示例（`loot_tables/entities/chicken.json`）：
```json
{
    "condition": "entity_properties",
    "entity": "this",
    "properties": {
        "on_fire": true
    }
}
```

## has_mark_variant

检测该实体是否是某个变种。不明。

- `value`：（数字）

示例（`loot_tables/entities/mooshroom_milking.json`）：
```json
{
    "condition": "has_mark_variant",
    "value": -1
}
```

## killed_by_entity

被指定类型的实体击杀时达成条件。

- `entity_type`：（字符串）指定实体的 ID。

示例（`loot_tables/entities/creeper.json`）：
```json
{
    "condition": "killed_by_entity",
    "entity_type": "minecraft:skeleton"
}
```

## killed_by_player

被玩家击杀时达成条件。

示例（`loot_tables/entities/cave_spider.json`）：
```json
{
    "condition": "killed_by_player"
}
```

## killed_by_player_or_pets

被玩家或玩家的宠物击杀时达成条件。

示例（`loot_tables/entities/blaze.json`）：
```json
{
    "condition": "killed_by_player_or_pets"
}
```

## random_chance

有一定几率达成。

- `chance`：（数字）条件达成的几率。取值应当在 [0, 1] 的范围内。

示例（`loot_tables/entities/drowned_equipment.json`）：
```json
{
    "condition": "random_chance",
    "chance": 0.01
}
```

## random_chance_with_looting

有一定几率达成。该几率可以被「抢夺」附魔提高。

- `chance`：（数字）条件达成的基础几率。取值应当在 [0, 1] 的范围内。
- `looting_multiplier`：（数字）每一级「抢夺」魔咒所增加的几率。取值应当在 [0, 1] 的范围内。

示例（`loot_tables/entities/drowned.json`）：
```json
{
    "condition": "random_chance_with_looting",
    "chance": 0.11,
    "looting_multiplier": 0.02
}
```

## random_difficulty_chance

有一定几率达成。可以指定在不同难度下的几率。

- `default_chance`：（数字）条件达成的默认几率。取值应当在 [0, 1] 的范围内。
- `peaceful`：（数字）和平模式下条件达成的几率。取值应当在 [0, 1] 的范围内。
- `easy`：（数字）简单模式下条件达成的几率。取值应当在 [0, 1] 的范围内。
- `normal`：（数字）普通模式下条件达成的几率。取值应当在 [0, 1] 的范围内。
- `hard`：（数字）困难模式下条件达成的几率。取值应当在 [0, 1] 的范围内。

示例（`loot_tables/entities/armor_set_chain.json`）：
```json
{
    "condition": "random_difficulty_chance",
    "default_chance": 0.50,
    "peaceful": 0,
    "hard": 0.6
}
```

## random_regional_difficulty_chance

有一定几率达成。该几率随着区域难度提高而提高。

- `max_chance`：（数字）条件达成的最大几率。取值应当在 [0, 1] 的范围内。

示例（`loot_tables/entities/skeleton_gear.json`）：
```json
{
    "condition": "random_regional_difficulty_chance",
    "max_chance": 0.15
}
```

[page]

# 交易表

行为包可以定义村民的交易表。

原版放置村民交易表的位置是在根目录下的 `trading` 文件夹内。事实上，你可以放在行为包里任何你想要的地方，但建议大家按传统习惯行事。

## 简述

简单介绍一下村民交易的机制：村民的交易分为多个层级。首先只会解锁第一个层级的交易。当交易几次以后，会解锁下一个层级的交易。如此反复，直到所有层级的交易都解锁了为止。

## JSON 格式

- `tiers`：（数组）储存该交易表中的所有交易层级。从上到下层级逐渐增高。
    - *（对象）一层交易*
        - `trades`：（数组）该层中具有的所有交易。
        - *（对象）一个交易*
            - `wants`：（数组）村民想要的物品。项数应小于等于 2，超出的物品会被无视。
                - *（对象）一个物品*
                    - `item`：（字符串）物品 ID，如果有数据值，使用冒号分割。
                    - `quantity`：（数字或范围）物品数量。
            - `gives`：（数组）村民给予的物品。会从中随机抽取。
                - *（对象）一个物品*
                    - `item`：（字符串）物品 ID，如果有数据值，使用冒号分割。
                    - `quantity`：（数字或范围）物品数量。
                    - `functions`：（数组）交易表函数。
                        - *（对象）一个函数*
                            - `function`：（字符串）使用的函数。
                            - 其他参数（有关函数的具体内容见下文）。

## 学习原版

这次我们以渔夫的交易 `trading/fisherman_trades.json` 为例。删除了一小点儿内容，不然太长了。

```
{
    "tiers": [
        {
            // 第一层交易
            "trades": [
                {
                    // 第一层交易的第一个交易：
                    "wants": [
                        // 该交易接受：
                        {
                            // 6 条鱼。
                            "item": "minecraft:fish",
                            "quantity": 6
                        },
                        {
                            // 1 个绿宝石。
                            "item": "minecraft:emerald",
                            "quantity": 1
                        }
                    ],
                    "gives": [
                        // 该交易给出：
                        {
                            // 6 条熟鱼。
                            "item": "minecraft:cooked_fish",
                            "quantity": 6
                        }
                    ]
                },
                {
                    // 第一层交易的第二个交易：
                    "wants": [
                        // 该交易接受：
                        {
                            // 15-20 根线，具体数量由游戏随机生成。
                            "item": "minecraft:string",
                            "quantity": {
                                "min": 15,
                                "max": 20
                            }
                        }
                    ],
                    "gives": [
                        // 该交易给出：
                        {
                            // 1 个绿宝石。
                            "item": "minecraft:emerald",
                            "quantity": 1
                        }
                    ]
                }
            ]
        },
        {
            // 第二层交易
            "trades": [
                {
                    // 第二层交易的第一个交易
                    "wants": [
                        // 该交易接受：
                        {
                            // 7-8 个绿宝石，具体数量由游戏随机生成。
                            "item": "minecraft:emerald",
                            "quantity": {
                                "min": 7,
                                "max": 8
                            }
                        }
                    ],
                    "gives": [
                        // 该交易给出：
                        {
                            // 1 个用相当于 5-19 级经验附魔过的钓鱼杆。具体用几级经验由游戏随机决定。
                            "item": "minecraft:fishing_rod",
                            "quantity": 1,
                            "functions": [
                                {
                                    "function": "enchant_with_levels",
                                    "treasure": false,
                                    "levels": {
                                        "min": 5,
                                        "max": 19
                                    }
                                }
                            ]
                        }
                    ]
                }
            ]
        }
    ]
}
```

## 自造轮子

让奸商渔夫第三层交易为，绿宝石 + 钻石换取超级普通的钓鱼竿！

由于我们要修改的是原版就有的交易表，比较便捷的做法是直接在你的行为包中用同名同路径的文件把它覆盖掉。创建 `loot_tables/trading/fisherman_trades.json`，把原版交易表中的内容复制进去。

接下来就可以加第三层交易了。在原有的两个交易层级后继续加入一个对象，设置 `wants` 为 `minecraft:emerald`（绿宝石）和 `minecraft:diamond`（钻石），`gives` 为 `minecraft:fishing_rod`。即

```json
{
    "tiers": [
        { ... },
        { ... }, // 原有的两个交易层级
        {
            "trades": [
                {
                    "wants": [
                        {
                            "item": "minecraft:emerald",
                            "quantity": {
                                "min": 10,
                                "max": 14
                            }
                        },
                        {
                            "item": "minecraft:diamond",
                            "quantity": {
                                "min": 5,
                                "max": 7
                            }
                        }
                    ],
                    "gives": [
                        {
                            "item": "minecraft:fishing_rod",
                            "quantity": 1
                        }
                    ]
                }
            ]
        }
    ]
}
```

退出再重进世界。搞定。

![](https://i.loli.net/2018/10/20/5bca1fee0a1b0.png)

## 课后习题

让 shepherd 村民（交易表位于 `trading/shepherd_trades.json`）可以拿羊毛换对应颜色的混凝土！【好无厘头的题目，没办法我只是想讲方块数据值。

提示: 羊毛使用方块数据值来区分颜色。在写 `item` 时，把方块数据值用冒号 `:` 连接在方块 ID 的后面。例如: `minecraft:wool:0`。

[page]

# 附录：函数

在给出交易物品前会执行指定的函数对物品进行操作。下面是交易表中可用的全部函数。

## enchant_book_for_trading

给书附魔。

- `base_cost`：（数字）不明。个人猜测可能和玩家使用附魔台附魔物品所消耗的经验等级有关。
- `base_random_cost`：（数字）不明。
- `per_level_random_cost`：（数字）不明。
- `per_level_cost`：（数字）不明。

示例（`trading/librarian_trades.json`）：
```json
{
    "function": "enchant_book_for_trading",
    "base_cost": 2,
    "base_random_cost": 5,
    "per_level_random_cost": 10,
    "per_level_cost": 3
}
```

## enchant_with_levels

进行相当于指定经验等级的附魔。

- `treasure`：（布尔值）如果为 `true`，将会给予宝藏魔咒。
- `levels`：（数字或范围）要设置的等级。

示例（`trading/leather_worker_trades.json`）：
```json
{
    "function": "enchant_with_levels",
    "treasure": false,
    "levels": {
        "min": 5,
        "max": 19
    }
}
```

## exploration_map

绘制探险地图。

- `destination`：字符串。地图的目的地。需要是一个结构的 ID。全部结构的 ID 可以在 [Wiki](https://minecraft-zh.gamepedia.com/%E5%91%BD%E4%BB%A4/locate) 上查看。

示例（`trading/cartographer_trades.json`）：
```json
{
  "function": "exploration_map",
  "destination": "monument"
}
```

## random_aux_value

随机生成一个辅助数值。不明。

- `values`：（数字或范围）要设置的值。

示例（`trading/economy_trades/wandering_trader_trades.json`）：
```json
{
    // 这是给染料 minecraft:dye 设置的
    "function": "random_aux_value",
    "values": {
        "min": 0,
        "max": 15
    }
}
```

## random_block_state

随机设置方块状态。

- `block_state`：（字符串）要设置的方块状态名。
- `values`：（数字或范围）要设置的方块状态的值。

示例（`trading/economy_trades/wandering_trader_trades.json`）：
```json
{
    // 这是给树苗 minecraft:sapling 设置的
    "function": "random_block_state",
    "block_state": "sapling_type",
    "values": {
        "min": 0,
        "max": 5
    }
}
```

[page]

# 生成规则

行为包可以设置实体的生成规则。只有在满足了生成规则的情况下，该实体才有可能会生成。

放置生成规则的位置是在根目录下的 `spawn_rules` 文件夹内。

## 简述

![image.png](https://i.loli.net/2019/10/23/yKEd8uxQpl9ODXV.png)

## JSON 格式

- `format_version`：（字符串）文件格式版本。本帖一律写为 `1.14.0`。
- `minecraft:spawn_rules`：（对象）表明该文件定义了生成规则。
    - `description`：（对象）描述。
        - `identifier`：（字符串）该生成规则的 ID。在实体的定义文件中可以通过指定该 ID 来使用该规则。
        - `population_control`：（字符串）实体数量控制。为了避免卡顿，Minecraft 会分别限制不同种类的实体的总数量。该参数正是被用于确定实体种类的。可填写`ambient`（环境，如蝙蝠）、`animal`（动物，如蜜蜂、羊等）、`cat`（猫）、`monster`（怪物，如僵尸等）、`pillager`（掠夺者）、`water_animal`（水生动物，如鱼等）。（Wiki 此处内容严重不全）
    - `conditions`：（数组）生成条件。只需达成一组条件即会生成。
        - *（对象）一组条件（有关条件的具体内容见下方介绍）。需要达成这下面的所有的条件才算达成了这一组条件。*
            - `minecraft:条件 1`：（对象）一个条件。
            - `minecraft:条件 2`：（对象）一个条件。
            - ···

## 学习原版

以下是僵尸的生成规则 `./spawn_rules/zombie.json`。

```json
{
    "format_version": "1.8.0",
    "minecraft:spawn_rules": {
        "description": {
            // 该生成规则的 ID 为 minecraft:zombie。
            // 在僵尸的实体定义文件中可以通过 minecraft:zombie 来引用这个生成规则。
            "identifier": "minecraft:zombie",
            // 生成与否受到怪物总数的控制。
            "population_control": "monster"
        },
        "conditions": [
            {
                // 一组条件。只有这些条件全部满足才能生成。
                // 具体的条件列表可在下方查看。
                "minecraft:spawns_on_surface": {},
                "minecraft:spawns_underground": {},
                "minecraft:brightness_filter": {
                    "min": 0,
                    "max": 7,
                    "adjust_for_weather": true
                },
                "minecraft:difficulty_filter": {
                    "min": "easy",
                    "max": "hard"
                },
                "minecraft:weight": {
                    "default": 100
                },
                "minecraft:herd": {
                    "min_size": 2,
                    "max_size": 4
                },
                "minecraft:permute_type": [
                    {
                        "weight": 95
                    },
                    {
                        "weight": 5,
                        "entity_type": "minecraft:zombie_villager"
                    }
                ],
                "minecraft:biome_filter": {
                    "test": "has_biome_tag", "operator": "==", "value": "monster"
                }
            }
        ]
    }
}
```

## 自造轮子 & 课后习题

……我不知道该造什么，也不知道该出什么题，那就咕了吧。看看附录里面有没有什么你感兴趣的条件（

[page]

# 附录：条件

当达成指定条件时，实体才会生成。

特别指明，此处的条件必须要加 `minecraft:` 命名空间前缀，否则不会被读取。

## minecraft:biome_filter

限制允许该实体生成的生态群系。其格式与滤器一致。有关滤器的详细内容，请看「实体」章节下的「附录：滤器」部分。

示例（`spawn_rules/bat.json`）：
```json
{
    "minecraft:biome_filter": {
        "test": "has_biome_tag", "operator":"==", "value": "animal"
    }
}
```

## minecraft:brightness_filter

限制允许该实体生成的光照等级范围。

- `min`：（数字）指定该实体能够生成的最低光照等级。
- `max`：（数字）指定该实体能够生成的最高光照等级。
- `adjust_for_weather`：（布尔值）指定天气是否能够影响亮度，进而让该实体生成（可用于让敌对生物在下雨的白天生成）。

示例（`spawn_rules/bat.json`）：
```json
{
    "minecraft:brightness_filter": {
        "min": 0,
        "max": 4,
        "adjust_for_weather": true
    }
}
```

## minecraft:delay_filter

限制两次尝试生成之间的间隔。

- `min`：（数字）两次尝试生成的最短间隔。单位为秒。
- `max`：（数字）两次尝试生成的最长间隔。单位为秒。
- `identifier`：（数字）不明。推测是一个计时器的标识符。
- `spawn_chance`：（数字）每次尝试成功的几率。取值应当在 [0, 100] 之间。

示例（`spawn_rules/pillager_patrol.json`）：
```json
{
    "minecraft:delay_filter": {
        "min": 600,
        "max": 660,
        "identifier": "minecraft:pillager_patrol_easy",
        "spawn_chance": 20
    }
}
```
该条件使得灾厄巡逻队会每 10 至 11 分钟尝试生成一次，每次尝试生成有 20% 的几率成功。

## minecraft:density_limit

限制实体的密度。

- `surface`：（数字）在地面上的密度限制。
- `underground`：（数字）在地下的密度限制。

示例（`spawn_rules/bat.json`）：
```json
{
    "minecraft:density_limit": {
        "surface": 5
    }
}
```

## minecraft:difficulty_filter

限制允许该实体生成的难度范围。

- `min`：（字符串）该实体能够生成的最低难度。可为 `peaceful`、`easy`、`normal`、`hard` 中的一种。
- `max`：（字符串）该实体能够生成的最高难度。可为 `peaceful`、`easy`、`normal`、`hard` 中的一种。

示例（`spawn_rules/creeper.json`）：
```json
{
    "minecraft:difficulty_filter": {
        "min": "easy",
        "max": "hard"
    }
}
```

## minecraft:distance_filter

限制允许该实体生成的与玩家的距离。

- `min`：（数字）允许该实体生成的与玩家距离的最小值。
- `max`：（数字）允许该实体生成的与玩家距离的最大值。

示例（`spawn_rules/cod.json`）：
```json
{
    "minecraft:distance_filter": {
        "min": 12,
        "max": 32
    }
}
```

## minecraft:height_filter

限制允许该实体生成的高度。

- `min`：（数字）允许该实体生成的高度的最小值。
- `max`：（数字）允许该实体生成的高度的最大值。

示例（`spawn_rules/bat.json`）：
```json
{
    "minecraft:height_filter": {
        "min": 0,
        "max": 63
    }
}
```

## minecraft:herd

指定该实体按群生成。

- `min_size`：（数字）生成的一群实体的数量的最小值。
- `max_size`：（数字）生成的一群实体的数量的最大值。
- `event`：（字符串）生成实体所执行的事件（有关事件的详细介绍，请查看「实体」章节）。
- `event_skip_count`：（数字）在触发 `event` 前生成的实体数量。当实体数量达到这个值以后，才会触发上面 `event` 中指定的事件。

示例（`spawn_rules/fox.json`）：
```json
{
    "minecraft:herd": {
        "min_size": 2,
        "max_size": 4,
        "event": "minecraft:entity_born",
        "event_skip_count": 2
    }
}
```

## minecraft:mob_event_filter

不明。

- `event`：（字符串）不明。

示例（`spawn_rules/pillager_patrol.json`）：
```json
{
    "minecraft:mob_event_filter": {
        "event": "minecraft:pillager_patrols_event"
    }
}
```

## minecraft:permute_type

生成时使该实体成为它的变种（该条件应为一个数组，见下方示例）。

- *（对象）一个变种*
    - `weight`：（数字）该变种的权重。
    - `entity_type`：（字符串）变种的实体 ID。如不指定则表明不进行任何变化。在该 ID 后可以再追加一个尖括号（`<>`），其中放置一个事件名，会在变为该变种后触发该事件。有关事件的详细内容请查看「实体」章节。

示例一（`spawn_rules/zombie.json`）：
```json
{
    "minecraft:permute_type": [
        {
            "weight": 95
        },
        {
            "weight": 5,
            "entity_type": "minecraft:zombie_villager"
        }
    ]
}
```
这个条件指定了僵尸在生成后有 `95 / (95 + 5)` 也就是 95% 的几率保持不变，有 `5 / (95 + 5)` 也就是 5% 的几率变为 `minecraft:zombie_villager`（僵尸村民）。

示例二（`spawn_rules/pillager_patrol.json`）：
```json
{
    "minecraft:permute_type": [
        {
            "weight": 20,
            "entity_type": "minecraft:vindicator"
        },
        {
            "weight": 80,
            "entity_type": "minecraft:pillager<minecraft:spawn_as_patrol_follower>"
        }
    ]
}
```
这个条件指定了灾厄巡逻队在生成后有 20% 的几率变为卫道士（`vindicator`），有 80% 的几率变为掠夺者（`pillager`）并触发 `minecraft:spawn_as_patrol_follower` 事件。

## minecraft:player_in_village_filter

不明。

- `distance`：（数字）不明。
- `village_border_tolerance`：（数字）不明。

示例（`spawn_rules/pillager_patrol.json`）：
```json
{
    "minecraft:player_in_village_filter": {
        "distance": 48,
        "village_border_tolerance": 10
    }
}
```

## minecraft:spawn_event

当该实体生成后执行的事件。有关事件的详细介绍请看「实体」章节。可以通过这一事件来让该实体在生成后进行一些条件判断，以及进行一些变换等。

- `event`：（字符串）要执行的事件。

示例（`spawn_rules/stray.json`）：
```json
{
    "minecraft:spawn_event": {
        "event": "change_to_skeleton"
    }
}
```

## minecraft:spawns_on_block_filter

限制该生物只能生成在指定方块上（该条件应为一个表明方块 ID 的字符串，见下方示例）。

示例（`spawn_rules/chicken.json`）：
```json
{
    "minecraft:spawns_on_block_filter": "minecraft:grass"
}
```

## minecraft:spawns_on_surface

通过添加该对象，实体将可以生成在地表上。如果不添加则不能生成。

示例（`spawn_rules/bee.json`）：
```json
{
    "minecraft:spawns_on_surface": {}
}
```

## minecraft:spawns_underground

通过添加该对象，实体将可以生成在地下。如果不添加则不能生成。

示例（`spawn_rules/bat.json`）：
```json
{
    "minecraft:spawns_underground": {}
}
```

## minecraft:spawns_underwater

通过添加该对象，实体将可以生成在水里。如果不添加则不能生成。

示例（`spawn_rules/cod.json`）：
```json
{
    "minecraft:spawns_underwater": {}
}
```

## minecraft:weight

指定实体生成的权重。

- `default`：（数字）该实体生成的权重。值越**高**，生成率越**低**。

示例（`spawn_rules/bat.json`）：
```json
{
    "minecraft:weight": {
        "default": 10
    }
}
```

## minecraft:world_age_filter

限制只有当玩家在该世界的游戏时间达到一定长度以后该实体才能生成。

- `min`：（数字）玩家在该世界的游戏时间的最小值。
- `max`：（数字）玩家在该世界的游戏时间的最大值。

示例（`spawn_rules/pillager_patrol.json`）：
```json
{
    "minecraft:world_age_filter": {
        "min": 6000
    }
}
```

[page]

# 实体

「行为」包，最重头的便是可以定义实体的行为。

实体定义文件应放置在行为包根目录下的 `entities` 文件夹内。

## 简述

一个实体是十分复杂的东西。但这个复杂的实体其实可以拆成很多简单的部分，我们把这些简单的部分称为「**组件**」。写实体的定义文件就像是写在拼积木，用一个个的组件来拼凑出一个完整的实体。

下图是一个经过大量简化的牛的基础组件：

![牛-基础组件.png](https://i.loli.net/2019/10/25/l6gVSL91nxtqGEN.png)

我们知道，只有成年的牛才能繁殖、掉落物品，而只有幼体的牛才会长大、被缩小到原来的 1/2。这些组件会根据牛的状态来变化，因此不能直接添加到牛的基础组件当中：

![牛-组件组.png](https://i.loli.net/2019/10/25/VGiQP3Hrj2tMdTe.png)

我们把右侧「成年牛特有」、「幼体牛特有」这两组，称为「**组件组**」。当一头牛是成年牛时，我们会把成年牛特有的组件组添加到它的组件当中；当一头牛是幼体牛时，我们会把幼体牛特有的组件组添加到它的组件当中。

当一头牛从幼体成长为成年牛以后，我们需要从它的组件当中移除幼体牛特有的组件组，再加入成年牛特有的组件组。这一行为实际上是由**事件**完成的：在幼体牛特有组件组的「长大」组件中，会在该实体成长为成年牛的那一刻触发某个**事件**，这个事件可以移除掉幼体牛特有组件组，并添加成年牛特有组件组。

实体涉及到的两大重要部分，也就是**组件**和**事件**，已经讲解完了。

## JSON 格式

- `format_version`：（字符串）格式版本。本教程介绍的是 1.14.0 的写法，因此本教程自己的示例中都将一律写为 `1.14.0`。不过，写为旧版本的格式也仍然能在最新版游戏中使用，例如原版的药水云的定义文件的格式版本在发帖时仍为 `1.8.0`。事实上，这个参数存在的意义就是为了能够保持向后兼容。
- `minecraft:entity`：（对象）表明这个文件定义了实体。
    - `description`：（对象）对该实体的描述。
        - `identifier`：（字符串）该实体的 ID。
        - `is_spawnable`：（布尔值）可选。该实体是否拥有刷怪蛋。如果设置为 `true`，在创造模式物品栏中将会有该实体的对应刷怪蛋。默认为 `false`。
        - `is_summonable`：（布尔）可选。是否可以通过命令生成。如果设置为 `true`，将可以通过 `/summon <实体 ID>` 命令生成该实体。默认为 `false`。
        - `is_experimental`：（布尔）可选。该实体是否为实验性特性。如果设置为 `true`，只有在世界选项里勾选「启用实验玩法」，该实体才会能够使用。默认为 `false`。
    - `components`：（对象）该实体的基础组件。
    - `component_groups`：（对象）该实体具有的所有组件组。
        - *组件组的名称*：（对象）一个组件组。组件组下的格式同上面的 `components`（基础组件）一样。
    - `events`：（对象）该实体的所有事件。事件可以在指定条件下执行，执行以后可以禁用/启用在 `component_groups` 中定义的组件组。
        - `事件名`：（对象）一个事件（有关事件的具体介绍见下）。

相信看完简述已经能够理解，不过这里再重复一下。`components`（基础组件）与 `component_groups`（组件组）下面的定义的每个组件组的区别在于，前者中定义的是该实体的基础行为，而后者中则有一些与特定状态有关的行为。此外，如果组件组修改了和基础组件中相同的行为，在这个组件组被启用后，基础组件中的值会被组件组里定义的覆盖掉。

## 学习原版

我们先挑一个字符数少又骨架完整的来作示例。以钓鱼竿的鱼钩实体（`./entities/fishing_hook.json`）为例，格式略有调整。注释是我加的。

```json
{
    "format_version": "1.12.0",
    "minecraft:entity": {
        "description": {
            "identifier": "minecraft:fishing_hook", // 该实体的 ID 为 minecraft:fishing_hook
            "is_spawnable": false,
            "is_summonable": false,
            "is_experimental": false
        },
        "components": {
            // 这里面都是鱼钩实体的基础组件。
            "minecraft:collision_box": {
                // 定义了碰撞箱的大小。
                "width": 0.15,
                "height": 0.15
            },
            "minecraft:projectile": {
                // 定义了该实体抛到地上后的特性。
                "on_hit": { "stick_in_ground": {} }
            },
            "minecraft:loot": {
                // 定义了该实体死亡后的战利品表，其实就是钓鱼的战利品表。没错，你钓鱼获得的物品是通过「杀死鱼钩」来得到的。
                "table": "loot_tables/gameplay/fishing.json"
            },
            "minecraft:physics": {
                // 使该实体有物理属性，也就是能撞墙上，不会穿墙。
            },
            "minecraft:pushable": {
                // 定义了该实体能否被推动等。
                "is_pushable": false,
                "is_pushable_by_piston": true
            }
        },
        "component_groups": {
            // 这下面定义的就都是组件组了。
            "loot_jungle": {
                // 这是一个叫 loot_jungle 的组件组。
                "minecraft:loot": {
                    // 这个组件组里又定义了一遍该实体死亡后的战利品表。这意味着，如果 loot_jungle 组件组被启用，这个鱼钩的战利品表将变成这个，而不是在基础组件里定义的那个。
                    "table": "loot_tables/gameplay/jungle_fishing.json"
                }
            }
        },
        "events": {
            // 这下面定义的是各种事件。
            "minecraft:entity_spawned": {
                // 这是一个叫 minecraft:entity_spawned 的事件。这个事件是游戏内置的事件，不需要我们自己调用。游戏会在实体生成后立刻执行这个事件。
                // 简单说一下这个事件的功能。很简单，就是在当前生物群系为 jungle（丛林）时，启用上面的那个 loot_jungle 组件组。
                "sequence": [
                    {
                        "filters": {
                            "test": "is_biome",
                            "value": "jungle"
                        },
                        "add": { "component_groups": [ "loot_jungle" ] }
                    }
                ]
            }
        }
    }
}
```

似乎很简单？确实很简单。

当你阅读到这里以后，推荐你先**停止阅读**这一章节，把页面拉到上面目录，把「实体」章节的 3 个附录中的「文件结构」和「学习原版」部分都看一遍，相信你会对整个实体行为定义文件理解得更加深刻。

那么，当你的理解更进一步以后，我们再找一个原版示例吧。这回把牛的定义文件拿出来好了。在「简述」章节举的例子正好就是牛，来个收尾呼应，争取能当个满分作文（笑

这一回我可就一条注释都不写了哦！遇到不知道的，你可以翻回上面「简述」再看一遍，或者看上面的「文件格式」，或者再看看那三个附录。加油！

```
{
    "format_version": "1.13.0",
    "minecraft:entity": {
        "description": {
            "identifier": "minecraft:cow",
            "is_spawnable": true,
            "is_summonable": true,
            "is_experimental": false
        },
        "component_groups": {
            "minecraft:cow_baby": {
                "minecraft:is_baby": {},
                "minecraft:scale": {
                    "value": 0.5
                },
                "minecraft:ageable": {
                    "duration": 1200,
                    "feed_items": "wheat",
                    "grow_up": {
                        "event": "minecraft:ageable_grow_up",
                        "target": "self"
                    }
                },
                "minecraft:behavior.follow_parent": {
                    "priority": 6,
                    "speed_multiplier": 1.1
                }
            },
            "minecraft:cow_adult": {
                "minecraft:experience_reward": {
                    "on_bred": "Math.Random(1,7)",
                    "on_death": "query.last_hit_by_player ? Math.Random(1,3) : 0"
                },
                "minecraft:loot": {
                    "table": "loot_tables/entities/cow.json"
                },
                "minecraft:behavior.breed": {
                    "priority": 3,
                    "speed_multiplier": 1.0
                },
                "minecraft:breedable": {
                    "require_tame": false,
                    "breed_items": "wheat",
                    "breeds_with": {
                        "mate_type": "minecraft:cow",
                        "baby_type": "minecraft:cow",
                        "breed_event": {
                            "event": "minecraft:entity_born",
                            "target": "baby"
                        }
                    }
                },
                "minecraft:interact": {
                    "interactions": [
                        {
                            "on_interact": {
                                "filters": {
                                    "all_of": [
                                        {
                                            "test": "is_family",
                                            "subject": "other",
                                            "value": "player"
                                        },
                                        {
                                            "test": "has_equipment",
                                            "domain": "hand",
                                            "subject": "other",
                                            "value": "bucket:0"
                                        }
                                    ]
                                }
                            },
                            "use_item": true,
                            "transform_to_item": "bucket:1",
                            "play_sounds": "milk",
                            "interact_text": "action.interact.milk"
                        }
                    ]
                }
            }
        },
        "components": {
            "minecraft:type_family": {
                "family": [
                    "cow",
                    "mob"
                ]
            },
            "minecraft:breathable": {
                "total_supply": 15,
                "suffocate_time": 0
            },
            "minecraft:navigation.walk": {
                "can_path_over_water": true,
                "avoid_water": true,
                "avoid_damage_blocks": true
            },
            "minecraft:movement.basic": {},
            "minecraft:jump.static": {},
            "minecraft:can_climb": {},
            "minecraft:collision_box": {
                "width": 0.9,
                "height": 1.3
            },
            "minecraft:nameable": {},
            "minecraft:health": {
                "value": 10,
                "max": 10
            },
            "minecraft:hurt_on_condition": {
                "damage_conditions": [
                    {
                        "filters": { "test": "in_lava", "subject": "self", "operator": "==", "value": true },
                        "cause": "lava",
                        "damage_per_tick": 4
                    }
                ]
            },
            "minecraft:movement": {
                "value": 0.25
            },
            "minecraft:behavior.float": {
                "priority": 0
            },
            "minecraft:behavior.panic": {
                "priority": 1,
                "speed_multiplier": 1.25
            },
            "minecraft:behavior.mount_pathing": {
                "priority": 2,
                "speed_multiplier": 1.5,
                "target_dist": 0.0,
                "track_target": true
            },
            "minecraft:behavior.breed": {
                "priority": 3,
                "speed_multiplier": 1.0
            },
            "minecraft:behavior.tempt": {
                "priority": 4,
                "speed_multiplier": 1.25,
                "items": [
                    "wheat"
                ]
            },
            "minecraft:behavior.follow_parent": {
                "priority": 5,
                "speed_multiplier": 1.1
            },
            "minecraft:behavior.random_stroll": {
                "priority": 6,
                "speed_multiplier": 0.8
            },
            "minecraft:behavior.look_at_player": {
                "priority": 7,
                "look_distance": 6.0,
                "probability": 0.02
            },
            "minecraft:behavior.random_look_around": {
                "priority": 9
            },
            "minecraft:leashable": {
                "soft_distance": 4.0,
                "hard_distance": 6.0,
                "max_distance": 10.0
            },
            "minecraft:balloonable": {},
            "minecraft:rideable": {
                "seat_count": 1,
                "family_types": [
                    "zombie"
                ],
                "seats": {
                    "position": [0.0, 1.105, 0.0]
                }
            },
            "minecraft:physics": {},
            "minecraft:pushable": {
                "is_pushable": true,
                "is_pushable_by_piston": true
            }
        },
        "events": {
            "minecraft:entity_spawned": {
                "randomize": [
                    {
                        "weight": 95,
                        "add": {
                            "component_groups": [
                                "minecraft:cow_adult"
                            ]
                        }
                    },
                    {
                        "weight": 5,
                        "add": {
                            "component_groups": [
                                "minecraft:cow_baby"
                            ]
                        }
                    }
                ]
            },
            "minecraft:entity_born": {
                "add": {
                    "component_groups": [
                        "minecraft:cow_baby"
                    ]
                }
            },
            "minecraft:entity_transformed": {
                "remove": {},
                "add": {
                    "component_groups": [
                        "minecraft:cow_adult"
                    ]
                }
            },
            "minecraft:ageable_grow_up": {
                "remove": {
                    "component_groups": [
                        "minecraft:cow_baby"
                    ]
                },
                "add": {
                    "component_groups": [
                        "minecraft:cow_adult"
                    ]
                }
            }
        }
    }
}
```

我相信强大的你已经完全理解了牛这个实体的行为，以及这些复杂行为的定义方式了。你真是太棒了，请给我的帖子最高等级的评分！

## 自造轮子

本教程再举一个例子来让大家进一步熟悉如何编辑实体的行为 —— 让牛入水爆炸！（牛：喵喵喵？）

由于要修改的是原版就有的文件，所以在你的行为包下创建 `entities/cow.json` 文件，把原版牛的内容复制过去即可。游戏会用你的行为包里面定义的牛的行为文件覆盖掉原版自带的牛的行为了。

分析一下我们的需求「让牛入水爆炸」：

1. 「爆炸」这一行为怎么做？查询附录可知，爆炸组件的名称叫做 `minecraft:explode`；
2. 爆炸是在一定条件下才有的，因此爆炸组件应该放在一个组件组里面，不能直接加到基础组件；
3. 用了组件组，就一定要在某条件下触发一个事件，让该事件启用这个组件组，才能让它发挥作用。条件很简单，是「入水」，所以我们添加一个在牛入水后触发指定事件的基础组件即可。

为了添加这一行为，需要在基础组件里添加一个组件，并且再添加一个组件组、一个事件。

我们先添加组件组，命名为 `spgoding:exploding`，在其中加入爆炸的组件。当该组件组被启用后，牛就会开始自爆了：

```
"spgoding:exploding": {
    "minecraft:explode": {
        "fuseLength": -1,
        "fuseLit": true,
        "power": 3,
        "causesFire": false,
        "destroyAffectedByGriefing": true
    }
}
```

然后添加事件，使其启用上面定义的 `spgoding:exploding` 组件组。事件名就叫 `spgoding:start_explode` 好了：

```
"spgoding:start_explode": {
    "add": {
        "component_groups": [
            "spgoding:exploding"
        ]
    }
}
```

最后在基础组件中添加一个组件，当满足指定滤器（入水）时触发我们的 `spgoding:start_explode` 事件：

```
"minecraft:environment_sensor": {
    "on_environment": [
        {
            "filters": { "test": "in_water" },
            "event": "spgoding:start_explode"
        }
    ]
}
```

至此，我们已经成功修改了牛的行为。

退出再重进世界。

[page]

# 附录：滤器

滤器能够用于测试指定的项目是否满足你所定义的条件。在组件、事件等各种地方都有运用到滤器。

## JSON 格式的基本单位

- `test`：（字符串）要测试的项目（详细介绍见下）。
- `subject`：（字符串）测试的主体。只有部分测试需要指定这个字段（详细介绍见下）。
- `domain`：（字符串）域。只有部分测试需要指定这个字段（详细介绍见下）。
- `value` （字符串或数字或布尔）给定的值。将会拿实际的数据与这个值进行比较。
- `operator`：（字符串）比较方式。在指定项目（`test`）上，主体（`subject`）的域（`domain`）与给定的值（`value`）的比较方式（详细介绍见下）。

> *这都啥玩意儿，我怎么看不懂？*
> 
> 没事儿，先装作看懂了往下再看点儿吧。

## 测试主体

- `self`：调用此测试的实体（即使用了该滤器的实体）。
- `other`：除调用者以外的，参与交互的实体。该实体具体是哪个，由调用该滤器的组件或事件决定。可能没有这个实体。
- `parent`：调用者的父母。可能没有这个实体。
- `player`：参与交互的玩家。该玩家具体是哪个，由调用该滤器的组件或事件决定。可能没有这个实体。
- `target`：调用者的目标实体。通常是该实体正在追杀的目标。可能没有这个实体。

## 比较方式

- `=`、`==`、`equals`：等于。当主体的项目与给定数据完全一致时，测试通过。如果比较方式被省略，则默认使用该种方式比较。
- `!=`、`<>`、`not`：不等于。当主体的项目与给定数据不一致时，测试通过。
- `<`：小于。当主体的项目小于给定数据时，测试通过。
- `<=`：小于等于。当主体的项目小于或等于给定数据时，测试通过。
- `>`：大于。当主体的项目大于给定数据时，测试通过。
- `>=`：大于等于。当主体的项目大于或等于给定数据时，测试通过。

## 组合

滤器其实可以将基本单位通过逻辑进行任意组合。

- `any_of`：（数组）当其下只要有任何一个滤器通过时，这组滤器就能通过。
- `all_of`：（数组）当其下的所有滤器通过时，这组滤器才能通过。
- `none_of`：（对象或数组）当其下的（所有）滤器没有通过时，该滤器才能通过。

例如我们可以这么写：

```json
{
    "any_of": [
        { "test": "has_biome_tag", "operator": "==", "value": "taiga"}, 
        { "test": "has_biome_tag", "operator": "!=", "value": "mega" }
    ]
}
```

当生态群系有 `taiga` 标签**或者**没有 `mega` 标签时，条件达成。

如果我们把 `any_of` 改成 `all_of`：

```json
{
    "all_of": [
        { "test": "has_biome_tag", "operator": "==", "value": "taiga"}, 
        { "test": "has_biome_tag", "operator": "!=", "value": "mega" }
    ]
}
```

就变成了当生态群系有 `taiga` 标签**并且**没有 `mega` 标签时，条件才能达成。

如果我们再把 `all_of` 改成 `none_of`：

```json
{
    "none_of": [
        { "test": "has_biome_tag", "operator": "==", "value": "taiga"}, 
        { "test": "has_biome_tag", "operator": "!=", "value": "mega" }
    ]
}
```

就变成了当生态群系有 `taiga` 标签**或者**没有 `mega` 标签时，条件**都不能**达成。

`all_of`、`any_of` 以及 `none_of` 可以自由组合和嵌套。示例（`entities/pufferfish.json`）：

```json
{
    "none_of": [
        { /* A */ "test": "is_family", "subject": "other", "value": "mob" },
        {
            "all_of": [
                { /* B */ "test": "is_family", "subject": "other", "value": "player" },
                { 
                    "none_of": { /* C */ "test": "has_ability", "subject": "other", "value": "instabuild" } 
                }
            ]
        }
    ]
}
```

只要 A 滤器通过，**或者** B 滤器通过*且* C 滤器不通过，这整个一大块就**不能**通过。

> *我怎么还是看不懂？？？*
> 
> 你就快要看懂了。不就是示例嘛，我有一吨的示例给你。

## 测试项目

下面将列出所有能够填写到 `test` 中的测试项目。将会在每个测试项目介绍的首段前用括号表明该测试项目的类型，也就是你应该写在 `value` 里的值的类型。

### clock_time

（数字）当前时间。

将一天的时间均分到了 `[0.00, 1.00]` 中。即：

- `0.00`：正午。
- `0.25`：日落。
- `0.50`：午夜。
- `0.75`：次日日出。
- `1.00`：次日正午。

示例：
```json
{ "test": "clock_time", "value": 0.001 }
```
在当前时间为正午后过了一小点儿时，该滤器通过。

### distance_to_nearest_player

（数字）主体到最近的玩家的距离。

示例（`entities/fox.json`）：
```json
{ "test": "distance_to_nearest_player", "operator": ">", "value": 16 }
```
在距离最近玩家的距离大于 `16` 时，该滤器通过。

### has_ability

（字符串）主体是否具有某个能力。可选值：

- `flySpeed`：飞行速度。
- `walkSpeed`：行走速度。
- `flying`：是否正在飞行。
- `mayfly`：是否能够飞行。
- `instabuild`：是否能够瞬间摧毁方块。经常被用来判断玩家是否为创造模式。
- `invulnerable`：是否无敌。
- `mute`：是否被禁言（教育版特性）。
- `worldbuilder`：是否为地图建造者（教育版特性）。
- `noclip`：（教育版特性）。
- `lightning`：（教育版特性）。

示例（`entities/pufferfish.json`）：
```json
{ "none_of": { "test": "has_ability", "subject": "other", "value": "instabuild" } }
```
在 `other` 主体不处于创造模式时，该滤器通过。`other` 主体具体是哪个，由调用该滤器的组件或事件决定。

### has_biome_tag

（字符串）当前生物群系是否具有某个标签。有关生物群系标签的内容你可以在「生物群系」章节查看。

示例（`entities/villager_v2.json`）：
```json
{ "test": "has_biome_tag", "value": "desert" }
```
在当前生物群系具有 `desert` 标签时，该滤器通过。

### has_component

（字符串）主体是否拥有某个组件。

示例（`entities/creeper.json`）：
```json
{ "test": "has_component", "operator": "!=", "value": "minecraft:explode" }
```
在当前实体没有 `minecraft:explode` 组件时，该滤器通过。

### has_damage

（字符串）主体是否受到了某种类型的伤害。可选值：

- `anvil`：掉落的铁砧砸伤。
- `attack`：攻击。
- `block_explosion`：方块爆炸。
- `contact`：不明。
- `drowning`：溺水。
- `entity_explosion`：实体爆炸。
- `fall`：摔伤。
- `falling_block`：掉落的方块砸伤。
- `fatal`：任何能直接杀死该实体的致命伤害。
- `fire`：燃烧伤害。
- `fire_tick`：不明。
- `fly_into_wall`：鞘翅飞行撞墙。
- `lava`：熔岩烧伤。
- `magic`：魔法（药水）。
- `none`：无。
- `override`：不明。
- `piston`：活塞。
- `projectile`：弹射物。
- `starve`：饥饿。
- `suffocation`：窒息。
- `suicide`：自杀。
- `thorns`：荆棘。
- `void`：虚空。
- `wither`：凋零。

示例（`entities/pillager.json`）：
```json
{ "test": "has_damage", "value": "fatal" }
```
在当前实体受到致命打击时，该滤器通过。

### has_equipment

（字符串）主体是否拥有某个物品。

该检测项目需要用域（`domain`）来指定检测的栏位，可选值：
- `any`：任意栏位。
- `hand`：手部。
- `armor`：盔甲。
- `head`：头部。
- `torso`：躯干。
- `feet`：脚部。
- `leg`：腿部。

示例（`entities/cow.json`）：
```json
{
    "test": "has_equipment",
    "domain": "hand",
    "subject": "other",
    "value": "bucket:0"
}
```
在 `other` 主体的手中含有空桶时，该滤器通过。

### has_mob_effect

（字符串）主体是否拥有某个状态效果。

示例（`entities/player.json`）：
```json
{ "test": "has_mob_effect", "subject": "self", "value": "bad_omen" }
```
在当前实体具有 `bad_omen`（不祥之兆）状态效果时，该滤器通过。

### has_target

（布尔）主体是否拥有目标实体。

示例（`entities/fox.json`）：
```json
{ "test": "has_target", "operator": "==", "value": false }
```
在当前实体没有目标实体时，该滤器通过。

### has_trade_supply

（布尔）主体是否能提供交易。似乎只能被流浪商人使用。

示例（`entities/wandering_trader.json`）：
```json
{ "test": "has_trade_supply", "subject": "self", "value": false }
```
在主体没有交易要提供时，该滤器通过。

### hourly_clock_time

（数字）24 小时制的时间。取值范围 `[0, 24000]`。

示例（`entities/wandering_trader.json`）：
```json
{ "test": "hourly_clock_time", "operator": ">=", "value": 18000 }
```
在当前时间晚于 18 点时，该滤器通过。

### in_caravan

（布尔）主体是否在车队中。似乎只对行商骆驼有用。

示例（`entities/llama.json`）：
```json
{ "test": "in_caravan", "value": false }
```
在当前实体不在车队中时，该滤器通过。

### in_clouds

（布尔）主体是否在云中。

示例：
```json
{ "test": "in_clouds" }
```
在当前实体是云实体时，该滤器通过。

### in_lava

（布尔）主体是否在岩浆中。

示例（`entities/armor_stand.json`）：
```json
{ "test": "in_lava", "subject": "self", "operator": "==", "value": true }
```
在当前实体位于熔岩中时，该滤器通过。

### in_water

（布尔）主体是否在水中。

示例（`entities/drowned.json`）：
```json
{ "test": "in_water", "subject": "other", "value": true }
```
在 `other` 实体位于水中时，该滤器通过。

### in_water_or_rain

（布尔）是否在水中或雨中。

示例（`entities/blaze.json`）：
```json
{ "test": "in_water_or_rain", "operator": "==", "value": true }
```
在当前实体位于水中或雨中时，该滤器通过。

### is_altitude

（数字）海拔高度。0 为基岩所在的高度。

示例：
```json
{ "test": "is_altitude", "operator": "<=", "value": 1 }
```
在当前实体位于基岩层时，该滤器通过。

### is_avoiding_mobs

（布尔）正在躲避怪物。似乎只对流浪商人有用。

示例（`entities/wandering_trader.json`）：
```json
{ "test": "is_avoiding_mobs", "subject": "self", "value": true }
```
在当前实体正在躲避怪物时，该滤器通过。

### is_biome

（字符串）生态群系。

示例（`entities/cave_spider.json`）：
```json
{ "test": "is_biome", "value" : "the_nether" }
```
在当前生物群系为 `the_nether`（下界）时，该滤器通过。

### is_block

（字符串）主体是否为某个方块。

该检测项目需要使用特殊的主体（`subject`）：`block`。

示例（`entities/bee.json`）：
```json
{ "test": "is_block", "subject": "block", "value": "minecraft:bee_nest" }
```
在指定方块是蜂巢时，该滤器通过。

### is_brightness

（数字）亮度。范围在 `[0.0, 1.0]` 之中。

示例（`entities/cave_spider.json`）：
```json
{ "test" : "is_brightness", "operator" : "<", "value" : 0.49 }
```
在当前亮度相对于满亮度的比例小于 `0.49` 时，该滤器通过。

### is_climbing

（布尔）是否正在攀爬（梯子等）。

示例：
```json
{ "test": "is_climbing" }
```
在当前实体正在攀爬时，该滤器通过。

### is_color

（字符串）检测主体的颜色。似乎只能在羊上使用。

- `black`：黑色。
- `blue`：蓝色。
- `brown`：棕色。
- `cyan`：青色。
- `gray`：灰色。
- `green`：绿色。
- `light_blue`：淡蓝色。
- `light_green`：淡灰色。
- `magenta`：品红色。
- `orange`：橘色。
- `pink`：粉色。
- `purple`：紫色。
- `red`：红色。
- `silver`：银色。
- `white`：白色。
- `yellow`：黄色。

示例（`entities/evocation_illager.json`）：
```json
{ "test": "is_color", "subject": "other", "value":  "blue" }
```
在 `other` 实体的颜色为 `blue`（蓝色）时，该滤器通过。

### is_daytime

（布尔）是否正在白天。

示例（`entities/bee.json`）：
```json
{ "test" : "is_daytime", "value" : false }
```
在当前时间不是白天时，该滤器通过。

### is_difficulty

（布尔）游戏难度。可选值：

- `peaceful`：和平。
- `easy`：简单。
- `normal`：普通。
- `hard`：困难。

示例（`entities/arrow.json`）：
```json
{"test": "is_difficulty", "value": "hard"}
```
在当前难度为困难模式时，该滤器通过。

### is_family

（布尔）主体是否是某个实体种类。

示例（`entities/arrow.json`）：
```json
{"test": "is_family", "subject": "other", "value": "player"}
```
在 `other` 实体属于 `player` 类（玩家）时，该滤器通过。

### is_game_rule

游戏规则。

该检测项目需要定义域，来指明检测哪个游戏规则。

示例（`entities/ender_pearl.json`）：
```json
{"test": "is_game_rule", "domain": "domobspawning", "value": false}
```
在 `domobspawning` 游戏规则为 `false`（禁止生物生成）时，该滤器通过。

### is_humid

（布尔）区域是否潮湿。

示例：
```json
{ "test": "is_humid" }
```
在当前区域潮湿时，该滤器通过。

### is_immobile

（布尔）主体是否不能移动。当失去行为组件、刚刚改变维度或没有生命值时，实体会不能移动。

示例：
```json
{ "test": "is_immobile" }
```
在当前实体不能移动时，该滤器通过。

### is_in_village

（布尔）主体是否位于村庄

示例（`entities/player.json`）：
```json
{ "test": "is_in_village", "subject": "self", "value": true }
```
在当前实体位于村庄时，该滤器通过。

### is_leashed

（布尔）主体是否被拴绳拴着。

示例（`entities/llama.json`）：
```json
{ "test": "is_leashed", "subject": "self", "value": false }
```
在当前实体没有被拴绳牵着时，该滤器通过。

### is_leashed_to

（布尔）当前实体是否被拴绳牵到主体上。

示例（`entities/wandering_trader.json`）：
```json
{ "test": "is_leashed_to", "subject": "other", "value": true }
```
在当前实体被拴绳牵到 `other` 实体上时，该滤器通过。

### is_mark_variant

（数字）主体的类型是否是指定变种。

示例（`entities/llama.json`）：
```json
{ "test": "is_mark_variant", "subject": "self", "operator": "!=", "value": 1 }
```
在当前实体的变种不是 `1` 时，该滤器通过。

### is_moving

（布尔）主体是否正在移动。

示例：
```json
{ "test": "is_moving" }
```
在当前实体正在移动时，该滤器通过。

### is_owner

（布尔）主体是否是当前实体的主人。

示例：
```json
{ "test": "is_owner", "subject": "other" }
```
在 `other` 实体是当前实体的主人时，该滤器通过。

### is_riding

（布尔）主体是否正在骑乘。

示例（`entities/parrot.json`）：
```json
{ "test": "is_riding", "operator": "!=", "value": true }
```
在当前实体没有在骑乘其他实体时，该滤器通过。

### is_skin_id

（数字）主体的皮肤 ID。似乎只对新版村民和新版僵尸村民有用。

示例（`entities/villager_2.json`）：
```json
{ "test": "is_skin_id", "subject": "other", "value": 0 }
```
在当前实体的皮肤 ID 为 `0` 时，该滤器通过。

### is_sleeping

（布尔）主体是否正在睡觉。

示例（`entities/fox.json`）：
```json
{ "test": "is_sleeping", "value": true }
```
在当前实体正在睡觉时，该滤器通过。

### is_sneaking

（布尔）主体是否正在潜行。

示例（`entities/fox.json`）：
```json
{ "test": "is_sneaking", "subject": "other", "operator": "!=", "value": true }
```
在 `other` 实体没有潜行时，该滤器通过。

### is_snow_covered

（布尔）主体所在区域是否被雪覆盖。

示例（`entities/fox.json`）：
```json
{ "test": "is_snow_covered", "value": true }
```
在当前实体所在区域被雪覆盖了时，该滤器通过。

### is_target

（布尔）主体是否是当前实体的目标。

示例：
```json
{ "test": "is_target" }
```
在主体是当前实体的目标实体时，该滤器通过。

### is_temperature_type

（布尔）当前区域的温度类型。

- `cold`：冷。
- `mild`：温和。
- `ocean`：洋。
- `warm`：暖。

示例：
```json
{ "test": "is_temperature_type", "value": "cold" }
```
在当前区域的温度类型为冷时，该滤器通过。

### is_temperature_value

（布尔）温度值。最冷为 0.0，最热为 1.0。

示例（`entities/snow_golem.json`）：
```json
{ "test": "is_temperature_value", "operator": "<", "value": 0.81 }
```
在当前温度小于 `0.81` 时，该滤器通过。

### is_underground

（布尔）主体是否在地下。当实体头上有非固体方块时，被认为是在地下。

示例（`entities/cave_spider.json`）：
```json
{ "test" : "is_underground", "value" : false }
```
在当前实体不在地下时，该滤器通过。

### is_underwater

（布尔）主体是否在水下。当实体完全泡在水中时，被认为是在水下。

示例（`entities/stray.json`）：
```json
{ "test": "is_underwater", "subject": "self", "operator": "==", "value": true }
```
在当前实体在水下时，该滤器通过。

### is_variant

（布尔）主体的变种 ID。

示例（`entities/mooshroom.json`）：
```json
{ "test": "is_variant", "subject": "self", "operator": "==", "value": 1}
```
在当前实体的变种 ID 为 `1` 时，该滤器通过。

### is_visible

（布尔）主体是否没有隐形状态效果。

示例（`entities/wandering_trader.json`）：
```json
{ "test": "is_visible", "subject": "self", "value": true }
```
在当前实体没有隐身时，该滤器通过。

### is_weather

（字符串）当前天气。可选值：
- `clear`：晴天。
- `rain`：雨天。
- `thunderstorm`：雷雨天。

示例（`entities/fox.json`）：
```json
{ "test": "is_weather", "operator": "!=", "value": "thunderstorm" }
```
在当前天气不是雷雨天时，该滤器通过。

### moon_intensity

（布尔）月亮强度。取值应在 `[0.00, 1.00]` 之中。

示例：
```json
{ "test": "moon_intensity", "value": 0.0 }
```
在当前月亮强度为 `0.0` 时，该滤器通过。

### moon_phase

（布尔）月相。取值应在 `[0, 7]` 之中。

示例：
```json
{ "test": "moon_phase", "value": 0 }
```
在当前月相为 `0` 时，该滤器通过。

### on_ground

（布尔）主体是否在地面。

示例（`entities/dolphin.json`）：
```json
{ "test": "on_ground", "operator": "==", "value": true },
```
在当前实体位于地面时，该滤器通过。

### on_ladder

（布尔）主体是否在梯子上。

示例：
```json
{ "test": "on_ladder" }
```
在当前实体位于梯子上时，该滤器通过。

### rider_count

（数字）骑乘主体的实体的数量。

示例（`entities/chicken.json`）：
```json
{ "test": "rider_count", "subject": "self", "operator": "==", "value": 0 }
```
在当前实体没有被骑乘时，该滤器通过。

### trusts

（布尔）当前实体是否信任主体。似乎只对狐狸有用。

示例（`entities/fox.json`）：
```json
{ "test": "trusts", "subject": "other", "operator": "!=", "value": true }
```
在当前实体信任 `other` 实体时，该滤器通过。

[page]

# 附录：组件

组件在 Wiki 的文档中被分为四类，一类是描述实体所固有**属性**（如碰撞箱等），一类描述实体的**行为**（即 **AI 目标**，例如能够跟随玩家、破坏方块等），一类是在指定情况下会被执行的**触发器**（能够用于触发事件，进而启用或禁用组件组），还有一类**其他**。本附录下的四节将分别介绍这四类下面都有哪些组件可以使用。在 `components` 以及每一个组件组下面，我们都可以直接使用下面提到的所有组件。用法为：

```json
{
    "组件名": {
        "参数 1": 值, // 有些组件可以指定参数，但是你也可以不指定，依你的需求而定。
        "参数 2": 值,
        ...
    },
    "又一个组件名": {} // 如果某个组件没有参数，或者你不想指定任何参数，直接闭合大括号即可。
}
```

[page]

# 触发器

首先讲解触发器，是因为在后面有些组件的参数中，使用到了与触发器完全相同的 JSON 格式。

触发器，是一种能够根据指定滤器来执行事件的东西（有关滤器、事件的详细内容，可以通过上方目录切换到相关附录查看）。它的工作流程如下所示：

![触发器.png](https://i.loli.net/2019/10/25/GVYvFQqrd2ih3I4.png)

## JSON 格式

- `event`：（字符串）该触发器被触发后要执行的事件的名称。
- `filters`：（对象）可选。一个滤器。如果指定了，则只有在该滤器通过的情况下才会执行 `event` 中指定的事件。
- `target`：（字符串）可选。执行该事件的目标。默认为 `self`，代表该实体自身。可选值推测与滤器中的 `subject` 一致，即：
    - `self`：调用此测试的实体（即使用了该触发器的实体）。
    - `other`：除调用者以外的，参与交互的实体。该实体具体是哪个，由该触发器的种类决定。可能没有这个实体。
    - `parent`：调用者的父母。可能没有这个实体。
    - `player`：参与交互的玩家。该玩家具体是哪个，由该触发器的种类决定。可能没有这个实体。
    - `target`：调用者的目标实体。通常是该实体正在追杀的目标。可能没有这个实体。

再次强调，触发器属于组件的一种，所以要定义在 `components` 或是 `component_groups` 中的一个组件组之下。

## minecraft:on_death

触发时机：该实体死亡时。**只能被末影龙使用**。

示例（末影龙 `entities/ender_dragon.json`）：
```json
{
    "minecraft:on_death": {
        "event": "minecraft:start_death",
        "target": "self"
    }
}
```
在该实体死亡时，会触发该实体的 `minecraft:start_death`（开始死亡）事件。

## minecraft:on_friendly_anger

触发时机：该实体附近有其他相同种类的实体愤怒时。

示例（羊驼 `entities/llama.json`）：
```json
{
    "minecraft:on_friendly_anger": {
        "event": "minecraft:defend_wandering_trader",
        "target": "self"
    }
}
```
在该实体附近有其他相同种类的实体（此例中为 `llama` 的变种 `1`，即行商骆驼）愤怒时，会触发该实体的 `minecraft:defend_wandering_trader`（保护流浪商人）事件。

## minecraft:on_hurt

触发时机：该实体受到伤害时。

示例（末影水晶 `entities/ender_crystal.json`）：
```json
{
    "minecraft:on_hurt": {
        "event": "minecraft:crystal_explode",
        "target": "self"
    }
}
```
在该实体受到伤害时，会触发该实体的 `minecraft:crystal_explode`（末影水晶爆炸）事件。

## minecraft:on_hurt_by_player

触发时机：该实体被玩家攻击时。

示例（掠夺者 `entities/pillager.json`）：
```json
{
    "minecraft:on_hurt_by_player": {
        "event": "minecraft:synchronized_ranged_mode",
        "target": "self"
    }
}
```
在该实体受到玩家攻击时，会触发该实体的 `minecraft:synchronized_ranged_mode`（和其他掠夺者同步攻击范围）事件。

## minecraft:on_ignite

触发时机：该实体着火时。

无原版示例。

## minecraft:on_start_landing

触发时机：该实体开始着陆时。**只能被末影龙使用**。

示例（末影龙 `entities/ender_dragon.json`）：
```json
{
    "minecraft:on_start_landing": {
        "event": "minecraft:start_land",
        "target": "self"
    }
}
```
在该实体开始着陆时，会触发该实体的 `minecraft:start_land`（着陆）事件。

## minecraft:on_start_takeoff

触发时机：该实体开始起飞时。**只能被末影龙使用**。

示例（末影龙 `entities/ender_dragon.json`）：
```json
{
    "minecraft:on_start_takeoff": {
        "event": "minecraft:start_fly",
        "target": "self"
    }
}
```
在该实体开始起飞时，会触发该实体的 `minecraft:start_fly`（起飞）事件。

## minecraft:on_target_acquired

触发时机：该实体找到攻击目标时。通常是该生物在视距内看到了敌对生物，或该生物受到了其他生物的攻击等。

示例（蜜蜂 `entities/bee.json`）：
```json
{
    "minecraft:on_target_acquired": {
        "event": "attacked",
        "target": "self"
    }
}
```
在该实体找到攻击目标时，会触发该实体的 `attacked`（受到攻击）事件。

## minecraft:on_target_escape

触发时机：该实体失去攻击目标时（目标已死亡，或逃出了该实体的视距等）。

示例（苦力怕 `entities/creeper.json`）：
```json
{
    "minecraft:on_target_escape": {
        "event": "minecraft:stop_exploding",
        "target": "self"
    }
}
```
在该实体失去目标时，会触发该实体的 `minecraft:stop_exploding`（停止爆炸）事件。

## minecraft:on_wake_with_owner

触发时机：该实体的主人在与该实体一起睡觉后醒来时。似乎只对宠物（猫）有用。

示例（猫 `entities/cat.json`）：
```json
{
    "minecraft:on_wake_with_owner": {
        "event": "minecraft:pet_slept_with_owner",
        "target": "self"
    }
}
```
在该实体和主人一起醒来时，会触发该实体的 `minecraft:pet_slept_with_owner`（和主人一起睡过觉）事件（该事件会启用给主人礼物的组件组 `minecraft:cat_gift_for_owner`）。

[page]

# 附录：事件

事件可以用于禁用/启用组件组。

玩家可以随意定义事件的名称（只要是由小写英文字母、下划线、半角冒号等组成的即可），并在触发器或部分组件的参数中填写事件名，以在特定条件下触发该事件。

事件按照 JSON 格式以及运作方式可以分为三种：普通事件、随机型事件、序列型事件。

## JSON 格式

- `add`：（对象）启用组件组。
    - `component_groups`：（数组）
        - *（字符串）一个组件组的名称*
- `remove`：（数组）禁用组件组。
    - `component_groups`：（数组）
        - *（字符串）一个组件组的名称*
- `randomize`：（数组）若干套操作。将会从你定义的多套操作中随机执行一套操作。
    - *（对象）一套操作*
        - `weight`：（整数）执行这套操作的权重。
        - *剩下的格式和事件 JSON 格式一样*
- `sequence`：（数组）若干套操作。这些操作将会被按顺序依次执行。
    - *（对象）一套操作*
        - `filters`：（对象，格式同滤器）一个滤器。只有该滤器测试通过以后，下方的 `add` 与 `remove` 才会被执行。
        - *剩下的格式和事件 JSON 格式一样*

## 学习原版

### add 与 remove 示例

这是牛（`entities/cow.json`）的一个事件：

```json
{
    "minecraft:ageable_grow_up": {
        "remove": {
            "component_groups": [
                "minecraft:cow_baby"
            ]
        },
        "add": {
            "component_groups": [
                "minecraft:cow_adult"
            ]
        }
    }
}
```

该事件被触发以后，将会禁用掉 `minecraft:cow_baby` 组件组，并启用 `minecraft:cow_adult` 组件组。

### randomize 示例

游戏会从 `randomize` 下定义的多套操作中随机执行一套操作。

这是牛（`entities/cow.json`）的一个事件：

```json
{
    "minecraft:entity_spawned": {
        "randomize": [
            {
                "weight": 95,
                "add": {
                    "component_groups": [
                        "minecraft:cow_adult"
                    ]
                }
            },
            {
                "weight": 5,
                "add": {
                    "component_groups": [
                        "minecraft:cow_baby"
                    ]
                }
            }
        ]
    }
}
```

该事件被触发以后，会有 `95 / (95 + 5)` 也就是 95% 的几率启用 `minecraft:cow_adult` 组件组，有 `5 / (95 + 5)` 也就是 5% 的几率启用 `minecraft:cow_baby` 组件组。

### sequence 示例

游戏会顺序执行 `sequence` 下定义的每一套操作。

这是箭（`entities/arrow.json`）的一个事件：

```json
{
    "minecraft:entity_spawned": {
        "sequence": [
            {
                "filters": { "test": "is_difficulty", "value": "hard" },
                "add": {
                    "component_groups": [
                        "minecraft:hard_arrow"
                    ]
                }
            },
            {
                "filters": { "test": "is_family", "subject": "other", "value": "player" },
                "add": {
                    "component_groups" : [ 
                        "minecraft:player_arrow"
                    ]
                }
            },
            {
                "filters": { "test": "is_family", "subject": "other", "value": "pillager" },
                "add": {
                    "component_groups": [
                        "minecraft:pillager_arrow"
                    ]
                }
            }
        ]
    }
}
```

该事件被触发以后，将会顺序执行里面定义的这三套操作。第一套将在难度为困难时执行，会添加 `minecraft:hard_arrow` 组件组；第二套会在射出箭的实体为玩家时执行，添加组件组 `minecraft:player_arrow`；第三套会在射出箭的实体为掠夺者时执行，添加组件组 `minecraft:pillager_arrow`

### 嵌套示例

事实上，`add`、`remove`、`randomize` 和 `sequence` 能够各种嵌套，实现一些复杂的操作。

这是这个世界上最复杂的实体（没有之一）—— 新版村民（`entities/villager_v2.json`）的**一个**事件：

```json
{
    "minecraft:entity_transformed": {
        "sequence": [
            // ... 此处省略 2 套操作
            {
                // 从僵尸村民转换为具有正确皮肤的村民
                "filters": { "test": "is_family", "subject": "other", "operator": "==", "value": "zombie_villager" }, 
                "sequence" : [
                    {
                        "filters": { "test": "is_skin_id", "subject": "other", "value": 0 },
                        "add": { "component_groups": [ "villager_skin_0" ] }
                    },
                    {
                        "filters": { "test": "is_skin_id", "subject": "other", "value": 1 },
                        "add": { "component_groups": [ "villager_skin_1" ] }
                    },
                    // ... 此处省略 13 套操作
                ]
            },
            {
                // 从旧版村民转换为具有正确皮肤的新版村民
                "filters": { "test": "is_family", "subject": "other", "operator": "==", "value": "villager" }, 
                "sequence" : [
                    {
                        "randomize": [
                            {
                                "weight": 1,
                                "add": { "component_groups": [ "villager_skin_0" ] }
                            },
                            {
                                "weight": 1,
                                "add": { "component_groups": [ "villager_skin_1" ] }
                            },
                            // ... 此处省略 4 套操作
                        ]
                    },
                    // ... 此处省略 6 套操作
                ]
            }
        ]
    }
}
```

该事件被触发以后……饶了我吧……

## 附录的附录：内置事件名

Minecraft 为我们提供了几个内置的事件名，这些名称的事件不需要我们手动调用，它们会在某些条件下自动触发。

### minecraft:entity_spawned

当实体生成后立刻执行。

示例（`entities/arrow.json`）：
```json
{
    "minecraft:entity_spawned": {
        "sequence": [
            {
                "filters": { "test": "is_difficulty", "value": "hard" },
                "add": {
                    "component_groups": [
                        "minecraft:hard_arrow"
                    ]
                }
            },
            {
                "filters": { "test": "is_family", "subject": "other", "value": "player" },
                "add": {
                    "component_groups" : [ 
                        "minecraft:player_arrow"
                    ]
                }
            },
            {
                "filters": { "test": "is_family", "subject": "other", "value": "pillager" },
                "add": {
                    "component_groups": [
                        "minecraft:pillager_arrow"
                    ]
                }
            }
        ]
    }
}
```

### minecraft:entity_born

当实体由于繁殖而出生时执行。

示例（`entities/bee.json`）：
```json
{
    "minecraft:entity_born": {
        "add": {
            "component_groups": [
                "bee_baby",
                "shelter_detection",
                "look_for_food"
            ]
        }
    }
}
```

### minecraft:entity_transformed

当该实体发生转变时执行。

示例（`entities/cow.json`）：
```json
{
    "minecraft:entity_transformed": {
        "remove": {},
        "add": {
            "component_groups": [
                "minecraft:cow_adult"
            ]
        }
    }
}
```

[page]

# 物品

行为包可以设置物品的行为。

所有物品的定义文件应该放置在根目录下的 `items` 文件夹内。

## 简述

没有，当你理解了实体以后理解物品没有什么障碍。物品的行为同样是由组件构成的。

## JSON 格式

- `format_version`：（字符串）文件格式版本。本帖一律写为 `1.14.0`。
- `minecraft:item`：（对象）表明该文件定义了物品行为。
    - `description`：（对象）描述。
        - `identifier`：（字符串）该物品的 ID。
    - `components`：（对象）构成该物品行为的各种组件。

## 学习原版

以苹果（`items/apple.json`）的定义文件为例：

```json
{
    "format_version": "1.10",
    "minecraft:item": {
        "description": {
            "identifier": "minecraft:apple"
        },

        "components": {
            "minecraft:use_duration": 32, // 使用该物品耗时 32 刻
            "minecraft:food": {
                // 该物品是个食物。以下参数都是针对食物的设定，详细介绍请见下。
                "nutrition": 4,
                "saturation_modifier": "low"
            }
        }
    }
}
```

可以看到，物品的定义文件其实非常简单，没有实体的定义文件中组件组、事件等的复杂逻辑。

## 自造轮子

没什么思路，我就瞎玩了。让苹果可以在沙子上种出 TNT。顺便闲得没事儿干把苹果的最大堆叠数量改到 `42` 好了。

所有你需要的物品组件可以在附录中找到。不过如果找不到，可以尝试寻找替代方案。

首先，要修改的是已经有的物品定义文件，因此直接在我们的行为包中创建 `items/apple.json`，将原版苹果的内容复制下来。

然后，新增一个 `minecraft:seed` 组件，设置能种植在沙子上，种出的作物是 TNT。

最后新增一个 `minecraft:max_stack_size` 组件，设置为 `42`。最终结果如下：

```json
{
    "format_version": "1.10",
    "minecraft:item": {
        "description": {
            "identifier": "minecraft:apple"
        },
        "components": {
            "minecraft:use_duration": 32, // 使用该物品耗时 32 刻
            "minecraft:food": {
                // 该物品是个食物。以下参数都是针对食物的设定，详细介绍请见下。
                "nutrition": 4,
                "saturation_modifier": "low"
            },
            "minecraft:seed": {
                "crop_result": "minecraft:tnt",
                "plant_at": [
                    "minecraft:sand"
                ]
            },
            "minecraft:max_stack_size": 42
        }
    }
}
```

退出再重进世界。

苹果堆叠上限变为 42：  
![42.gif](https://i.loli.net/2019/10/24/Lya2PmFcRfCOkJj.gif)

拿着苹果对沙子顶部右键可以放下 TNT ：  
![image.png](https://i.loli.net/2019/10/24/ALSwTxe4y3jH2lG.png)

## 课后习题

我不知道。改点儿你自己喜欢的物品吧。如果你会做资源包的话，甚至可以自己试试加新物品。

之后章节会把「课后习题」这一部分直接删掉了 <3。

[page]

# 附录：组件

特别指明，此处的组件名必须要加 `minecraft:` 命名空间前缀，否则不会被读取。

## minecraft:block （字符串）

不明。会把该物品显示为指定的方块的模型，但是右键没有效果。

示例（`items/camera.json`）：
```json
{
    "minecraft:block": "minecraft:camera"
}
```

## minecraft:camera

使该物品具有相机的特性。

- `black_bars_duration`：（数字）不明。
- `black_bars_screen_ratio`：（数字）不明。
- `shutter_duration`：（数字）不明。
- `picture_duration`：（数字）不明。
- `slide_away_duration`：（数字）不明。

示例（`items/camera.json`）：
```json
{
    "minecraft:camera": {
        "black_bars_duration": 0.2,
        "black_bars_screen_ratio": 0.08,
        "shutter_duration": 0.2,
        "picture_duration": 1.0,
        "slide_away_duration": 0.2
    }
}
```

## minecraft:foil （布尔）

使该物品自带附魔的光效。

示例（`items/appleEnchanted.json`）：
```json
{
    "minecraft:foil": true
}
```

## minecraft:food

使该物品具有食物的特性。

- `nutrition`：（数字）该食物能够恢复的饥饿值。
- `can_always_eat`：（布尔）是否在不饿的情况下也能吃该食物。
- `using_converts_to`：（字符串）当该食物被吃掉以后会转换为什么物品。可以做出喝掉蜂蜜瓶后得到空玻璃瓶的效果。
- `saturation_modifier`：（字符串）该食物所设置的饱食度。可选值：`poor`、`low`、`normal`、`good`、`high` 和 `supernatural`。
- `effects`：（数组）该食物所能给予玩家的状态效果。
    - *（对象）一个状态效果*
        - `name`：（字符串）该状态效果的 ID。
        - `chance`：（数字）获得该状态效果的几率。取值应在 [0.0, 1.0]。
        - `duration`：（数字）该状态效果的持续时间。单位为秒。
        - `amplifier`：（数字）该状态效果的等级。`0` 表示 1 级，`1` 表示 2 级，以此类推。
- `is_meat`：（布尔）该食物是否为肉类食物。效果不明。
- `on_use_action`：（字符串）食用后执行的行为。可选值：`chorus_teleport`（紫颂果的传送）、`suspicious_stew_effect`（迷之炖菜的给予效果）。
- `on_use_range`：（数组 `[x, y, z]`）食用后执行的行为的范围。目前只对紫颂果的传送有效，用于限制在各个方向上传送的距离。
- `cooldown_type`：（字符串）食用该物品后的冷却类型。可选值：`chorusfruit`（紫颂果）。
- `cooldown_time`：（数字）食用该物品后的冷却时长。单位为刻。目前只被紫颂果使用。

示例（附魔金苹果 `items/appleEnchanted.json`）：
```json
{
    "minecraft:food": {
        "nutrition": 4,
        "saturation_modifier": "supernatural",
        "can_always_eat": true,
        "effects": [
            {
                "name": "regeneration",
                "chance": 1.0,
                "duration": 30,
                "amplifier": 4
            },
            {
                "name": "absorption",
                "chance": 1.0,
                "duration": 120, // 2 * 60
                "amplifier": 3
            },
            {
                "name": "resistance",
                "chance": 1.0,
                "duration": 300,
                "amplifier": 0
            },
            {
                "name": "fire_resistance",
                "chance": 1.0,
                "duration": 300,
                "amplifier": 0
            }
        ]
    }
}
```

## minecraft:hand_equipped （布尔）

决定该物品拿到手上后是否像工具一样竖起来。

设置为 `true` 的效果：  
![image.png](https://i.loli.net/2019/10/24/d2rwEVa3XoWYn7K.png)

设置为 `false`（不设置）的效果：  
![image.png](https://i.loli.net/2019/10/24/BF4ZcnequG63dVo.png)

示例（`items/appleEnchanted.json`）：
```json
{
    "minecraft:hand_equipped": false
}
```

## minecraft:max_damage （数字）

该物品的总耐久度。当耗损到达这个值以后物品会破碎。

目前似乎并没有应用。

示例（`items/clownfish.json`）：
```json
{
    "minecraft:max_damage": 0
}
```

## minecraft:max_stack_size （数字）

该物品的最大堆叠上限。

设置为大于 64 的数字将能在部分位置突破 64 的上限，不过有些诡异。当堆叠数量超过 99 以后就不会显示具体数字，只会显示为 `99+`。

示例（`items/beetroot_soup.json`）：
```json
{
    "minecraft:max_stack_size": 1
}
```

## minecraft:seed

该物品可作为种子使用。

- `crop_result`：（字符串）一个方块 ID。将被作为该物品种植后的产物。
- `plant_at`：（数组）可选。默认为耕地。该种子所能种植到的所有方块。
    - *（字符串）能种植该种子的方块的 ID。*

示例（`items/beetroot_seeds.json`）：
```json
{
    "minecraft:seed": {
        "crop_result": "beetroot"
    }
}
```

## minecraft:stacked_by_data （布尔）

具有不同数据值的该物品是否不能互相堆叠。

示例（`items/appleEnchanted.json`）：
```json
{
    "minecraft:stacked_by_data": true
}
```

## minecraft:use_duration （数字）

使用该物品所持续的时长。单位为刻。

示例（`items/apple.json`）：
```json
{
    "minecraft:use_duration": 32
}
```

[page]

# 配方

行为包可以修改或增加各种配方，包括有序合成配方、无序合成配方（工作台、制图台、切石机）、熔炼配方（熔炉、烟熏炉、高炉、营火）、混合酿造配方、换容酿造配方等。

[page]

# 有序合成配方

## 简述

有序合成，指的是材料必须在工作台内按照一定顺序摆放才能获得输出物品的合成方式。

## JSON 格式

- `format_version`：（字符串）文件格式版本。本帖一律写为 `1.14.0`。
- `minecraft:recipe_shaped`：（对象）表明该文件是一个有序合成配方。
    - `description`：（对象）描述。
        - `identifier`：（字符串）该配方的 ID。
    - `group`：（字符串）该配方所属的分组。分组可以是任意字符串，具有相同分组的配方会在配方书中被显示在一起。
    - `tags`：（字符串数组）该配方可以用于的位置。位置只能是 `crafting_table`（工作台），因为目前只有工作台能接受有序合成配方。
    - `key`：（对象）该配方用到的字符和物品的对应。
        - *字符*：（对象）该字符对应的物品。
            - `item`：物品 ID。
            - `data`：物品数据值。
    - `pattern`：（字符串数组）该配方的形状。用字符来表示一个物品。需要把指定物品这么摆才能合成。此处不好用语言描述，见下方示例即可。
    - `result`：（对象）该配方产生的结果。
        - `item`：物品 ID。
        - `data`：物品数据值。
        - `count`：物品数量。

## 学习原版

以金合欢船（`recipes/acacia_boat.json`）的合成配方为例：

```json
{
    "format_version": "1.12",
    "minecraft:recipe_shaped": {
        "description": {
            "identifier": "minecraft:acacia_boat"
        },
        "tags": ["crafting_table"], // 该配方可用于工作台。
        "key": {
            "P": {
                // 将 P 这个字符对应为木锹。
                "item": "minecraft:wooden_shovel"
            },
            "#": {
                // 将 # 这个字符对应为数据值为 4 的木板（即金合欢木板）
                "item": "minecraft:planks",
                "data": 4
            }
        },
        "pattern": [
            // 在这里用 # 和 P 两个字符来拼出船的合成配方的图案。
            // # 和 P 分别代表的物品定义在上面的 `key` 里面。
            // 如果你想要表示空的栏位，使用空格即可。
            "#P#",
            "###"
            // 需要注意的是，这个数组里面最多只能有三个字符串，最少要有一个字符串。每个字符串中包含的字符数应该相同，并且字符数要位于 1 至 3 之间。
        ],
        "result": {
            // 合成结果是数据值为 4 的船，即金合欢船。
            "item": "minecraft:boat",
            "data": 4
        }
    }    
}
```

## 自造轮子

#### 修改金合欢船的配方

目标：把金合欢船的配方改成和 Java 版一样，也就是不需要木锹就能合成。

很简单，把 `pattern` 里面代表木锹的 `P` 换成空格（` `），然后顺手删掉 `key` 里面的 `P` 即可（其实不删也行）。

```json
{
    "format_version": "1.12",
    "minecraft:recipe_shaped": {
        "description": {
            "identifier": "minecraft:acacia_boat"
        },
        "tags": ["crafting_table"], // 该配方可用于工作台。
        "key": {
            "#": {
                // 将 # 这个字符对应为数据值为 4 的木板（即金合欢木板）
                "item": "minecraft:planks",
                "data": 4
            }
        },
        "pattern": [
            // 在这里用 # 这个字符来拼出船的合成配方的图案。
            // 如果你想要表示空的栏位，使用空格即可。
            "# #",
            "###"
            // 需要注意的是，这个数组里面最多只能有三个字符串，最少要有一个字符串。每个字符串中包含的字符数应该相同，并且字符数要位于 1 至 3 之间。
        ],
        "result": {
            // 合成结果是数据值为 4 的船，即金合欢船。
            "item": "minecraft:boat",
            "data": 4
        }
    }    
}
```

#### 新增配方

这回我们不覆盖原有的东西了。我们加新的有序合成配方！

目标：用草和土块能够合成出草方块。

> 基岩版中，草（不管是一格高的还是两格高的）都是 `tall_grass`，草方块叫做 `grass`，挺反直觉的。

在你的行为包下创建文件 `recipes/grass_from_dirt_and_tallgrass.json`（文件名其实可以随意，只要是小写英文 + 下划线的组合就行）。

先搭好架子：

```json
{
    "format_version": "1.14.0",
    "minecraft:recipe_shaped": {
        "description": {
            "identifier": "spgoding:grass_from_dirt_and_tallgrass"
        },
        "tags": ["crafting_table"],
        "key": {
            
        },
        "pattern": [

        ],
        "result": {
            "item": "minecraft:grass"
        }
    }
}
```

涉及到的材料有草和土块，因此我们首先在 `key` 中定义这两个物品所对应的字符。就让 `w` 对应草，`#` 对应土块好了 wwwww：

```json
"key": {
    "w": {
        "item": "minecraft:tallgrass"
    },
    "#": {
        "item": "minecraft:dirt"
    }
}
```

然后把图案设计成草（`w`）要放在土块（`#`）上面：

```json
"pattern": [
    "w",
    "#"
]
```

然后，就没有然后了！最终结果：

```json
{
    "format_version": "1.14.0",
    "minecraft:recipe_shaped": {
        "description": {
            "identifier": "spgoding:grass_block_from_dirt_and_grass"
        },
        "tags": ["crafting_table"],
        "key": {
            "w": {
                "item": "minecraft:tallgrass"
            },
            "#": {
                "item": "minecraft:dirt"
            }
        },
        "pattern": [
            "w",
            "#"
        ],
        "result": {
            "item": "minecraft:grass"
        }
    }
}
```

退出再重进世界。

游戏会自动读取 `recipes` 目录下的所有配方，因此我们不需要在别的什么文件里面设置就可以直接在工作台里使用这个配方了！这一点和前面的战利品表、交易表、生成规则等不同。

> 题外话：事实上实体和物品也是会自动读取的，不过因为它们还需要配合资源包才能有该有的效果，而资源包又不属于本教程的范畴，因此我没有在前文提及。

![image.png](https://i.loli.net/2019/10/24/JIKv84ZuGaT1EPO.png)

[page]

# 无序合成配方

## 简述

无序合成，指的是只要材料的种类、数目够了，就可以获得输出物品的配方。

## JSON 格式

- `format_version`：（字符串）文件格式版本。本帖一律写为 `1.14.0`。
- `minecraft:recipe_shapeless`：（对象）表明该文件是一个无序合成配方。
    - `description`：（对象）描述。
        - `identifier`：（字符串）该配方的 ID。
    - `group`：（字符串）该配方所属的分组。分组可以是任意字符串，具有相同分组的配方会在配方书中被显示在一起。
    - `tags`：（字符串数组）该配方可以用于的位置。位置可以是 `crafting_table`（工作台）、`cartography_table`（制图台）或 `stonecutter`（切石机）。
    - `ingredients`：（数组）该配方用到的所有材料。如果用于工作台，最多可以有九个材料；如果用于制图台，最多可以有两个材料；如果用于切石机，必须有且只有一个材料。如果材料的数目与 `tags` 中定义的位置不符合，则该配方不会被加载。
        - *（对象）一个材料物品*
            - `item`：物品 ID。
            - `data`：物品数据值。
    - `result`：（对象）该配方产生的结果。
        - `item`：物品 ID。
        - `data`：物品数据值。
        - `count`：物品数量。

## 学习原版

以砖块旗帜图案的合成配方（`recipes/banner_pattern_bricks.json`）为例，只要工作台的合成栏中放有一格纸和一格砖块即可合成旗帜图案：

```json
{
    "format_version": "1.12",
    "minecraft:recipe_shapeless": {
        "description": {
            "identifier": "minecraft:banner_pattern_bricks"
        },
        "tags": ["crafting_table"],
        "group": "banner_pattern",
        "ingredients": [
            {
                "item": "minecraft:paper"
            },
            {
                "item": "minecraft:brick_block"
            }
        ],
        "result": {
            "item": "minecraft:banner_pattern",
            "data": 4
        }
    }
}
```

以圆石台阶的切石机配方（`recipes/stonecutter_cobblestone_slab.json`）为例，只要在切石机中放置 1 个圆石即可合成 2 个圆石台阶：

```json
{
    "format_version": "1.12",
    "minecraft:recipe_shapeless": {
        "description": {
            "identifier": "minecraft:stonecutter_cobbledouble_stone_slab"
        },        
        "tags": ["stonecutter"],
        "priority": 0,
        "ingredients": [
            {
                "item": "minecraft:cobblestone",
                "data": 0
            }
        ],
        "result": {
            "item": "minecraft:double_stone_slab",
            "data": 3,
            "count": 2
        }
    }    
}
```

以空地图的制图台配方（`recipes/cartography_table_map.json`）为例，只要在制图台的两个输入格中任一格放入纸即可合成空地图：

```json
{
    "format_version": "1.12",
    "minecraft:recipe_shapeless": {
        "description": {
            "identifier": "minecraft:cartography_table_map"
        },        
        "tags": ["cartography_table"],
        "ingredients": [
            {
                "item": "minecraft:paper"
            }
        ],
        "result": {
            "item": "minecraft:emptymap",
            "data": 0
        }
    }
}
```

## 自造轮子

目标：用切石机能把草方块切碎为草。

在你的行为包下创建文件 `recipes/stonecutter_tallgrass_from_grass.json`（文件名其实可以随意，只要是小写英文 + 下划线的组合就行。切石机的配方以 `stonecutter_` 开头是原版行为包中做的事情，遵循传统总是好的）。

非常简单，一步到位：

```json
{
    "format_version": "1.14.0",
    "minecraft:recipe_shapeless": {
        "description": {
            "identifier": "spgoding:stonecutter_tallgrass_from_grass"
        },
        "tags": ["stonecutter"],
        "ingredients": [
            {
                "item": "minecraft:grass"
            }
        ],
        "result": {
            "item": "minecraft:tallgrass"
        }
    }
}
```

退出再重进世界。

![image.png](https://i.loli.net/2019/10/24/MHLE9GJoNi2TPF4.png)

[page]

# 熔炼配方

## 简述

熔炼配方，指的是放到炉子（广义，包含熔炉、高炉、烟熏炉、营火）里面能得到物品的配方。

## JSON 格式

- `format_version`：（字符串）文件格式版本。本帖一律写为 `1.14.0`。
- `minecraft:recipe_furnace`：（对象）表明该文件是一个熔炼配方。
    - `description`：（对象）描述。
        - `identifier`：（字符串）该配方的 ID。
    - `group`：（字符串）该配方所属的分组。分组可以是任意字符串，具有相同分组的配方会在配方书中被显示在一起。
    - `tags`：（字符串数组）该配方可以用于的位置。位置可以是 `furnace`（熔炉）、`blast_furnace`（高炉）、`smoker`（烟熏炉）或 `campfire`（营火）。
    - `input`：（字符串）一个物品 ID。该熔炼配方的输入物品。
    - `output`：（字符串）一个物品 ID。该熔炼配方的输出物品。

## 学习原版

以牛排的熔炼配方（`recipes/furnace_beef.json`）为例，生牛排（`beef`）可以在熔炉、烟熏炉或营火中烤成熟牛排（`cooked_beef`）：

```json
{
    "format_version": "1.12",
    "minecraft:recipe_furnace": {
        "description": {
            "identifier": "minecraft:furnace_beef"
        },
        "tags": ["furnace", "smoker", "campfire"],
        "input": "minecraft:beef",
        "output": "minecraft:cooked_beef"
    }
}
```

## 自造轮子

目标：用熔炉、高炉能把草方块烤成土块。

在你的行为包下创建文件 `recipes/furnace_dirt_from_grass.json`（文件名其实可以随意，只要是小写英文 + 下划线的组合就行。熔炼配方以 `furnace_` 开头是原版行为包中做的事情，遵循传统总是好的）。

非常简单，一步到位：

```json
{
    "format_version": "1.14.0",
    "minecraft:recipe_furnace": {
        "description": {
            "identifier": "spgoding:furnace_dirt_from_grass"
        },
        "tags": ["furnace", "blast_furnace"],
        "input": "minecraft:grass",
        "output": "minecraft:dirt"
    }
}
```

退出再重进世界。

![image.png](https://i.loli.net/2019/10/24/oLYAHysl6umgbpq.png)

[page]

# 混合酿造配方

## 简述

混合酿造配方，指的是将一个具有某种状态效果的药水放入酿造台后，再加入某个物品，可以酿造出具有另一种状态效果的药水的配方。

## JSON 格式

- `format_version`：（字符串）文件格式版本。本帖一律写为 `1.14.0`。
- `minecraft:recipe_brewing_mix`：（对象）表明该文件是一个混合酿造配方。
    - `description`：（对象）描述。
        - `identifier`：（字符串）该配方的 ID。
    - `group`：（字符串）该配方所属的分组。分组可以是任意字符串，具有相同分组的配方会在配方书中被显示在一起。
    - `tags`：（字符串数组）该配方可以用于的位置。位置只能是 `brewing_stand`（酿造台）。
    - `input`：（字符串）一个形如 `minecraft:potion_type:<药水类型>` 的药水类型 ID。该配方接受的药水类型。
    - `reagent`：（字符串）一个物品 ID。进行该酿造所需要添加的额外物品。
    - `output`：（字符串）一个形如 `minecraft:potion_type:<药水类型>` 的药水类型 ID。该配方的输出的药水类型。

## 学习原版

以力量药水的酿造配方（`recipes/brew_awkward_blaze_powder.json`）为例，当放入粗制的药水以及烈焰粉时，能够酿造出力量药水：

```json
{
    "format_version": "1.12",
    "minecraft:recipe_brewing_mix": {
        "description": {
            "identifier": "minecraft:brew_awkward_blaze_powder"
        },
        "tags": ["brewing_stand"],
        "input": "minecraft:potion_type:awkward",
        "reagent": "minecraft:blaze_powder",
        "output": "minecraft:potion_type:strength"
    }
}
```

以延长版力量药水的酿造配方（`recipes/brew_strength_redstone.json`）为例，当放入普通的力量药水以及红石粉后，能够酿造出延长版力量药水：

```json
{
    "format_version": "1.12",
    "minecraft:recipe_brewing_mix": {
        "description": {
            "identifier": "minecraft:brew_strength_redstone"
        },
        "tags": ["brewing_stand"],
        "input": "minecraft:potion_type:strength",
        "reagent": "minecraft:redstone",
        "output": "minecraft:potion_type:long_strength"
    }
}
```

## 自造轮子

不造了，酿造没意思。

[page]

# 换容酿造配方

## 简述

换容酿造配方，指的是一瓶药水被酿造后将会改变容器的配方。例如从普通玻璃瓶转换为喷溅型玻璃瓶或滞留型玻璃瓶等。

## JSON 格式

- `format_version`：（字符串）文件格式版本。本帖一律写为 `1.14.0`。
- `minecraft:recipe_brewing_container`：（对象）表明该文件是一个换容酿造配方。
    - `description`：（对象）描述。
        - `identifier`：（字符串）该配方的 ID。
    - `group`：（字符串）该配方所属的分组。分组可以是任意字符串，具有相同分组的配方会在配方书中被显示在一起。
    - `tags`：（字符串数组）该配方可以用于的位置。位置只能是 `brewing_stand`（酿造台）。
    - `input`：（字符串）一个物品 ID。该配方接受的药水瓶的 ID。可选：`minecraft:potion`（普通玻璃瓶）、`minecraft:splash_potion`（喷溅型）、`minecraft:lingering_potion`（滞留型）。
    - `reagent`：（字符串）一个物品 ID。进行该酿造所需要添加的额外物品。
    - `output`：（字符串）一个物品 ID。该配方输出的药水瓶的 ID。可选：`minecraft:potion`（普通玻璃瓶）、`minecraft:splash_potion`（喷溅型）、`minecraft:lingering_potion`（滞留型）。

## 学习原版

以喷溅型药水的酿造配方（`recipes/brew_potion_sulphur.json`）为例，当放入普通的玻璃瓶乘装的药水以及火药时，能够酿造出喷溅型的对应药水：

```json
{
    "format_version": "1.12",
    "minecraft:recipe_brewing_container": {
        "description": {
            "identifier": "minecraft:brew_potion_sulphur"
        },
        "tags": ["brewing_stand"],
        "input": "minecraft:potion",
        "reagent": "minecraft:gunpowder",
        "output": "minecraft:splash_potion"
    }
}
```

## 自造轮子

目标：用草能把喷溅型药水变回普通药水。（没错，我和草杠上了）。

在你的行为包下创建文件 `recipes/brew_splash_potion_tallgrass.json`（文件名其实可以随意，只要是小写英文 + 下划线的组合就行。酿造配方以 `brew_` 开头、中间写输入的药水、最后写额外添加的物品是原版行为包中做的事情，遵循传统总是好的）。

非常简单，一步到位：

```json
{
    "format_version": "1.14.0",
    "minecraft:recipe_furnace": {
        "description": {
            "identifier": "spgoding:brew_splash_potion_tallgrass"
        },
        "tags": ["brewing_stand"],
        "input": "minecraft:splash_potion",
        "reagent": "minecraft:tallgrass",
        "output": "minecraft:potion"
    }
}
```

退出再重进世界。

好，遗憾的是，并没有用。酿造台的那几个格子所能接受的物品是写死的，即使你在配方中写了 `tallgrass`，草也照样放不进去。这一目标以失败告终。

由此可见，酿造相关的配方其实可操作性并不强。它既不可以添加原版没有的药水，也不能让原版不支持的物品参与到酿造当中。

[page]

# 方块

[page]

# 结构

[page]

# 生物群系

行为包能够修改或增加生物群系。

## 简述

> 如果你不知道什么叫作“生物群系”，我可以告诉你，它是游戏中用于设置何种地表（沙子？还是草？），是下雨还是下雪，长什么树，以及允许何种动物生成的气候区。
> 
> —— [Jeb](https://minecraft-zh.gamepedia.com/Jens_Bergensten)  
> —— [Wiki](https://minecraft-zh.gamepedia.com/%E7%94%9F%E7%89%A9%E7%BE%A4%E7%B3%BB)

简单来讲，生物群系可以修改：

- 这块儿的地形
- 地表和地底的构成
- 装饰结构（树、草等）的分布
- 能够生成的生物
- 气候（下雨还是下雪，或者是永远干旱）

## JSON 格式

- `format_version`：（字符串）文件格式版本。本帖一律写为 `1.14.0`。
- `minecraft:biome`：（对象）表明该文件定义了生物群系。
    - `description`：（对象）描述。
        - `identifier`：（字符串）该生物群系的 ID。
    - `components`：（对象）构成该生物群系的各种组件。此外这里也能定义该生物群系的标签，具体内容请看附录。

[page]

# 函数

行为包中可以像 Java 版的数据包一样放置命令函数（简称函数）。函数实际上就是由一系列命令组成的文件，格式十分简单，每行一条命令，可以有空行，可以有注释。

所有函数应该放置在行为包根目录的 `functions` 文件夹及它的子文件夹下，文件名后缀为 `.mcfunction`。

## 创建第一个函数

首先，创建文件 `functions/test.mcfunction`。在其中书写以下内容：

```mcfunction
# 这是我的第一个函数

say Hello,
say World!
```

重载数据包。

在游戏中输入命令 `/function test`，观察聊天栏：

![test.png](https://i.loli.net/2019/10/24/3ieVHjpJk5hUxRf.png)

恭喜你，在基岩版中写出了第一个命令函数。

## JSON 格式

`mcfunction` 文件的格式很简单，每行写一条命令，可以有空行，可以有 `#` 开头的行。`#` 开头的行会被认为是注释，游戏会直接跳过这一行的内容。使用注释可以给自己留一些备注，方便后续更改。或者是在调试时临时把一条命令注释掉，使这条命令不执行。

函数中的命令会按顺序从上到下执行，因此刚刚那个函数执行后你先看到的是 `Hello,`，后看到的是 `World!`。

## `function` 命令

`function` 命令可以用于执行行为包中放置的函数。该命令有一个参数，表明的是要执行的函数的路径。在刚刚的例子中，你的函数路径是 `functions/test.mcfunction`，因此 `test` 即为它的路径。假设你的函数放到了 `functions/a/b/c/d.mcfunction`，则 `a/b/c/d` 是它的路径，需要使用 `/function a/b/c/d` 来执行该函数。

在函数内部同样可以执行 `function` 命令，你可以借此做出递归等操作。

## `reload` 命令

使用 `reload` 命令可以在不退出游戏的情况下重载**已经被加载的函数**。

例如，在刚刚的示例中，我们在 `test.mcfunction` 里面再加一行命令：

```mcfunction
# 这是我的第一个函数
# 修改过的

say Hello,
say World!
say ======
```

只需要在游戏里面输入 `/reload` 即可重新加载这个函数的内容，执行 `/function test` 就可以看到改动了。

但是，如果你新加了一个函数文件（例如 `functions/test114514.mcfunction`），执行 `reload` 命令并不能加载它，必须要退出存档再重进才能加载这个函数。

## 基岩版命令

所有基岩版命令可以在 [Wiki][https://minecraft-zh.gamepedia.com/%E5%91%BD%E4%BB%A4#.E5.91.BD.E4.BB.A4.E5.88.97.E8.A1.A8.E5.8F.8A.E5.85.B6.E6.A6.82.E8.BF.B0] 上查询，本文不再赘言。

[page]

# 脚本

这部分难度系数飙升，我先咕几个月。如果你不熟悉编程以及 JavaScript 的话，不要指望将来等我更了这部分你能看得懂……

[page]

# 发布

进入 Minecraft 放置行为包的文件夹：`%localappdata%\Packages\Microsoft.MinecraftUWP_8wekyb3d8bbwe\LocalState\games\com.mojang\development_behavior_packs`，再进入你几年前（？）创建的文件夹（本教程的为 `spg_best`），然后全选所有文件并压缩为 zip 格式。将压缩包的后缀 `.zip` 重命名为 `.mcpack`，这个 `.mcpack` 文件就可以拿去发布了。玩家只需双击这个文件 Minecraft 就会自动完成导入。

本教程所有章节中「自造轮子」部分涉及到的内容全部打包到 `spg_best` 行为包当中了，在此提供下载。内容并不多，一共只有几个文件，而且收费整整 50 金粒，挺坑的：

（帖子还没更完，没有下载）

[page]

# 参考资料

- CC BY-NC-SA 3.0: [wiki - Add-on](https://minecraft.gamepedia.com/Add-on) 及其中列出的各页面
- CC BY 4.0: [Skylinerw - Loot Tables](https://github.com/skylinerw/guides/blob/master/bedrock/loot%20tables.md)
- rua: [ruhuasiyu - 【1.13】数据包(原版模组)入门教程](http://www.mcbbs.net/thread-784662-1-1.html) 

# 致谢

- [MM2BC](https://www.mcbbs.net/thread-779668-1-1.html)：本文的命可以说是它给的了。

[page]

# 后语

看到大家都来这里水三个命令方块的东西然后拿走基岩版爱好者勋章，我也想来拿。但是水命令方块的东西太没有水平了，我当然要水些别的。于是，断断续续拖了一年多，这篇不知道是教程还是水文的东西就这么出来了，让您见笑了。就像我在简介里说的一样，

> 我不指望、也不可能在这一帖中用仅仅十几万的字符就彻底讲明白这到底是个什么东西。看完这一帖后我能保证你会对行为包有一个大致的了解，知道它能做到什么，以及怎么做。

我有着十足的自信，任何一名玩家在认真地看过这篇水文以后，都能够对行为包有着足够的认知。不过，也就到此为止了，更多的还是要靠自己。

基岩版的 Add-on 是十分强大的一个功能，作为一名 Java 版的忠实玩家其实是非常欣赏 Add-on 的这种开放性的。当然，我不会转行，Java 版仍然是我的家。不过我也迫切地希望能够在论坛看到优秀的原创 Add-on 作品。各位 dalaoes 不要吝啬自己的才华！

教程中有部分内容没有官方文档依据，仅靠测试、查看原版行为包文件以及在游戏内进行测试推断而出，如有错误请务必指正。另外还有一部分我本人推断不出的内容，在网上查询资料未果，在教程内统一标注为「不明」，希望有了解这部分内容的玩家不吝赐教！在本帖下方回复指出错漏，或帮助补充内容，可以获得本人的人气和金粒奖励！

后续还会视情况发布有关资源包的教程，加入动画、粒子效果、UI 界面等内容。将行为包与资源包结合到一起以后，便可自行任意添加新方块、物品、实体等，无人能当，十分愉快。不过，也有可能就此截止，还是要看反响。
