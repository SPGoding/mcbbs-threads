[index]  
[#1]简介  
[#2]准备  
[#3]开始制作  
[#4]实体  
*[#TID,PID]附录：组件  
*[#5]附录：滤器  
*[#6]附录：事件  
[#7]战利品表  
*[#8]附录：函数  
*[#9]附录：条件  
[#10]交易表  
*[#11]附录：函数  
[#12]生成规则  
*[#13]附录：条件  
[#14]物品  
*[#15]附录：组件  
[#16]配方  
*[#17]有序合成配方  
*[#18]无序合成配方  
*[#19]熔炼配方  
*[#20]混合酿造配方  
*[#21]换容酿造配方  
[#22]生物群系  
[#23]结构  
[#24]方块  
[#25]函数  
[#26]脚本（WIP）  
[#27]发布  
[#28]参考资料  
[#29]后语
[/index]

# 本帖目前仍在进行大幅变动，请几天后再看

# 简介

行为包（behavior pack），是 Minecraft 基岩版（本帖后文除非特别标注，Minecraft 一概指代基岩版）所独有的特性。利用行为包，可以在不改变游戏源代码的情况下修改许多东西，包括：生物群系、方块、结构、物品、合成配方、实体行为等。行为包与 Add-on 中的另一者「资源包」的区别在于，行为包修改的是服务端层面的内容，而资源包修改的则是客户端层面的内容，因此行为包只需要在服务器中启用，而资源包必须自己安装才能看到效果。

本教程顶部目录可点。由于帖子内容十分之多，每一次更新都会卡顿几分钟，因此论坛这边不会经常更新。可以在 [GitHub](https://github.com/SPGoding/mcbbs-threads/blob/master/tutorials/behavior-pack/thread.md) 上查看最新版本。

本教程使用版本为 1.14.0.2，将来版本可能会在细节上有细微变化，建议看教程时自己对照当时最新版本的原版行为包的格式（通常可以在[英文 Wiki 的 Add-on 页面](https://minecraft.gamepedia.com/Add-on)找到原版行为包的下载地址）。

本教程采用 CC BY-NC-SA 3.0 协议发布，因为其中大量使用到了英文 Wiki 的内容。

> 在本人编写教程时，1.14.0.2 仍为 beta 测试版。想要参与 beta 测试，需要在微软商城中搜索 Xbox Insider Hub 并安装。如果你的微软帐号中年龄并不是成年，可以注册另一个成年微软帐号并把该帐号加入家庭，给予权限。基本操作，暂且不言。

[page]

# 准备

## 文本编辑器

行为包中涉及到了大量的 JSON 文件，个人并不推荐使用系统自带的记事本进行编辑，一是因为编码问题，还有就是因为没有语法高亮以及缩进，很容易出错。所以建议下载一个专门的文本编辑器。

个人偏爱微软的 [VSCode](https://code.visualstudio.com/)，如果你已经习惯了诸如 [Notepad++](url=https://notepad-plus-plus.org/download/) 之类的文本编辑器也是可以的。

在编辑文件时，**请使用 `UTF-8 无 BOM` 编码**，否则可能会出现一些诡异的问题。

### VSCode 设置方法

输入 `Ctrl + Shift + P`，输入 `>Change File Encoding`，选择 `Save With Encoding`，选择 `UTF-8`。

### Notepad++ 设置方法

在上方展开「格式」，选择「转为 UTF-8 编码格式」。

## 显示文件拓展名

为了方便以后处理，建议开启文件拓展名显示。点击「查看」，勾选「文件拓展名」。

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

之所以在 `development_behavior_packs` 内建立我们的行为包，而不是在 `behavior_packs`，是因为游戏会在加载行为包时自动把外部的行为包在存档内做一个缓冲。这会导致你对行为包的改动不能实时反应到游戏里，造成调试的困难。当你把行为包建立在 `development_behavior_packs` 里面以后，游戏不会进行缓存，想要重载行为包只需要退出世界再重进即可。不过，如果你在行为包内新建了文件，需要彻底关闭游戏再打开才能正确加载那个文件。很遗憾，Minecraft 基岩版没有提供方便的完全重载行为包的方式。

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

# 实体

「行为」包，最重要的便是可以定义实体的行为。

实体定义文件应放置在根目录下的 `entities` 文件夹内。

本部分仍在大幅变动。

## 简述

实体

## 文件格式

- `format_version`：（字符串）格式版本。本教程介绍的是 1.14.0 的写法，因此这里写为 `1.14.0`。写为旧版本的格式也仍然能在最新版游戏中使用，例如原版的药水云的定义文件的格式版本在发帖时仍为 `1.8.0`。事实上，这个参数存在的意义就是为了能够保持向后兼容。
- `minecraft:entity`：（对象）定义实体。
    - `description`：（对象）对实体的宏观概述。
        - `identifier`：（字符串）实体的 ID。
        - `minecraft:is_experimental`：（对象）是否为实验性特性。
            - `value`：（布尔值）如果设置为 `true`，只有在世界选项里勾选「启用实验玩法」，该实体才会能够使用。
        - `minecraft:is_summonable`：（对象）是否可以生成。
            - `value`：（布尔值）如果设置为 `true`，将可以通过 `/summon <identifier>` 命令生成该实体。
        - `minecraft:runtime_identifier`：（对象）运行时标识符。Minecraft 中有一些实体的行为是硬编码的（即写死在代码里面不能改动），例如鸡下蛋（尽管有组件可以做出下蛋的效果，但是鸡并没有使用那个组件）。这个字段可以将已有的硬编码行为赋予给任意实体。
            - `id`：（字符串）要赋予的硬编码行为的实体 ID，例如 `minecraft:chicken`（鸡）就可以让这个实体会下蛋。
        - `minecraft:spawn_egg`：（对象）是否拥有刷怪蛋。
            - `value`：（布尔值）如果设置为 `true`，在创造模式物品栏中将会有该实体的对应刷怪蛋。
    - `components`：（对象）该实体的「组件」。
    - `component_groups`：（对象）该实体的「组件」组，每一个对象下面都储存了一些组件。一些较为复杂的实体会有多个状态，比如牛的「成年状态」、「幼年状态」和「被栓绳拴住」等。Minecraft 会根据你的设定，在某些情况下启用一组组件，或禁用一组组件，来达到实体在不同状态下有不同的行为。
    - `events`：（对象）该实体的所有事件。事件可以在指定条件下执行，执行以后可以禁用/启用在 `component_groups` 中定义的组件组。
        - `事件名`：（对象）一个事件（有关事件的具体介绍见下）。

`components`（组件）与 `component_groups`（组件组）的区别在于，前者中定义的是该实体的基础行为，而后者中则有一些与特定状态有关的行为。另外，如果组件组修改了和组件中相同的行为，在这个组件组被启用时，组件组里定义的值会覆盖掉组件中的值。

下面会对此进行详细的讲解。

## 学习原版

以钓鱼鱼钩（`./entities/fishing_hook.json`）为例，略有删改。

```
{
    "format_version": "1.8.0-beta.1",
    "minecraft:entity": {
        "description": {
            "identifier": "minecraft:fishing_hook",
            "minecraft:spawn_egg": {
                "value": false
            },
            "minecraft:is_summonable": {
                "value": false
            },
            "minecraft:is_experimental": {
                "value": false
            }
        },

        "component_groups": {
            "loot_jungle": {
                "minecraft:loot": {
                    "table": "loot_tables/gameplay/jungle_fishing.json"
                }
            }
        },

        "components": {
            "minecraft:loot": {
                "table": "loot_tables/gameplay/fishing.json"
            }
        },

        "events": {
            "minecraft:entity_spawned": {
                "sequence": [
                    {
                        "filters": {
                            "test": "is_biome",
                            "value": "jungle"
                        },
                        "add": {
                            "component_groups": [
                                "loot_jungle"
                            ]
                        }
                    }
                ]
            }
        }
    }
}
```
在 `components` 中定义了一个 `minecraft:loot` 组件，这个组件实际上是设置实体的战利品的。于是这一部分的效果就是，设置鱼钩的战利品表为 `loot_tables/gameplay/fishing.json`（即默认的钓鱼战利品表）。

在 `events` 中定义了一个 `minecraft:entity_spawned` 事件。目前我们并不太需要了解这个事件的详细工作原理，只需要知道它会在鱼钩处于丛林生态群系时执行，它的 `add` 部分启用了 `loot_jungle` 这个组件组即可。

于是我们回过头看 `component_groups` 中定义的 `loot_jungle` 这个组件组，它设置了 `minecraft:loot` 组件为另一个战利品表 `loot_tables/gameplay/jungle_fishing.json`（即在丛林生态群系下的钓鱼的战利品表）。在组件组 `loot_jungle` 被启用时，原先 `components` 中定义的战利品表就被新的适用于丛林的战利品表覆盖了。

似乎很简单？确实很简单。

做个小练习吧，下面这一串 JSON 取材于牛，并删除了一些看着烦的组件。请你尝试理清各种组件组的关系（组件、事件的作用已写注释）：

```
{
    "format_version": "1.8.0-beta.1",
    "minecraft:entity": {
        "description": {
            "identifier": "minecraft:cow",
            "minecraft:spawn_egg": {
                "value": true
            },
            "minecraft:is_summonable": {
                "value": true
            },
            "minecraft:is_experimental": {
                "value": false
            }
        },
        "component_groups": {
            "minecraft:cow_baby": {
                "minecraft:ageable": {
                    "duration": 1200,
                    "feedItems": "wheat",
                    "grow_up": {
                        "event": "minecraft:ageable_grow_up", // 到一定时间时执行 minecraft:ageable_grow_up 事件
                        "target": "self"
                    }
                }
            },

            "minecraft:cow_adult": {
                "minecraft:breedable": {
                    "requireTame": false,
                    "breedItems": "wheat",
                    "breedsWith": {
                        "mateType": "minecraft:cow",
                        "babyType": "minecraft:cow",
                        "breed_event": {
                            "event": "minecraft:entity_born", // 在繁殖时由新出生的实体执行 minecraft:entity_born 事件
                            "target": "baby"
                        }
                    }
                }
            },

            "minecraft:cow_leashed": {
                "minecraft:behavior.move_towards_restriction": { // 该组件可以让实体跟随缰绳
                    "priority": 2,
                    "speed_multiplier": 1.0
                }
            }
        },

        "components": {
            "minecraft:leashable": {
                "soft_distance": 4.0,
                "hard_distance": 6.0,
                "max_distance": 10.0,
                "on_leash": {
                    "event": "minecraft:on_leash", // 被栓绳拴住时执行 minecraft:on_leash 事件
                    "target": "self"
                },
                "on_unleash": {
                    "event": "minecraft:on_unleash", // 取消栓绳时执行 minecraft:on_leash 事件
                    "target": "self"
                }
            }
        },

        "events": {
            "minecraft:entity_spawned": { // 实体生成时自动执行的事件
                "randomize": [ // 会随机启用下面两个组件组（minecraft:cow_adult 与 minecraft:cow_baby）之一
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
            },

            "minecraft:on_leash": {
                "add": {
                    "component_groups": [
                        "minecraft:cow_leashed"
                    ]
                }
            },
            "minecraft:on_unleash": {
                "remove": {
                    "component_groups": [
                        "minecraft:cow_leashed"
                    ]
                }
            }
        }
    }
}
```
其实还是可以分析出来的，对吧~【啥，没看懂。没关系，静下心来从头再看一遍就好。

## 自造轮子

本教程举一个例子来让大家再进一步熟悉如何编辑实体行为 —— 让豹猫入水爆炸！（豹猫：喵喵喵？）

由于要修改的是原版就有的文件，所以在你的行为包下创建 `entities/ocelot.json` 文件，把原版豹猫的内容复制过去，即可覆盖掉原版的豹猫文件：

[spoiler]

```
{
  "format_version": "1.8.0-beta.1",
  "minecraft:entity": {
    "description": {
      "identifier": "minecraft:ocelot",
      "minecraft:spawn_egg": {
        "value": true
      },
      "minecraft:is_summonable": {
        "value": true
      },
      "minecraft:is_experimental": {
        "value": false
      }
    },
    "component_groups": {
      "minecraft:ocelot_baby": {
        "minecraft:is_baby": {},
        "minecraft:scale": {
          "value": 0.5
        },
        "minecraft:ageable": {
          "duration": 1200,
          "feedItems": [
            "fish",
            "salmon"
          ],
          "grow_up": {
            "event": "minecraft:ageable_grow_up",
            "target": "self"
          }
        }
      },
      "minecraft:ocelot_adult": {
        "minecraft:loot": {
          "table": "loot_tables/entities/ocelot.json"
        },
        "minecraft:scale": {
          "value": 1
        }
      },
      "minecraft:ocelot_wild": {
        "minecraft:trusting": {
          "probability": 0.33,
          "trust_items": [
            "fish",
            "salmon"
          ],
          "trust_event": {
            "event": "minecraft:on_trust",
            "target": "self"
          }
        },
        "minecraft:behavior.tempt": {
          "priority": 4,
          "speed_multiplier": 0.5,
          "within_radius": 16,
          "can_get_scared": true,
          "items": [
            "fish",
            "salmon"
          ]
        },
        "minecraft:behavior.avoid_mob_type": {
          "priority": 5,
          "entity_types": [
            {
              "filters": {
                "test": "is_family",
                "subject": "other",
                "value": "player"
              },
              "max_dist": 10,
              "walk_speed_multiplier": 0.8,
              "sprint_speed_multiplier": 1.33
            }
          ]
        },
        "minecraft:rideable": {
          "seat_count": 1,
          "family_types": [
            "zombie"
          ],
          "seats": {
            "position": [
              0.0,
              0.35,
              0.0
            ]
          }
        }
      },
      "minecraft:ocelot_trusting": {
        "minecraft:behavior.breed": {
          "priority": 3,
          "speed_multiplier": 1.0
        },
        "minecraft:behavior.tempt": {
          "priority": 4,
          "speed_multiplier": 0.5,
          "within_radius": 16,
          "items": [
            "fish",
            "salmon"
          ]
        },
        "minecraft:breedable": {
          "requireTame": false,
          "breedsWith": {
            "mateType": "minecraft:ocelot",
            "babyType": "minecraft:ocelot",
            "breed_event": {
              "event": "minecraft:entity_born",
              "target": "baby"
            }
          },
          "breedItems": [
            "fish",
            "salmon"
          ]
        }
      },
      "minecraft:ocelot_tame": {
        "minecraft:is_tamed": {},
        "minecraft:health": {
          "value": 20,
          "max": 20
        },
        "minecraft:sittable": {},
        "minecraft:leashable": {
          "soft_distance": 4.0,
          "hard_distance": 6.0,
          "max_distance": 10.0,
          "on_leash": {
            "event": "minecraft:on_leash",
            "target": "self"
          },
          "on_unleash": {
            "event": "minecraft:on_unleash",
            "target": "self"
          }
        },
        "minecraft:behavior.follow_owner": {
          "priority": 4,
          "speed_multiplier": 1.0,
          "start_distance": 10,
          "stop_distance": 2
        },
        "minecraft:behavior.stay_while_sitting": {
          "priority": 3
        },
        "minecraft:behavior.ocelot_sit_on_block": {
          "priority": 6,
          "speed_multiplier": 1.0
        }
      }
    },
    "components": {
      "minecraft:health": {
        "value": 10,
        "max": 10
      },
      "minecraft:attack_damage": {
        "value": 4
      },
      "minecraft:nameable": {},
      "minecraft:type_family": {
        "family": [
          "ocelot"
        ]
      },
      "minecraft:breathable": {
        "totalSupply": 15,
        "suffocateTime": 0
      },
      "minecraft:collision_box": {
        "width": 0.6,
        "height": 0.7
      },
      "minecraft:movement": {
        "value": 0.3
      },
      "minecraft:navigation.walk": {
        "can_path_over_water": true,
        "avoid_water": true,
        "avoid_damage_blocks": true
      },
      "minecraft:movement.basic": {},
      "minecraft:jump.static": {},
      "minecraft:can_climb": {},
      "minecraft:fall_damage": {
        "value": 0.0
      },
      "minecraft:behavior.float": {
        "priority": 0
      },
      "minecraft:behavior.panic": {
        "priority": 1,
        "speed_multiplier": 1.25
      },
      "minecraft:behavior.nearest_attackable_target": {
        "priority": 1,
        "attack_interval": 10,
        "reselect_targets": true,
        "entity_types": [
          {
            "filters": {
              "test": "is_family",
              "subject": "other",
              "value": "chicken"
            },
            "max_dist": 8
          },
          {
            "filters": {
              "all_of": [
                {
                  "test": "is_family",
                  "subject": "other",
                  "value": "baby_turtle"
                },
                {
                  "test": "in_water",
                  "subject": "other",
                  "operator": "!=",
                  "value": true
                }
              ]
            },
            "max_dist": 8
          }
        ]
      },
      "minecraft:behavior.mount_pathing": {
        "priority": 1,
        "speed_multiplier": 1.25,
        "target_dist": 0,
        "track_target": true
      },
      "minecraft:behavior.leap_at_target": {
        "priority": 3,
        "target_dist": 0.3
      },
      "minecraft:behavior.ocelotattack": {
        "priority": 4,
        "walk_speed_multiplier": 0.8,
        "sprint_speed_multiplier": 1.33,
        "sneak_speed_multiplier": 0.6
      },
      "minecraft:behavior.random_stroll": {
        "priority": 8,
        "speed_multiplier": 0.8
      },
      "minecraft:behavior.look_at_player": {
        "priority": 9
      },
      "minecraft:physics": {}
    },
    "events": {
      "minecraft:entity_spawned": {
        "sequence": [
          {
            "randomize": [
              {
                "weight": 3,
                "remove": {},
                "add": {
                  "component_groups": [
                    "minecraft:ocelot_adult",
                    "minecraft:ocelot_wild"
                  ]
                }
              },
              {
                "weight": 1,
                "remove": {},
                "add": {
                  "component_groups": [
                    "minecraft:ocelot_baby",
                    "minecraft:ocelot_wild"
                  ]
                }
              }
            ]
          }
        ]
      },
      "minecraft:entity_born": {
        "sequence": [
          {
            "remove": {},
            "add": {
              "component_groups": [
                "minecraft:ocelot_baby",
                "minecraft:ocelot_trusting"
              ]
            }
          }
        ]
      },
      "minecraft:entity_born_wild": {
        "remove": {
          "component_groups": [
            "minecraft:ocelot_trusting"
          ]
        },
        "add": {
          "component_groups": [
            "minecraft:ocelot_baby",
            "minecraft:ocelot_wild"
          ]
        }
      },
      "minecraft:ageable_grow_up": {
        "remove": {
          "component_groups": [
            "minecraft:ocelot_baby"
          ]
        },
        "add": {
          "component_groups": [
            "minecraft:ocelot_adult"
          ]
        }
      },
      "minecraft:on_trust": {
        "sequence": [
          {
            "remove": {
              "component_groups": [
                "minecraft:ocelot_wild"
              ]
            }
          },
          {
            "add": {
              "component_groups": [
                "minecraft:ocelot_trusting"
              ]
            }
          }
        ]
      },
      "minecraft:on_leash": {
        "add": {
          "component_groups": [
            "minecraft:ocelot_leashed"
          ]
        }
      },
      "minecraft:on_unleash": {
        "remove": {
          "component_groups": [
            "minecraft:ocelot_leashed"
          ]
        }
      }
    }
  }
}
```
[/spoiler]

在修改或添加实体时，所有你可能用到的组件、函数等都可以在附录里找到。

我们先添加一个立刻爆炸的组件组 `spgoding:exploding`，在其中加入爆炸的组件：

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
然后添加一个事件 `spgoding:start_explode`，使其启用 `spgoding:exploding` 组件组：

```
"spgoding:start_explode": {
    "add": {
        "component_groups": [
            "spgoding:exploding"
        ]
    }
}
```
最后添加一个组件，当满足指定滤器时执行事件 `spgoding:start_explode`：

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

然后我们重载行为包…

把豹猫推到水里，然后你会惊喜地发现——豹猫并没有爆炸。

为什么呢？我也不知道，留作课后习题。

[page]

## 附录：滤器

滤器能够用于测试指定的项目是否满足你所定义的条件。

### 基本单位


- `test`：（字符串）要测试的项目（详细介绍见下）。
- `subject`：（字符串）测试的主体。只有部分测试需要指定这个字段（详细介绍见下）。
- `domain`：（字符串）域。只有部分测试需要指定这个字段（详细介绍见下）。
- `operator`：（字符串）在指定项目 `test` 方面，主体 `subject` 的 `domain` 与 给定的数据 `value` 的比较方式（详细介绍见下）。
- `value` （字符串）给定的数据。

### 测试项目

[spoiler]

[size=2]**clock_time

当前时间。

将一天的时间均分到了 `[0.00, 1.00]` 中。即：


- `0.00`：正午。
- `0.25`：日落。
- `0.50`：午夜。
- `0.75`：次日日出。
- `1.00`：次日正午。

例子：

完整写法：`{ "test": "clock_time", "subject": "self", "operator": "equals", "value": "0.00" }`

简写：`{ "test": "clock_time", "value": "0.00" }`

[size=2]**has_ability

主体的能力。


- `flySpeed`：飞行速度。
- `walkSpeed`：行走速度。
- `flying`：是否正在飞行。
- `mayfly`：是否能够飞行。
- `instabuild`：是否能够瞬间摧毁方块。
- `invulnerable`：是否无敌。
- `mute`：是否被禁言（教育版特性）。
- `worldbuilder`：是否为地图建造者（教育版特性）。
- `noclip`：（教育版特性）。
- `lightning`：（教育版特性）。

例子：

完整写法：`{ "test": "has_ability", "subject": "self", "operator": "equals", "value": "instabuild" }`

简写：`{ "test": "has_ability", "value": "instabuild" }`

[size=2]**has_biome_tag

拥有指定名称的生态群系标签。

例子：

完整写法：`{ "test": "has_biome_tag", "subject": "self", "operator": "equals", "value": " " }`

简写：`{ "test": "has_biome_tag", "value": " " }`

[size=2]**has_component

主体拥有的组件（所有组件可在上文中找到）。

例子：

完整写法：`{ "test": "has_component", "subject": "self", "operator": "equals", "value": "minecraft:explode" }`

简写：`{ "test": "has_component", "value": "minecraft:explode" }`

[size=2]**has_damage

主体受到伤害的类型。


- `anvil`：铁砧砸伤。
- `attack`：攻击。
- `block_explosion`：方块爆炸。
- `contact`：接触。
- `drowning`：溺水。
- `entity_explosion`：实体爆炸。
- `fall`：摔伤。
- `falling_block`：掉落方块砸伤。
- `fatal`：任何能直接杀死该实体的致命伤害。
- `fire`：燃烧伤害。
- `fire_tick`：不可知。
- `fly_into_wall`：飞行撞墙。
- `lava`：熔岩烧伤。
- `magic`：魔法（药水）。
- `none`：无。
- `override`：覆盖。
- `piston`：活塞。
- `projectile`：弹射物。
- `starve`：饥饿。
- `suffocation`：窒息。
- `suicide`：自杀。
- `thorns`：荆棘。
- `void`：虚空。
- `wither`：凋灵。

例子：

完整写法：`{ "test": "has_damage", "subject": "self", "operator": "equals", "value": "fatal" }`

简写：`{ "test": "has_damage", "value": "fatal" }`

[size=2]**has_equipment

主体拥有的装备。

域：检测的栏位


- `any`：任意。
- `hand`：手。
- `armor`：盔甲。
- `head`：头部。
- `torso`：躯干。
- `leg`：腿。
- `feet`：脚。

例子：

完整写法：`{ "test": "has_equipment", "subject": "self", "domain": "any", "operator": "equals", "value": "dirt" }`

简写：`{ "test": "has_equipment", "value": "dirt" }`

[size=2]**in_caravan

主体是否在车队中。

例子：

完整写法：`{ "test": "in_caravan", "subject": "self", "operator": "equals", "value": "true" }`

简写：`{ "test": "in_caravan" }`

[size=2]**in_clouds

主体是否在云中。

例子：

完整写法：`{ "test": "in_clouds", "subject": "self", "operator": "equals", "value": "true" }`

简写：`{ "test": "in_clouds" }`

[size=2]**in_lava

主体是否在岩浆中。

例子：

完整写法：`{ "test": "in_lava", "subject": "self", "operator": "equals", "value": "true" }`

简写：`{ "test": "in_lava" }`

[size=2]**in_water

主体是否在水中。

例子：

完整写法：`{ "test": "in_water", "subject": "self", "operator": "equals", "value": "true" }`

简写：`{ "test": "in_water" }`

[size=2]**in_water_or_rain

是否在水中或雨中。

例子：

完整写法：`{ "test": "in_water_or_rain", "subject": "self", "operator": "equals", "value": "true" }`

简写：`{ "test": "in_water_or_rain" }`

[size=2]**is_altitude

海拔高度。0 为基岩所在的高度。

例子：

完整写法：`{ "test": "is_altitude", "subject": "self", "operator": "equals", "value": "0" }`

简写：`{ "test": "is_altitude", "value": "0" }`

[size=2]**is_biome

生态群系。

例子：

完整写法：`{ "test": "is_biome", "subject": "self", "operator": "equals", "value": "beach" }`

简写：`{ "test": "is_biome", "value": "beach" }`

[size=2]**is_brightness

亮度。范围在 `[0.0, 1.0]` 之中。

例子：

完整写法：`{ "test": "is_brightness", "subject": "self", "operator": "equals", "value": "0.50" }`

简写：`{ "test": "is_brightness", "value": "0.50" }`

[size=2]**is_climbing

是否正在攀爬。

例子：

完整写法：`{ "test": "is_climbing", "subject": "self", "operator": "equals", "value": "true" }`

简写：`{ "test": "is_climbing" }`

[size=2]**is_color

颜色。


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

例子：

完整写法：`{ "test": "is_color", "subject": "self", "operator": "equals", "value": "white" }`

简写：`{ "test": "is_color", "value": "white" }`

[size=2]**is_daytime

是否正在白天。

例子：

完整写法：`{ "test": "is_daytime", "subject": "self", "operator": "equals", "value": "true" }`

简写：`{ "test": "is_daytime" }`

[size=2]**is_difficulty

游戏难度。


- `peaceful`：和平。
- `easy`：简单。
- `normal`：普通。
- `hard`：困难。

例子：

完整写法：`{ "test": "is_difficulty", "subject": "self", "operator": "equals", "value": "normal" }`

简写：`{ "test": "is_difficulty", "value": "normal" }`

[size=2]**is_family

实体种类。

例子：

完整写法：`{ "test": "is_family", "subject": "self", "operator": "equals", "value": "player" }`

简写：`{ "test": "is_family", "value": "player" }`

[size=2]**is_game_rule

游戏规则。

例子：

完整写法：`{ "test": "is_game_rule", "subject": "self", "domain": "domobspawning", "operator": "equals", "value": "true" }`

简写：`{ "test": "is_game_rule", "domain": "domobspawning" }`

[size=2]**is_humid

主体所在区域的湿度。

例子：

完整写法：`{ "test": "is_humid", "subject": "self", "operator": "equals", "value": "true" }`

简写：`{ "test": "is_humid" }`

[size=2]**is_immobile

主体是否不移动。当失去 AI 目标、刚刚改变维度或者没有生命值时，实体会不移动。

例子：

完整写法：`{ "test": "is_immobile", "subject": "self", "operator": "equals", "value": "true" }`

简写：`{ "test": "is_immobile" }`

[size=2]**is_moving

主体是否移动。

例子：

完整写法：`{ "test": "is_moving", "subject": "self", "operator": "equals", "value": "true" }`

简写：`{ "test": "is_moving" }`

[size=2]**is_owner

主题是否是调用者的主人。

例子：

完整写法：`{ "test": "is_owner", "subject": "self", "operator": "equals", "value": "true" }`

简写：`{ "test": "is_owner" }`

[size=2]**is_riding

主体是否正在骑乘。

例子：

完整写法：`{ "test": "is_riding", "subject": "self", "operator": "equals", "value": "true" }`

简写：`{ "test": "is_riding" }`

[size=2]**is_sneaking

主体是否正在潜行

例子：

完整写法：`{ "test": "is_sneaking", "subject": "self", "operator": "equals", "value": "true" }`

简写：`{ "test": "is_sneaking" }`

[size=2]**is_snow_covered

主体所在区域是否被雪覆盖。

例子：

完整写法：`{ "test": "is_snow_covered", "subject": "self", "operator": "equals", "value": "true" }`

简写：`{ "test": "is_snow_covered" }`

[size=2]**is_target

主体是否是调用者的目标。

例子：

完整写法：`{ "test": "is_target", "subject": "self", "operator": "equals", "value": "true" }`

简写：`{ "test": "is_target" }`

[size=2]**is_temperature_type

当前温度类型。


- `cold`：冷。
- `mild`：温和。
- `ocean`：洋。
- `warm`：暖。

例子：

完整写法：`{ "test": "is_temperature_type", "subject": "self", "operator": "equals", "value": "cold" }`

简写：`{ "test": "is_temperature_type", "value": "cold" }`

[size=2]**is_temperature_value

温度值。最冷为 0.0，最热为 1.0。

例子：

完整写法：`{ "test": "is_temperature_value", "subject": "self", "operator": "equals", "value": "0.50" }`

简写：`{ "test": "is_temperature_value", "value": "0.50" }`

[size=2]**is_underground

主体是否在地下。当实体头上有非固体方块时，被认为是在地下。

例子：

完整写法：`{ "test": "is_underground", "subject": "self", "operator": "equals", "value": "true" }`

简写：`{ "test": "is_underground" }`

[size=2]**is_underwater

主体是否在水下。当实体完全泡在水中时，被认为是在水下。

例子：

完整写法：`{ "test": "is_underwater", "subject": "self", "operator": "equals", "value": "true" }`

简写：`{ "test": "is_underwater" }`

[size=2]**is_variant

主体的变种 ID。

例子：

完整写法：`{ "test": "is_variant", "subject": "self", "operator": "equals", "value": "0" }`

简写：`{ "test": "is_variant", "value": "0" }`

[size=2]**moon_intensity

月亮强度。取值应在 [0.00, 1.00] 之中。

例子：

完整写法：`{ "test": "moon_intensity", "subject": "self", "operator": "equals", "value": "0.00" }`

简写：`{ "test": "moon_intensity", "value": "0.00" }`

[size=2]**moon_phase

月相。取值应在 [0, 7] 之中。

例子：

完整写法：`{ "test": "moon_phase", "subject": "self", "operator": "equals", "value": "0" }`

简写：`{ "test": "moon_phase", "value": "0" }`

[size=2]**on_ground

主体是否在地面。

例子：

完整写法：`{ "test": "on_ground", "subject": "self", "operator": "equals", "value": "true" }`

简写：`{ "test": "on_ground" }`

[size=2]**on_ladder

主体是否在梯子上。

例子：

完整写法：`{ "test": "on_ladder", "subject": "self", "operator": "equals", "value": "true" }`

简写：`{ "test": "on_ladder" }`

[/spoiler]

### 测试主体

### 调用：即填写了该滤器的 JSON 文件所属的实体。


- `self`：调用此测试的实体。
- `other`：除调用者以外的，参与交互的实体。
- `parent`：调用者的父母。
- `player`：参与交互的玩家。
- `target`：调用者的目标实体。

### 比较方式


- `==`、`===`、`equals`：等于。当主体的项目与给定数据完全一致时，测试通过。
- `!==`、`<>`、`not`：不等于。当主体的项目与给定数据不一致时，测试通过。
- `<`：小于。当主体的项目小于给定数据时，测试通过。
- `<==`：小于等于。当主体的项目小于或等于给定数据时，测试通过。
- `>`：大于。当主体的项目大于给定数据时，测试通过。
- `>==`：大于等于。当主体的项目大于或等于给定数据时，测试通过。

注意：比较方式中永远不会出现单个的等号 `=`。建议在写滤器时，只要需要写等号就写成两个，即 `==`。

### 组合


- `any_of`：（数组）当其下的任何一个测试通过时，测试通过。
- `all_of`：（数组）当其下的所有测试通过时，条件才能通过。
- *（数组）*：当符合其下的所有条件时，条件才能通过（与 `all_of` 作用相同）。

例如我们可以这么写：

```
{"any_of":[{"test":"has_biome_tag","operator":"==","value":"taiga"},{"test":"has_biome_tag","operator":"!=","value":"mega"}]}
```
当生态群系有 `taiga` 标签**或者**没有 `mega` 标签时，条件达成。

如果我们把 `any_of` 改成 `all_of`：

```
{"all_of":[{"test":"has_biome_tag","operator":"==","value":"taiga"},{"test":"has_biome_tag","operator":"!=","value":"mega"}]}
```
就变成了当生态群系有 `taiga` 标签**并且**没有 `mega` 标签时，条件才能达成。

写成 `all_of` 与直接写成一个数组是等价的：

```
[{"test":"has_biome_tag","operator":"==","value":"taiga"},{"test":"has_biome_tag","operator":"!=","value":"mega"}]
```
`all_of`、`any_of` 以及数组之间可以自由组合和嵌套：

```
{
    "any_of": [
        {
            "all_of": [
                {
                    "test": "has_biome_tag",
                    "operator": "==",
                    "value": "taiga"
                },
                {
                    "test": "has_biome_tag",
                    "operator": "!=",
                    "value": "mega"
                }
            ]
        },
        {
            "test": "is_snow_covered",
            "operator": "==",
            "value": true
        },
        {
            "test": "has_biome_tag",
            "operator": "==",
            "value": "desert"
        }
    ]
}
```

[page]

## 附录：事件

事件可以用于禁用/启用组件组。

### 格式


- `add`：（对象）要启用的所有组件组。

- `component_groups`：（数组）

- *（字符串）一个组件组的名称*

- `remove`：（数组）要禁用的所有组件组。

- `component_groups`：（数组）

- *（字符串）一个组件组的名称*

- `randomize`：（数组）随机执行下面一组。

- *（对象）*

- `weight`：（整数）该组的权重。
- `add`：（数组）要启用的所有组件组。

- `component_groups`：（数组）

- *（字符串）一个组件组的名称*

- `remove`：（数组）要禁用的所有组件组。

- `component_groups`：（数组）

- *（字符串）一个组件组的名称*



- `sequence`：（数组）顺序执行下面每一组。

- *（对象）*

- `filters`：（对象，格式同滤器）执行该组的滤器。
- `add`：（数组）要启用的所有组件组。

- `component_groups`：（数组）
- *（字符   串）一个组件组的名称*
- `remove`：（数组）要禁用的所有组件组。

- `component_groups`：（数组）

- *（字符串）一个组件组的名称*





玩家可以随意定义事件的名称，并在触发器或部分组件的参数中填写事件名，以在特定条件下执行事件。

### 内置事件

Minecraft 为我们提供了几个内置的事件，这些事件不需要我们调用，它们会在某些条件下自动执行。


- `minecraft:entity_spawned`：当实体被生成到世界中时执行。
- `minecraft:entity_born`：当实体由于繁殖而出生时执行。
- `minecraft:entity_transformed`：当实体从一个实体转变为另一个实体时执行。
- `minecraft:on_prime`：当实体被点燃并将要爆炸时执行。

[page]

# 战利品表

战利品表可以用来定义宝箱/实体/游戏行为等各种东西所能给玩家带来的物品，以及指定实体所能穿的装备等。

原版放置战利品表的位置是在行为包根目录下的 `loot_tables` 文件夹内。事实上，你可以放在行为包里任何你想要的地方，但建议大家按传统习惯行事。

在 Java 版中同样也有战利品表，隔壁的教程也是我写的，有兴趣可以对比一下：[Java 版战利品表：从入门到重新入门](https://www.mcbbs.net/thread-831542-1-1.html)。

## 简述

![image.png](https://i.loli.net/2019/10/23/4b2a8QiIMvVWFx6.png)

## 文件格式

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

重载行为包。

可以看到僵尸被烧死后产生了烈焰粉。

![](https://i.loli.net/2018/10/20/5bca0ae304212.gif)

（这张动图是上次写这篇教程时候录的，大体效果没错，就不重新录制了）

好的，一个悲伤的故事是，僵尸就算不是被烧死的也会掉烈焰粉。这并不是我的问题，我已经将该漏洞反馈：[MCPE-54162](https://bugs.mojang.com/browse/MCPE-54162)。

## 课后习题

让玩家钓鱼能钓上来有随机附魔的钻石剑吧！

提示: 可以在原版的行为包里找钓鱼相关战利品表的位置（找不到的话我告诉你，主钓鱼战利品表是 `loot_tables/gameplay/fishing.json`，它又引用了别的战利品表。带有随机附魔的钻石剑可以被认为是宝物，因此你需要修改的是钓鱼出宝物的战利品表，即 `loot_tables/gameplay/fishing/treasure.json`），在本人整理的附录中找到你需要的函数。

本节完，点击论坛右侧的书签可以快速回到顶部。

[page]

## 附录：函数

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

## 附录：条件

当你指定的条件全部达成时，才有可能会使用你所设置的随机池（`pool`）、项（`entry`）或是函数（`function`）。下面是战利品表中可用的全部条件。

### entity_properties

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

### has_mark_variant

检测该实体是否是某个变种。不明。

- `value`：（数字）

示例（`loot_tables/entities/mooshroom_milking.json`）：
```json
{
    "condition": "has_mark_variant",
    "value": -1
}
```

### killed_by_entity

被指定类型的实体击杀时达成条件。

- `entity_type`：（字符串）指定实体的 ID。

示例（`loot_tables/entities/creeper.json`）：
```json
{
    "condition": "killed_by_entity",
    "entity_type": "minecraft:skeleton"
}
```

### killed_by_player

被玩家击杀时达成条件。

示例（`loot_tables/entities/cave_spider.json`）：
```json
{
    "condition": "killed_by_player"
}
```

### killed_by_player_or_pets

被玩家或玩家的宠物击杀时达成条件。

示例（`loot_tables/entities/blaze.json`）：
```json
{
    "condition": "killed_by_player_or_pets"
}
```

### random_chance

有一定几率达成。

- `chance`：（数字）条件达成的几率。取值应当在 [0, 1] 的范围内。

示例（`loot_tables/entities/drowned_equipment.json`）：
```json
{
    "condition": "random_chance",
    "chance": 0.01
}
```

### random_chance_with_looting

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

### random_difficulty_chance

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

### random_regional_difficulty_chance

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

## 文件格式

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

重载行为包，搞定。

![](https://i.loli.net/2018/10/20/5bca1fee0a1b0.png)

## 课后习题

让 shepherd 村民（交易表位于 `trading/shepherd_trades.json`）可以拿羊毛换对应颜色的混凝土！【好无厘头的题目，没办法我只是想讲方块数据值。

提示: 羊毛使用方块数据值来区分颜色。在写 `item` 时，把方块数据值用冒号 `:` 连接在方块 ID 的后面。例如: `minecraft:wool:0`。

[page]

## 附录：函数

在给出交易物品前会执行指定的函数对物品进行操作。下面是交易表中可用的全部函数。

### enchant_book_for_trading

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

### enchant_with_levels

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

### exploration_map

绘制探险地图。

- `destination`：字符串。地图的目的地。需要是一个结构的 ID。全部结构的 ID 可以在 [Wiki](https://minecraft-zh.gamepedia.com/%E5%91%BD%E4%BB%A4/locate) 上查看。

示例（`trading/cartographer_trades.json`）：
```json
{
  "function": "exploration_map",
  "destination": "monument"
}
```

### random_aux_value

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

### random_block_state

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

## 文件格式

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

## 附录：条件

当达成指定条件时，实体才会生成。

特别指明，此处的条件必须要加 `minecraft:` 命名空间前缀，否则不会被读取。

### minecraft:biome_filter

限制允许该实体生成的生态群系。其格式与滤器一致。有关滤器的详细内容，请看「实体」章节的「滤器」部分。

示例（`spawn_rules/bat.json`）：
```json
{
    "minecraft:biome_filter": {
        "test": "has_biome_tag", "operator":"==", "value": "animal"
    }
}
```

### minecraft:brightness_filter

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

### minecraft:delay_filter

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

### minecraft:density_limit

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

### minecraft:difficulty_filter

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

### minecraft:distance_filter

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

### minecraft:height_filter

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

### minecraft:herd

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

### minecraft:mob_event_filter

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

### minecraft:permute_type

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

### minecraft:player_in_village_filter

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

### minecraft:spawn_event

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

### minecraft:spawns_on_block_filter

限制该生物只能生成在指定方块上（该条件应为一个表明方块 ID 的字符串，见下方示例）。

示例（`spawn_rules/chicken.json`）：
```json
{
    "minecraft:spawns_on_block_filter": "minecraft:grass"
}
```

### minecraft:spawns_on_surface

通过添加该对象，实体将可以生成在地表上。如果不添加则不能生成。

示例（`spawn_rules/bee.json`）：
```json
{
    "minecraft:spawns_on_surface": {}
}
```

### minecraft:spawns_underground

通过添加该对象，实体将可以生成在地下。如果不添加则不能生成。

示例（`spawn_rules/bat.json`）：
```json
{
    "minecraft:spawns_underground": {}
}
```

### minecraft:spawns_underwater

通过添加该对象，实体将可以生成在水里。如果不添加则不能生成。

示例（`spawn_rules/cod.json`）：
```json
{
    "minecraft:spawns_underwater": {}
}
```

### minecraft:weight

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

### minecraft:world_age_filter

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

# 物品

行为包可以设置物品的行为。

所有物品的定义文件应该放置在根目录下的 `items` 文件夹内。

## 简述

没有，当你理解了实体以后理解物品没有什么障碍。物品的行为同样是由组件构成的。

## 文件格式

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

重载行为包。

苹果堆叠上限变为 42：  
![42.gif](https://i.loli.net/2019/10/24/Lya2PmFcRfCOkJj.gif)

拿着苹果对沙子顶部右键可以放下 TNT ：  
![image.png](https://i.loli.net/2019/10/24/ALSwTxe4y3jH2lG.png)

## 课后习题

我不知道。改点儿你自己喜欢的物品吧。如果你会做资源包的话，甚至可以自己试试加新物品。

之后章节会把「课后习题」这一部分直接删掉了 <3。

[page]

## 附录：组件

特别指明，此处的组件名必须要加 `minecraft:` 命名空间前缀，否则不会被读取。

### minecraft:block （字符串）

不明。会把该物品显示为指定的方块的模型，但是右键没有效果。

示例（`items/camera.json`）：
```json
{
    "minecraft:block": "minecraft:camera"
}
```

### minecraft:camera

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

### minecraft:foil （布尔）

使该物品自带附魔的光效。

示例（`items/appleEnchanted.json`）：
```json
{
    "minecraft:foil": true
}
```

### minecraft:food

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

### minecraft:hand_equipped （布尔）

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

### minecraft:max_damage （数字）

该物品的总耐久度。当耗损到达这个值以后物品会破碎。

目前似乎并没有应用。

示例（`items/clownfish.json`）：
```json
{
    "minecraft:max_damage": 0
}
```

### minecraft:max_stack_size （数字）

该物品的最大堆叠上限。

设置为大于 64 的数字将能在部分位置突破 64 的上限，不过有些诡异。当堆叠数量超过 99 以后就不会显示具体数字，只会显示为 `99+`。

示例（`items/beetroot_soup.json`）：
```json
{
    "minecraft:max_stack_size": 1
}
```

### minecraft:seed

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

### minecraft:stacked_by_data （布尔）

具有不同数据值的该物品是否不能互相堆叠。

示例（`items/appleEnchanted.json`）：
```json
{
    "minecraft:stacked_by_data": true
}
```

### minecraft:use_duration （数字）

使用该物品所持续的时长。单位为刻。

示例（`items/apple.json`）：
```json
{
    "minecraft:use_duration": 32
}
```

[page]

# 配方

行为包可以修改各种配方，包括有序合成配方、无序合成配方（工作台、制图台、切石机）、熔炼配方（熔炉、烟熏炉、高炉、营火）、混合酿造配方、换容酿造配方等。

[page]

## 有序合成配方

### 简述

有序合成，指的是材料必须在工作台内按照一定顺序摆放才能获得输出物品的合成方式。

### 文件结构

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

### 学习原版

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

### 自造轮子

这回我们不覆盖原有的东西了。我们直接加新的有序合成配方！

目标：用草和土块能够合成出草方块。

> 基岩版中，草（不管是一格高的还是两格高的）都是 `tall_grass`，草方块叫做 `grass`，挺反人类的。

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

重载行为包。

游戏会自动读取 `recipes` 目录下的所有配方，因此我们不需要在别的什么文件里面设置就可以直接在工作台里使用这个配方了！这一点和前面的战利品表、交易表、生成规则等不同。

> 题外话：事实上实体和物品也是会自动读取的，不过因为它们还需要配合资源包才能有该有的效果，而资源包又不属于本教程的范畴，因此我没有在前文提及。

![image.png](https://i.loli.net/2019/10/24/JIKv84ZuGaT1EPO.png)

[page]

## 无序合成配方

### 简述

无序合成，指的是只要材料的种类、数目够了，就可以获得输出物品的配方。

### 文件结构

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

### 学习原版

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

### 自造轮子

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

重载行为包。

![image.png](https://i.loli.net/2019/10/24/MHLE9GJoNi2TPF4.png)

[page]

## 熔炼配方

### 简述

熔炼配方，指的是放到炉子（广义，包含熔炉、高炉、烟熏炉、营火）里面能得到物品的配方。

### 文件结构

- `format_version`：（字符串）文件格式版本。本帖一律写为 `1.14.0`。
- `minecraft:recipe_furnace`：（对象）表明该文件是一个熔炼配方。
    - `description`：（对象）描述。
        - `identifier`：（字符串）该配方的 ID。
    - `group`：（字符串）该配方所属的分组。分组可以是任意字符串，具有相同分组的配方会在配方书中被显示在一起。
    - `tags`：（字符串数组）该配方可以用于的位置。位置可以是 `furnace`（熔炉）、`blast_furnace`（高炉）、`smoker`（烟熏炉）或 `campfire`（营火）。
    - `input`：（字符串）一个物品 ID。该熔炼配方的输入物品。
    - `output`：（字符串）一个物品 ID。该熔炼配方的输出物品。

### 学习原版

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

### 自造轮子

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

重载行为包。

![image.png](https://i.loli.net/2019/10/24/oLYAHysl6umgbpq.png)

[page]

## 混合酿造配方

### 简述

混合酿造配方，指的是将一个具有某种状态效果的药水放入酿造台后，再加入某个物品，可以酿造出具有另一种状态效果的药水的配方。

### 文件结构

- `format_version`：（字符串）文件格式版本。本帖一律写为 `1.14.0`。
- `minecraft:recipe_brewing_mix`：（对象）表明该文件是一个混合酿造配方。
    - `description`：（对象）描述。
        - `identifier`：（字符串）该配方的 ID。
    - `group`：（字符串）该配方所属的分组。分组可以是任意字符串，具有相同分组的配方会在配方书中被显示在一起。
    - `tags`：（字符串数组）该配方可以用于的位置。位置只能是 `brewing_stand`（酿造台）。
    - `input`：（字符串）一个形如 `minecraft:potion_type:<药水类型>` 的药水类型 ID。该配方接受的药水类型。
    - `reagent`：（字符串）一个物品 ID。进行该酿造所需要添加的额外物品。
    - `output`：（字符串）一个形如 `minecraft:potion_type:<药水类型>` 的药水类型 ID。该配方的输出的药水类型。

### 学习原版

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

### 自造轮子

不造了，酿造没意思。

[page]

## 换容酿造配方

### 简述

换容酿造配方，指的是一瓶药水被酿造后将会改变容器的配方。例如从普通玻璃瓶转换为喷溅型玻璃瓶或滞留型玻璃瓶等。

### 文件结构

- `format_version`：（字符串）文件格式版本。本帖一律写为 `1.14.0`。
- `minecraft:recipe_brewing_container`：（对象）表明该文件是一个换容酿造配方。
    - `description`：（对象）描述。
        - `identifier`：（字符串）该配方的 ID。
    - `group`：（字符串）该配方所属的分组。分组可以是任意字符串，具有相同分组的配方会在配方书中被显示在一起。
    - `tags`：（字符串数组）该配方可以用于的位置。位置只能是 `brewing_stand`（酿造台）。
    - `input`：（字符串）一个物品 ID。该配方接受的药水瓶的 ID。可选：`minecraft:potion`（普通玻璃瓶）、`minecraft:splash_potion`（喷溅型）、`minecraft:lingering_potion`（滞留型）。
    - `reagent`：（字符串）一个物品 ID。进行该酿造所需要添加的额外物品。
    - `output`：（字符串）一个物品 ID。该配方输出的药水瓶的 ID。可选：`minecraft:potion`（普通玻璃瓶）、`minecraft:splash_potion`（喷溅型）、`minecraft:lingering_potion`（滞留型）。

### 学习原版

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

### 自造轮子

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

重载行为包。

好，遗憾的是，并没有用。酿造台的那几个格子所能接受的物品是写死的，即使你在配方中写了 `tallgrass`，草也照样放不进去。这一目标以失败告终。

由此可见，酿造相关的配方其实可操作性并不强。它既不可以添加原版没有的药水，也不能让原版不支持的物品参与到酿造当中。

[page]

# 生物群系

[page]

# 结构

[page]

# 方块

[page]

# 函数

[page]

# 脚本

这部分难度系数飙升，我先咕几个月。如果你不熟悉编程以及 JavaScript 的话，不要指望将来等我更了这部分你能看得懂……

[page]

# 发布

进入 Minecraft 放置行为包的文件夹：`%localappdata%\Packages\Microsoft.MinecraftUWP_8wekyb3d8bbwe\LocalState\games\com.mojang\behavior_packs`，再进入你创建的文件夹（本教程以 `spg_best` 为例），然后全选所有文件并压缩为 zip。将压缩包的后缀 `.zip` 重命名为 `.mcpack`，这个 `*.mcpack` 文件就可以拿去发布了。玩家只需双击这个文件 Minecraft 就会自动完成导入。

[page]

# 参考资料

- [wiki - Add-on](https://minecraft.gamepedia.com/Add-on)
- [Skylinerw - Loot Tables](https://github.com/skylinerw/guides/blob/master/bedrock/loot%20tables.md)
- [ruhuasiyu - 【1.13】数据包(原版模组)入门教程](http://www.mcbbs.net/thread-784662-1-1.html) 【我很认真

# 致谢

- [MM2BC](http://www.mcbbs.net/thread-779668-1-1.html)：本文的命可以说是它给的了。

[page]

# 后语

看到大家都来这里水三个命令方块的东西然后拿走基岩版爱好者勋章，我也想来拿。但是水命令方块的东西太没有水平了，我当然要水些别的。于是，断断续续脱了一年多，这篇不知道是教程还是水文的东西就这么出来了，让您见笑了。

基岩版的 Add-on 是十分强大的一个功能，作为一名 Java 版的忠实玩家其实是非常欣赏 Add-on 的这种开放性的。当然，我不会转行，Java 版仍然是我的家。不过我也迫切地希望能够在论坛看到优秀的原创 Add-on 作品。各位 dalaoes 不要吝啬自己的才华！

教程中有部分内容没有官方文档依据，仅靠测试、查看原版行为包文件以及在游戏内进行测试推断而出，如有错误请务必指正。另外还有一部分我本人推断不出的内容，在网上查询资料未果，在教程内同一标注为「不明」，希望有了解这部分内容的玩家不吝赐教！

后续还会视情况发布有关资源包的教程，加入动画、粒子效果、UI 界面等内容。将行为包与资源包结合到一起以后，便可自行任意添加新方块、物品、实体等，无人能当，十分愉快。
