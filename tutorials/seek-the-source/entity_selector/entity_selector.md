[1.14.4] 追根溯「源」——实体选择器

# 追根溯「源」

**追根溯「源」**，是从源代码角度对部分命令机制的漫谈系列。也许在骂着 Mojang SB 的间隙，偶尔看看他们是怎么犯下的蠢，能帮你更好地驾驭命令？谁知道呢。

本系列漫谈基于对 Minecraft Java Edition 1.14.4 的反编译与反混淆，十分感谢 [FabricMC](https://github.com/FabricMC) 提供的散发着自由气息的 [yarn](https://github.com/FabricMC/yarn) 项目，梓榆谨代表自己向所有该项目的贡献者致以最崇高的敬意。三鞠躬。由于散步商业闭源软件的源代码属于违法行为，本帖中只给出极少数代码片段，仅供学习交流使用。当然，因为名字都是人起的，为了方便，本人对部分映射名进行了修改。

阅读本系列可能需要具备一定的英语水平，或是具备查阅字典的能力，并不需要有多么高深的编程水平。毕竟，笔者压根就不会编程。

# 实体选择器

**实体选择器（entity selector）**，也叫**目标选择器（target selector）**，是一种用于选择实体的命令参数。你可以在 [Wiki](https://minecraft.gamepedia.com/Commands#Target_selectors_in_Java_Edition)（或[中文 Wiki](https://minecraft-zh.gamepedia.com/%E5%91%BD%E4%BB%A4#.E7.9B.AE.E6.A0.87.E9.80.89.E6.8B.A9.E5.99.A8)）中阅读相关信息。

实体选择器的相关代码放置在 `net.minecraft.command.EntitySelector` 类中。我们可以看到其定义了如下的字段：

![fileds.png](https://s2.ax1x.com/2019/07/25/eZ4zxU.png)

可以发现，它们和实体选择器的参数并不是一一对应的关系。那么是怎样的关系呢？这需要先从 `net.minecraft.command.EntitySelectorReader` 类讲起。

## 解析

`EntitySelectorReader` 可以解析玩家输入的字符串形式的实体选择器。我们简单来看一下它的流程：

判断输入字符串是 UUID、玩家名，还是一个实体选择器：
- 若第一个字符<sup>[1]</sup>不是 `@`，则为一个 UUID 或玩家名；
- 若第一个字符<sup>[1]</sup>是 `@`，则为一个实体选择器。

### 对于 UUID 或玩家名的处理流程

#### 尝试按照 UUID 解析。如果解析成功，则设定：

| 字段 | 值 | 备注 |
| - | - | - |
| `includingNonPlayer` | `true` | **允许包含非玩家**。 |
| `uuid` | - | UUID。 |
| `limit` | `1` | 相当于我们在选择器参数里写的 `limit=1`。 |

#### 解析失败，说明它不是 UUID，而是一个玩家名。设定：

| 字段 | 值 | 备注 |
| - | - | - |
| `includingNonPlayer` | `false` | **不允许包含非玩家**。 |
| `playerName` | - | 玩家名。 |
| `limit` | `1` | 相当于我们在选择器参数里写的 `limit=1`。 |

### 对于实体选择器的处理流程

#### 解析实体选择器变量

在这一步中，将根据不同的实体选择器变量，来设定实体选择器的参数。

##### `@p`

| 字段 | 值 | 备注 |
| - | - | - |
| `includingNonPlayer` | `false` | **不允许包含非玩家**。 |
| `limit` | `1` | 相当于我们在选择器参数里写的 `limit=1` |
| `sorter` | `NEAREST` | 相当于我们在选择器参数里写的 `sort=nearest` |
| `entityType` | `EntityType.PLAYER` | 相当于我们在选择器参数里写的 `type=player` |

##### `@a`

| 字段 | 值 | 备注 |
| - | - | - |
| `includingNonPlayer` | `false` | **不允许包含非玩家**。 |
| `limit` | `2147483647` | 相当于我们在选择器参数里写的 `limit=2147483647`。从这里可以看出，**选择器选择的数量默认有上限的，一般达不到就是了。** |
| `sorter` | `ARBITRARY` | 相当于我们在选择器参数里写的 `sort=arbitrary` |
| `entityType` | `EntityType.PLAYER` | 相当于我们在选择器参数里写的 `type=player` |

##### `@r`

| 字段 | 值 | 备注 |
| - | - | - |
| `includingNonPlayer` | `false` | **不允许包含非玩家**。 |
| `limit` | `1` | 相当于我们在选择器参数里写的 `limit=1`。 |
| `sorter` | `RANDOM` | 相当于我们在选择器参数里写的 `sort=random` |
| `entityType` | `EntityType.PLAYER` | 相当于我们在选择器参数里写的 `type=player` |

##### `@s`

| 字段 | 值 | 备注 |
| - | - | - |
| `includingNonPlayer` | `true` | **允许包含非玩家**。 |
| `senderOnly` | `true` | **只选择执行者**。 |
| `limit` | `1` | 相当于我们在选择器参数里写的 `limit=1`。 |

##### `@e`

| 字段 | 值 | 备注 |
| - | - | - |
| `includingNonPlayer` | `true` | **允许包含非玩家**。 |
| `predicate` | `Entity::isAlive` | **筛选出活着的实体**。 |
| `limit` | `2147483647` | 相当于我们在选择器参数里写的 `limit=2147483647`。 |
| `sorter` | `ARBITRARY` | 相当于我们在选择器参数里写的 `sort=arbitrary` |
| `entityType` | `EntityType.PLAYER` | 相当于我们在选择器参数里写的 `type=player` |

#### 读取选择器参数

在这一步中，将根据不同的选择器参数，来设定实体选择器的字段。本部分中使用 `%值%` 来简单表示玩家指定的值，实际上整个过程要复杂不少。

##### `name=%值%`

| 字段 | 值 | 备注 |
| - | - | - |
| `predicate` | `entity.getName().asString().equals(%值%) != isNegation` | 判断实体名是否满足指定条件。 |

注： `predicate` 中调用的 `asString()` 很有意思。我们可以把它返回的内容大体理解为显示出来的文本内容（其实完全不是），因此你使用 `/summon xxx ~ ~ ~ {CustomName:'{"text":"haha","color":"red"}'}` 命令生成的生物可以被 `@e[name=haha]` 选中。可能以后本系列会对 JSON 原始文本进行详细的讲解。

##### `distance=%值%`

| 字段 | 值 | 备注 |
| - | - | - |
| `localWorldOnly` | `true` | **只获取当前世界中的实体**。 |
| `distance` | `%值%` | - |

##### `level=%值%`

| 字段 | 值 | 备注 |
| - | - | - |
| `includingNonPlayer` | `false` | **不允许包含非玩家**。 |
| `levelRange` | `%值%` | - |

##### `x=%值%`

| 字段 | 值 | 备注 |
| - | - | - |
| `localWorldOnly` | `true` | **只获取当前世界中的实体**。 |
| `offsetX` | `%值%` | - |

##### `y=%值%`

| 字段 | 值 | 备注 |
| - | - | - |
| `localWorldOnly` | `true` | **只获取当前世界中的实体**。 |
| `offsetY` | `%值%` | - |

##### `z=%值%`

| 字段 | 值 | 备注 |
| - | - | - |
| `localWorldOnly` | `true` | **只获取当前世界中的实体**。 |
| `offsetZ` | `%值%` | - |

##### `dx=%值%`

| 字段 | 值 | 备注 |
| - | - | - |
| `localWorldOnly` | `true` | **只获取当前世界中的实体**。 |
| `boxX` | `%值%` | - |

##### `dy=%值%`

| 字段 | 值 | 备注 |
| - | - | - |
| `localWorldOnly` | `true` | **只获取当前世界中的实体**。 |
| `boxY` | `%值%` | - |

##### `dz=%值%`

| 字段 | 值 | 备注 |
| - | - | - |
| `localWorldOnly` | `true` | **只获取当前世界中的实体**。 |
| `boxZ` | `%值%` | - |

##### `x_rotation=%值%`

| 字段 | 值 | 备注 |
| - | - | - |
| `pitchRange` | `%值%` | - |

##### `y_rotation=%值%`

| 字段 | 值 | 备注 |
| - | - | - |
| `yawRange` | `%值%` | - |

##### `limit=%值%`

| 字段 | 值 | 备注 |
| - | - | - |
| `limit` | `%值%` | - |

##### `sort=%值%`

| 字段 | 值 | 备注 |
| - | - | - |
| `sorter` | `%值%` | - |

##### `gamemode=%值%`

| 字段 | 值 | 备注 |
| - | - | - |
| `includingNonPlayer` | `false` | **不允许包含非玩家**。 |
| `predicate` | `isNegation ? mode != %值% : mode == %值%` | 判断玩家的游戏模式是否满足指定条件。 |

##### `team=%值%`

| 字段 | 值 | 备注 |
| - | - | - |
| `predicate` | `teamName.equals(%值%) != isNegation` | 判断实体所处的队伍是否满足指定条件。 |

##### `type=%值%`

| 字段 | 值 | 备注 |
| - | - | - |
| `includingNonPlayer` | 某些情况下设置为 `false` | 当 `%值%` 为 `minecraft:player` 并且没有感叹号 `!` 时设置为**不允许包含非玩家**。 |
| `predicate` | 略 | 判断实体的类型是否满足指定条件。 |

##### `tag=%值%`

| 字段 | 值 | 备注 |
| - | - | - |
| `predicate` | 略 | 判断实体的标签是否满足指定条件。 |

注：设置 `predicate` 的函数中有判断 `%值%` 是否为空字符串的部分，如果为空，则会设置 `predicate` 为 `entity.getScoreboardTags().isEmpty() != isNegation`。**因此我们可以使用 `@e[tag=]` 来选择没有标签的实体，用 `@e[tag=!]` 来选择有任意标签的实体**。*我也真佩服 Mojang 绕得出这个逻辑*。

##### `nbt=%值%`

| 字段 | 值 | 备注 |
| - | - | - |
| `predicate` | `TagHelper.areTagsEqual(%值%, tag, true) != isNegation` | 判断实体的类型是否满足指定条件。 |

##### `scores=%值%`

| 字段 | 值 | 备注 |
| - | - | - |
| `predicate` | 略 | 判断实体的分数是否满足指定条件。 |

##### `advancements=%值%`

| 字段 | 值 | 备注 |
| - | - | - |
| `includingNonPlayer` | `false` | **不允许包含非玩家**。 |
| `predicate` | 略 | 判断实体的分数是否满足指定条件。 |

### 小结

> 停一停，我都忘了这部分标题叫什么了！

这一部分的标题是「解析」，讲述的是游戏如何将玩家输入的字符串解析为实体选择器。游戏会根据不同的实体选择器变量、实体选择器参数，来不断调整各种字段的值。

从上面的字段表中我们可以发现，除了我们熟悉的 `limit`、`sorter`、`type` 等字段对应着实体选择器的各种参数外，还有 `includingNonPlayer`、`predicate`、`senderOnly`、`localWorldOnly`、`playerName`、`uuid` 这六个陌生的东西。它们是什么？有什么用？作为 CBer 的我们该如何利用？且看下一部分。

---

**注**：
1. 其实「下一个字符」比「第一个字符」更加准确，但这么描述就涉及到了整个命令解析的技术细节，比较繁琐，所以简化了描述；
2. 本帖描述省略了各种异常处理。

## 获取

本部分将讲述实体选择器获取实体的工作流程。

获取实体的主要行为定义在函数 `getEntities()` 中。**该函数有一个参数，代表命令的执行者（执行者不一定是实体，也可以是方块、命令函数、控制台等）**。当不满足 `includeNonPlayers`，即只允许选取玩家时，该函数会把后续操作交给 `getPlayers()` 函数进行。

### `getPlayers()` 的流程

1. 当指定了 `playerName` 时，遍历服务器玩家列表，找到玩家名和 `playerName` 一致的玩家（见源码 `net.minecraft.server.PlayerManager#getPlayer`），并**返回**；
2. 当指定了 `uuid` 时，直接从服务器玩家列表中获取 UUID 为 `uuid` 的玩家（该操作基于 `HashMap`，速度非常快）（见源码 `net.minecraft.server.PlayerManager#getPlayer`），并**返回**；
3. 基于命令执行坐标等，**建立断言**（后文有说明）；
4. 当 `senderOnly` 为 `true`，即只选择命令执行者时，将检查命令执行者是否为玩家、是否满足断言，均满足则**返回**命令执行者，否则**返回**空列表。（由于是直接对命令执行者进行判断，而上文又提到过，命令的执行者被作为参数传入函数，因此该判断没有进行任何遍历，非常迅速）；
5. 当 `localWorldOnly` 为 `true`，即只选择命令执行者所在的世界的玩家时，将遍历该世界的玩家列表，**筛选**出满足断言的玩家（见源码 `net.minecraft.server.world.ServerWorld#getPlayers`）；
6. 否则，遍历全服的玩家列表（见源码 `net.minecraft.server.MinecraftServer#getPlayerManager` 和 `net.minecraft.server.PlayerManager#getPlayerList`），**筛选**出满足断言的玩家。
7. 将上述步骤（5 或 6）中**筛选**出的玩家按照 `sorter` 排序，再按照 `limit` 限制的数量从列表中移除多余的玩家（见源码 `net.minecraft.command.EntitySelector#getEntities`），并**返回**。

刚刚我们提到了「断言」，这就进行解释。

### 断言

**断言**，英文 predicate，可简单理解为一系列的条件。当我们把一个实体传入断言中后，断言会进行一系列判断，返回这个实体是否满足各种条件。

上述过程中的「建立断言」，是在**基础断言**的基础上，加入了**坐标断言**后合成出来的新断言。其中，基础断言，是在实体选择器的解析过程中不断建立出来的：在解析过程中，设置 `predicate` 字段的时候，其实是将新内容合并到旧内容中，不断地合并建立出了基础断言（见源码 `net.minecraft.command.EntitySelectorReader#setPredicate`）。坐标断言，

再来看看 `getEntities()` 的流程：

1. 当不满足 `includeNonPlayers`，即只允许选取玩家时，调用上面提到的 `getPlayers()` 函数。
2. 当指定了 `playerName` 时，遍历服务器玩家列表，找到玩家名和 `playerName` 一致的玩家（这部分操作和 `getPlayers()` 里相应部分一致）；
3. 当指定了 `uuid` 时，遍历所有加载的世界，获取该世界中 UUID 为 `uuid` 的实体（后半部分操作同样基于 `HashMap`，速度非常快）（见源码 `net.minecraft.server.world.ServerWorld#getEntity`）；
4. 当以上条件均不满足时：
    0. a

## 总结

## 后语

在最初观看 Wiki 时，看到实体选择器的相关介绍时，我曾有过各种各样的困惑。而如今，我也能够把握十足地回答它们了：

> `@e[type=minecraft:player]`、`@a` 是否等价？性能呢？

并不等价。由于 `@e` 向断言中自动加入 `Entity::isAlive` ，导致前者不能选中死亡的玩家；后者则没有这种限制。

性能区别不大。前者在读取完 `type` 后会设定 `includingNonPlayers` 为 `false`，后者 `@a` 自动设定 `includingNonPlayers` 为 `false`，两者都是从玩家列表中选择玩家。

> `@p[name=SPGoding]`、`SPGoding` 是否等价？ 性能呢？

效果等价，都是选择名为 `SPGoding` 的玩家。

性能上，前者慢于后者。//

> `@e[nbt={UUIDMost:1L,UUIDLeast:1L}]`、`00000000-0000-0001-0000-000000000001` 是否等价？ 性能呢？

效果等价，都是选择 UUID 为  `00000000-0000-0001-0000-00000000000` 的实体。

性能上，前者远远慢于后者。因为前者将遍历全部世界和世界中的全部实体，而后者将在遍历全部世界时直接从 `HashMap` 中获取指定 UUID 对应的实体。

> `@e[type=minecraft:zombie,tag=haha]`、`@e[tag=haha,type=minecraft:zombie]` 是否等价？ 性能呢？

只是交换了一下参数位置，效果肯定等价。//

> `@a[sort=nearest,limit=1]`、`@p` 是否等价？ 性能呢？

效果等价，都是选择最近的玩家。

性能几乎一致。不过，写那么一大长串的人似乎不太聪明…？

## 结语

你对实体选择器有过什么困惑吗？如今，面对着上面整理出的所有实体选择器变量、实体选择器参数的字段设定表，以及实体选择器的工作原理，你的困惑是否解决了呢？如果还有什么疑问，请在本帖留言。

由于笔者完全不会编程，只是对命令略有涉猎，文章中可能有不少错误，望编程 dalaoes 不吝指出，感谢。

## 你知道吗

本部分是本人在分析反编译后的源代码中发现的一些有趣的事情。

1. 在判断实体到指定坐标的距离是否满足选择器参数 `distance` 中规定的范围时，Mojang 进行比较的是距离的平方，省去了很多次开根运算。其实我们在写一些数据包的时候也可以这样的，不一定非要算出距离是多少，距离的平方也许就够用了；
2. 在判断实体 NBT 标签的断言中，有一句向玩家的标签中添加 `SelectedItem` 的代码。也就是说，**我们平时检测得很爽的 `SelectedItem` 标签其实原本是不存在的，它是 Mojang 为了让我们方便检测，硬生生加出来的**。感动！
3. 对字符串进行的 `switch` 编译以后会变成对 `hashCode()` 的 `switch`。这是因为 `switch` 其实只能对数字进行，对字符串的支持是在 Java7 加入的补救措施。
