【CBL|SPG】[1.14] NBT 路径：从入门到胡了

# 前言

![知识共享许可协议](https://i.creativecommons.org/l/by-nc-sa/4.0/88x31.png)

本教程全文以 [知识共享署名-非商业性使用-相同方式共享 4.0 国际许可协议](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)发布，受孤星共和国法律保护，面向全体华人玩家服务。小部分翻译或引用自 [Minecraft Wiki](https://minecraft.gamepedia.com) 与 [Minecraft 中文 Wiki](https://minecraft-zh.gamepedia.com) 的内容均已标记。

本教程将着重介绍 NBT 路径的概念、语法与相关应用。我们假设您已经完全掌握了[命令进阶中对 NBT 的讲解](https://mc-command.oschina.io/command-tutorial/output/common-format/nbt/nbt.html)（尽管命令进阶成文时间较早，这一部分内容仍没有过时）。如果您在下文中遇到不熟悉并且没有进行讲解的专有名词，请先阅读命令进阶。

由于作者才疏学浅，难免会有很多疏漏，请细心的读者们不吝鞭挞♡

阅读本文将消耗约半个小时。

## 译名表

| 英文 | 本文采用译名 | 备注 |
| - | - | - |
| Array | 数组 | 与列表（List）的区别在于数组有序。 |
| Chat Component / Raw JSON text | JSON 文本 | - |
| Compound | 复合标签 | - |
| Index | 索引 | - |
| Integer | 整型 | - |
| Item | 项 | - |
| Key | 键 | - |
| List | 列表 | 与数组（Array）的区别在于列表无序。 |
| Resource Location | 资源路径 | 函数、进度、战利品表等数据的形如 `<命名空间>:<目录>/<文件名>` 的标识符。 |
| Match | 匹配 | - |
| Namespace | 命名空间 | - |
| NBT | NBT | - |
| NBT Path | NBT 路径 | - |
| Value | 值 | - |

# 胡诌概念

## NBT 路径

**NBT 路径**，是命令中的一种参数。在 Wiki 或是游戏内 `/help` 显示的帮助信息中，常常使用 `<path>` 或 `<路径>` 来表示 NBT 路径。为了行文简洁，后续使用「**路径**」来简称该参数。

路径可以被用于筛选出指定 NBT 值。通俗地说，就是当我们有一个 NBT 复合标签、列表或数组时，能够根据一个路径来得到 0 个、1 个或多个值。打一个并不通俗易懂的比方：我们可以用「灯塔国哥谭市幸福路 42 号楼 450 室」来表示一个（或零个、几个）人的住址。类似地，我们可以用形如 `foo.bar.baz[42][450]` 的**路径**表示一个（或零个、几个）NBT 值的位置。

## 匹配

**匹配**，是一种比较两个 NBT 值的方法，它与**相等**是不同的两个概念。

对于任意的两个 NBT 值 A 和 B：

- 如果 A 的类型和 B 的类型不同，它们不匹配。
- 如果 A、B 均为列表，当 B 中的每一项都能在 A 中找到与之匹配的一项时，A 匹配 B。特别地，如果 B 为 `[]`，只有在 A 也为 `[]` 时 A 匹配 B。
- 如果 A、B 均为复合标签，当 B 中所有键都存在于 A 中，并且同名的键所对应的值匹配时，A 匹配 B。
- 如果 A、B 均为数字、数组等，**匹配**与**相等**是同一效果。

| A | B | A 匹配 B |
| - | - | - |
| `0` | `0b` | 否 |
| `0b` | `1b` | 否 |
| `0b` | `0b` | 是 |
| `[I;0,1,2]` | `[I;2,1,0]` | 否 |
| `[I;0,1,2]` | `[I;0,1]` | 否 |
| `[I;0,1,2]` | `[I;0,1,2]` | 是 |
| `[0,1,2]` | `[]` | 否 |
| `[0,1,2]` | `[0,1,2,3]` | 否 |
| `[0,1,2]` | `[2,1,0]` | 是 |
| `[0,1,2]` | `[2,2,2,2]` | 是 |
| `[0,1,2]` | `[0,1,2]` | 是 |
| `{foo:1,bar:0b}` | `{bar:1b}` | 否 |
| `{foo:1,bar:0b}` | `{qux:450}` | 否 |
| `{foo:1,bar:0b}` | `{}` | 是 |
| `{foo:1,bar:0b}` | `{bar:0b}` | 是 |

# 胡蒙语法

路径真的超容易写的【跑

## 复合标签内的路径

### 是键！

能够返回复合标签中某一值的路径，应为这个值所对应的键。以下面的复合标签为例：

```
{
    foo: {
        bar: 1b,
        qux: {
            duang: 'qwq',
            qwq: 'duang'
        }
    },
    bar: 42b,
    '要加"引号"的键': 233
}
```

> 关于引号：
> 
> 在 NBT 中，除了英文字母（`a-z``A-Z`）、数字（`0-9`）、少数符号（`-`、`+`、`.`、`_`）以外的字符串都需要被引号（`'` 或 `"`）包裹。
> 需要注意的是，尽管在 NBT 中可以使用单引号（`'`）包裹作为键的字符串，路径中是**不能**用单引号（`'`）包裹键的，这可能是一个 bug。

| 路径 | 返回 |
| - | - |
| `foo` | `{ bar: 1b, qux: 0b }` |
| `bar` | `42b` |
| `"要加\"引号\"的键"` | `233` |
| `'要加"引号"的键'` | 什么也不返回，可能是个 bug |
| `asdf` | 什么也不返回，因为并没有这个键 |

### 筛选

如果路径中的键所对应的值是一个复合标签，则可以在键后面添加一对大括号（`{}`），并在大括号中填写筛选条件。
只有在该复合标签能够匹配大括号中的筛选条件时，该路径才能返回该复合标签。

| 路径 | 返回 |
| - | - |
| `foo{}` | `{bar:1b,qux:{duang:'qwq',qwq:'duang'}}` |
| `foo{bar:1b}` | `{bar:1b,qux:{duang:'qwq',qwq:'duang'}}` |
| `foo{qux:{duang:'qwq'}}` | `{bar:1b,qux:{duang:'qwq',qwq:'duang'}}` |
| `foo{bar:0b}` | 什么也不返回 |
| `foo{asdf:233}` | 什么也不返回 |

### 根标签

使用一对闭合的大括号 `{}` 能够返回整个根标签。

- 路径 `{}` 返回例子所给的这一大坨：
```
{
    foo: {
        bar: 1b,
        qux: {
            duang: 'qwq',
            qwq: 'duang'
        }
    },
    bar: 42b,
    '要加"引号"的键': 233
}
```

## 数组或列表中的路径

### 是索引！

能够返回数组或列表中某一项的路径，应为放到半角中括号 （`[` 和  `]`）内的这一项所对应的索引。以下面的整型数组为例：

```
[I;
    1,
    2
]
```

索引是从 `0` 开始计数的：

| 路径 | 返回 |
| - | - |
| `[0]` | `1` |
| `[1]` | `2` |
| `[2]` | 什么也不返回 |

### 倒着数

索引如果是负数，将返回该数组/列表中的倒数的指定项：

| 路径 | 返回 |
| - | - |
| `[-1]` | 倒数第一项，即 `2` |
| `[-2]` | 倒数第二项，即 `1` |
| `[-3]` | 什么也不返回 |

### 全选

如果不写索引，只写中括号，将返回数组中的所有项：
| 路径 | 返回 |
| - | - |
| `[]` | `1` 和 `2` |

### 筛选

如果该列表是一个由复合标签构成的列表，中括号中可以添加筛选条件。以下面的列表为例：

```
[
    {
        foo: {
            bar: 1b,
            baz: 2b
        },
        qux: 3b
    }
]
```

| 路径 | 返回 |
| - | - |
| `[]` | `{foo:{bar:1b,baz:2b},qux:3b}` |
| `[{}]` | `{foo:{bar:1b,baz:2b},qux:3b}` |
| `[{qux:3b}]` | `{foo:{bar:1b,baz:2b},qux:3b}` |
| `[{foo:{bar:1b}}]` | `{foo:{bar:1b,baz:2b},qux:3b}` |
| `[{qux:3}]` | 什么也不返回 |
| `[{foo:{asdf:1b}}]` | 什么也不返回 |

## 嵌套路径

当一个路径返回的值是复合标签、列表或数组等可以继续使用路径的值时，在原先路径的基础上加一个点（`.`）就可以继续书写下一级的路径。以下面这个复杂的实体复合标签为例：

```
{
    Brain: {
        memories: {
            "minecraft:home": "孤儿"
        }
    }, 
    HurtByTimestamp: 0, 
    Attributes: [
        {Base: 20.0d, Name: "generic.maxHealth"}, 
        {Base: 0.0d, Name: "generic.knockbackResistance"}
    ], 
    Invulnerable: 0b, 
    Pose: {}, 
    Rotation: [0.0f, 0.0f], 
    HandItems: [
        {
            id: "minecraft:stone",
            Count: 1b,
            tag: {
                display: {
                    Name: '{"text":"是石头 awsl"}'
                }
            }
        }, 
        {}
    ]
}
```

> 为了看起来更像一些编程语言的写法，左中括号（`[`）前连接的点（`.`）可以省略，并且推荐省略。

| 路径 | 返回 |
| - | - |
| `{}` | 根标签 |
| `Brain` | `{memories:{"minecraft:home":"孤儿"}}` |
| `Brain.memories` | `{"minecraft:home":"孤儿"}` |
| `Brain.memories."minecraft:home"` | `"孤儿"` |
| `Attributes` | `[{Base:20.0d,Name:"generic.maxHealth"},{Base:0.0d,Name:"generic.knockbackResistance"}]` |
| `Attributes.[0]` | `{Base:20.0d,Name:"generic.maxHealth"}` |
| `Attributes[0]` | `{Base:20.0d,Name:"generic.maxHealth"}` |
| `Attributes[0].Base` | `20.0d` |
| `Attributes[]` | `{Base:20.0d,Name:"generic.maxHealth"}` 和 `{Base:0.0d,Name:"generic.knockbackResistance"}` |
| `Attributes[].Name` | `"generic.maxHealth"` 和 `"generic.knockbackResistance"` |
| `Attributes[{Base:0.0d}].Name` | `"generic.knockbackResistance"` |

# 胡编应用

## 获取类

**获取类**运用，即根据路径来获得 NBT 值，并对该值进行诸如传递、显示等操作，不对其进行任何修改。

### data get

> `data get block <坐标> [<路径>] [<倍率>]`
>
> `data get entity <实体> [<路径>] [<倍率>]`
>
> 来自 [中文 Wiki](https://minecraft-zh.gamepedia.com/%E5%91%BD%E4%BB%A4/data)

data get 命令可以用于获取指定目标（方块实体或实体）的指定路径对应的 NBT 值。

### execute if data

### JSON 文本


> `{ "nbt": "<路径>", "block": "<X> <Y> <Z>" }`
>
> `{ "nbt": "<路径>", "entity": "<实体>" }`

## 赋值类

**赋值类**运用，是

# 不知所言



## 特别鸣谢

- Discord 用户 @Energyxxer#4393：提供「根标签操作」有关解释

[afd]SPGoding[/afd]
