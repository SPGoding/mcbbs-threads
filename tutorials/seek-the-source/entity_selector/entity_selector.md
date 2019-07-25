[1.14.4] 追根溯「源」——实体选择器

# 追根溯「源」

**追根溯「源」**，是从源代码角度对部分命令机制的漫谈系列。也许在骂着 Mojang SB 的间隙，偶尔看看他们是怎么犯下的蠢，能帮你更好地驾驭命令？谁知道呢。

本系列漫谈基于对 Minecraft Java Edition 1.14.4 的反编译与反混淆，十分感谢 [FabricMC](https://github.com/FabricMC) 提供的散发着自由气息的 [yarn](https://github.com/FabricMC/yarn) 项目，梓榆谨代表自己向所有该项目的贡献者致以最崇高的敬意。三鞠躬。由于散步商业闭源软件的源代码属于违法行为，本帖中只给出极少数代码片段，仅供学习交流使用。当然，因为名字都是人起的，为了方便，本人对部分映射名进行了修改。

阅读本系列可能需要具备一定的英语水平，或是具备查阅字典的能力，并不需要有多么高深的编程水平。毕竟，笔者压根就不会编程。

# 实体选择器

**实体选择器（Entity selector）**，也叫**目标选择器（target selector）**，是一种用于选择实体的命令参数。你可以在 [Wiki](https://minecraft.gamepedia.com/Commands#Target_selectors_in_Java_Edition)（或[中文 Wiki](https://minecraft-zh.gamepedia.com/%E5%91%BD%E4%BB%A4#.E7.9B.AE.E6.A0.87.E9.80.89.E6.8B.A9.E5.99.A8)）中阅读相关信息。

实体选择器的相关代码放置在 `net.minecraft.command.EntitySelector` 类中。我们可以看到其定义了如下的字段：

![fileds.png](https://s2.ax1x.com/2019/07/25/eZ4zxU.png)

可以发现，它们和实体选择器的参数并不是一一对应的关系。那么是怎样的关系呢？这需要先从 `net.minecraft.command.EntitySelectorReader` 类讲起。

## 解析

`EntitySelectorReader` 可以解析玩家输入的字符串形式的实体选择器。我们简单来看一下它的流程：

0. 判断是 UUID、玩家名，还是一个实体选择器。
    - 若第一个字符<sup>[1]</sup>不是 `@`，则为一个 UUID 或玩家名。
    - 若第一个字符<sup>[1]</sup>是 `@`，则为一个实体选择器。

**对于 UUID 或玩家名的处理流程：**

0. 尝试按照 UUID 解析。如果解析成功，则设定：
    | 变量 | 值 | 备注 |
    | - | - | - |
    | `uuid` | - | UUID。 |
    | `includingNonPlayer` | `true` | **允许包含非玩家**。 |
    | `limit` | `1` | 相当于我们在选择器参数里写的 `limit=1`。 |
1. 解析失败，说明它不是 UUID，而是一个玩家名。设定：
    | 变量 | 值 | 备注 |
    | - | - | - |
    | `playerName` | - | 玩家名。 |
    | `includingNonPlayer` | `false` | **不允许包含非玩家**。 |
    | `limit` | `1` | 相当于我们在选择器参数里写的 `limit=1`。 |

**对于实体选择器的处理流程：**

0. 解析实体选择器变量：
    - `@p`: 
        | 变量 | 值 | 备注 |
        | - | - | - |
        | `includingNonPlayer` | `false` | **不允许包含非玩家**。 |
        | `limit` | `1` | 相当于我们在选择器参数里写的 `limit=1` |
        | `sorter` | `NEAREST_FIRST` | 相当于我们在选择器参数里写的 `sort=nearest` |
        | `entityType` | `EntityType.PLAYER` | 相当于我们在选择器参数里写的 `type=player` |
    - `@a`: 
        | 变量 | 值 | 备注 |
        | - | - | - |
        | `includingNonPlayer` | `false` | **不允许包含非玩家**。 |
        | `limit` | `2147483647` | 相当于我们在选择器参数里写的 `limit=2147483647`。从这里可以看出，**选择器选择的数量默认有上限的，一般达不到就是了。** |
        | `sorter` | `ARBITRARY` | 相当于我们在选择器参数里写的 `sort=arbitrary` |
        | `entityType` | `EntityType.PLAYER` | 相当于我们在选择器参数里写的 `type=player` |
    - `@r`: 
        | 变量 | 值 | 备注 |
        | - | - | - |
        | `includingNonPlayer` | `false` | **不允许包含非玩家**。 |
        | `limit` | `1` | 相当于我们在选择器参数里写的 `limit=1`。 |
        | `sorter` | `RANDOM` | 相当于我们在选择器参数里写的 `sort=random` |
        | `entityType` | `EntityType.PLAYER` | 相当于我们在选择器参数里写的 `type=player` |
    - `@s`: 
        | 变量 | 值 | 备注 |
        | - | - | - |
        | `includingNonPlayer` | `true` | **允许包含非玩家**。 |
        | `senderOnly` | `EntityType.PLAYER` | **只选择执行者**。 |
        | `limit` | `1` | 相当于我们在选择器参数里写的 `limit=1`。 |
    - `@e`: 
        | 变量 | 值 | 备注 |
        | - | - | - |
        | `includingNonPlayer` | `true` | **允许包含非玩家**。 |
        | `predicate` | `Entity::isAlive` | **只选择活着的实体**。 |
        | `limit` | `2147483647` | 相当于我们在选择器参数里写的 `limit=2147483647`。 |
        | `sorter` | `ARBITRARY` | 相当于我们在选择器参数里写的 `sort=arbitrary` |
        | `entityType` | `EntityType.PLAYER` | 相当于我们在选择器参数里写的 `type=player` |
1. 读取选择器参数。针对不同的选择器参数设定不同的 `Predicate`。将在下一部分详细讲解。

从上面的流程描述中我们可以发现，除了我们熟悉的 `limit`、`sort`、`type` 等参数外，还有着 `includingNonPlayer`、`predicate`、`senderOnly` 这些陌生的东西。它们是什么？有什么用？作为 CBer 的我们该如何利用？且看下一部分。

---

**注**：
1. 其实「下一个字符」比「第一个字符」更加准确，但这么描述就涉及到了整个命令解析的技术细节，比较繁琐，所以简化了描述。
2. 以上描述省略了各种异常处理。

## 获取

本部分将讲述实体选择器获取实体的工作流程。

获取实体的主要行为定义在函数 `getEntities()` 中。该函数有一个参数，代表命令的执行者（执行者不一定是实体，也可以是方块、命令函数、控制台等）。当执行该函数时：

0. 检查命令的执行者是否有足够权限（2 级权限等级）。
1.  - 如果不允许包含非玩家（`!includeNonPlayers`）
    - 

## 总结

## 你知道吗

本部分是本人在分析反编译后的源代码中发现的一些有趣的事情。

0. 在判断实体到指定坐标的距离是否满足选择器参数 `distance` 中规定的范围时，Mojang 进行比较的是距离的平方，省去了很多次开根运算；
1. 对字符串进行的 `switch` 编译以后会变成对 `hashCode()` 的 `switch`。这是因为 `switch` 其实只能对数字进行，对字符串的支持是在 Java7 加入的补救措施。
