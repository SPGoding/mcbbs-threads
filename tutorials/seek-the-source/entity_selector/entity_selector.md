# 【CBL】[1.14.4] 追根溯「源」——实体选择器

[MIT](https://github.com/SPGoding/mcbbs-threads/blob/master/LICENSE)

[GitHub 原帖](https://github.com/SPGoding/mcbbs-threads/blob/master/tutorials/seek-the-source/entity_selector/entity_selector.md)

[MCBBS 原帖](https://www.mcbbs.net/thread-891687-1-1.html)

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
- 若第一个字符不是 `@`，则为一个 UUID 或玩家名；
- 若第一个字符是 `@`，则为一个实体选择器。

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
| `type` | 某些情况下设置为 `%值%` | 只有在 `%值%` 不包含感叹号 `!` 时才设置。 |
| `predicate` | 略 | 判断实体的类型是否满足指定条件。 |

##### `tag=%值%`

| 字段 | 值 | 备注 |
| - | - | - |
| `predicate` | 略 | 判断实体的标签是否满足指定条件。 |

注：设置 `predicate` 的函数中有判断 `%值%` 是否为空字符串的部分，如果为空，则会设置 `predicate` 为 `entity.getScoreboardTags().isEmpty() != isNegation`。**因此我们可以使用 `@e[tag=]` 来选择没有标签的实体，用 `@e[tag=!]` 来选择有任意标签的实体**。*我也真佩服 Mojang 绕得出这个逻辑*。

##### `nbt=%值%`

| 字段 | 值 | 备注 |
| - | - | - |
| `predicate` | `TagHelper.areTagsEqual(%值%, tag, true) != isNegation` | 判断实体的 NBT 标签是否满足指定条件。 |

##### `scores=%值%`

| 字段 | 值 | 备注 |
| - | - | - |
| `predicate` | 略 | 判断实体的分数是否满足指定条件。 |

##### `advancements=%值%`

| 字段 | 值 | 备注 |
| - | - | - |
| `includingNonPlayer` | `false` | **不允许包含非玩家**。 |
| `predicate` | 略 | 判断实体的进度是否满足指定条件。 |

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
3. 基于命令执行坐标等，**建立谓词**（后文有说明）；
4. 当 `senderOnly` 为 `true`，即只选择命令执行者时，将检查命令执行者是否为玩家、是否满足谓词，均满足则**返回**命令执行者，否则**返回**空列表。（由于是直接对命令执行者进行判断，而上文又提到过，命令的执行者被作为参数传入函数，因此该判断没有进行任何遍历，非常迅速）；
5. 当 `localWorldOnly` 为 `true`，即只选择命令执行者所在的世界的玩家时，将遍历该世界的玩家列表，**筛选**出满足谓词的玩家（见源码 `net.minecraft.server.world.ServerWorld#getPlayers`）；
6. 否则，遍历全服的玩家列表（见源码 `net.minecraft.server.MinecraftServer#getPlayerManager` 和 `net.minecraft.server.PlayerManager#getPlayerList`），**筛选**出满足谓词的玩家。
7. 将上述步骤（5 或 6）中**筛选**出的玩家按照 `sorter` 排序，再按照 `limit` 限制的数量从列表中移除多余的玩家（见源码 `net.minecraft.command.EntitySelector#getEntities`），并**返回**。

刚刚我们提到了「谓词」，这就进行解释。

### 谓词

**谓词**，英文 predicate，可简单理解为一系列的条件。当我们把一个实体传入谓词中后，谓词会进行一系列判断，返回这个实体是否满足各种条件。

上述过程中的「建立谓词」，指的是在**基础谓词**的基础上，加入**坐标谓词**合成出新谓词的过程。

其中，基础谓词，是在实体选择器的解析过程中不断建立出来的：在解析过程中，设置 `predicate` 字段的时候，其实是在将新谓词合并到原有谓词中（见源码 `net.minecraft.command.EntitySelectorReader#setPredicate`）。在全部解析完成后，又会执行一个函数，添加 `x_rotation`、`y_rotation`、`level` 这三个选择器参数对应的谓词到基础谓词当中（见源码 `net.minecraft.command.EntitySelectorReader#buildPredicate`），至此基础谓词彻底建立完毕。例如，根据上面的字段表，在解析实体选择器 `@e[type=zombie]` 时，当解析完变量 `@e` 后，会将 `Entity::isAlive` 加入谓词；当解析完参数 `type=zombie` 后，会将「判断实体类型是否为僵尸」合并入谓词；全部解析完成后，得到了基础谓词，它要求实体既需要是活的，也需要是一只僵尸。

坐标谓词，是基于 `distance`、`x`、`y`、`z`、`dx`、`dy`、`dz` 这七个选择器参数建立的对实体坐标的谓词。我们省去 `offset`、`box` 等只在源代码中体现的技术性细节不谈，只说近似结论：
1. `x`、`y`、`z` 中缺省的项目会使用命令执行坐标补全；
2. 当存在 `distance` 时，会判定实体所在坐标到 (`x`, `y`, `z`) 的距离是否在 `distance` 限定范围之内；
3. 当存在 `dx`、`dy`、`dz` 的任意一个时，会将缺省项目用 `0.0` 补齐，然后使用以下函数建立 `Box`，判定实体的碰撞箱是否与这个 `Box` 相交：

```Java
// net.minecraft.command.EntitySelectorReader#createBox
private Box createBox(double dx, double dy, double dz) {
    boolean boolean_1 = dx < 0.0D;
    boolean boolean_2 = dy < 0.0D;
    boolean boolean_3 = dz < 0.0D;
    double double_1 = boolean_1 ? dx : 0.0D;
    double double_2 = boolean_2 ? dy : 0.0D;
    double double_3 = boolean_3 ? dz : 0.0D;
    double double_4 = (boolean_1 ? 0.0D : dx) + 1.0D;
    double double_5 = (boolean_2 ? 0.0D : dy) + 1.0D;
    double double_6 = (boolean_3 ? 0.0D : dz) + 1.0D;
    return new Box(double_1, double_2, double_3, double_4, double_5, double_6);
}
```

这是一个非常莫名其妙的函数。说成人话，即：

> 如果 `dx` 小于 `0.0`，那么实体的碰撞箱在 x 轴方向上需要接触的范围是 (`x + dx`, `x + 1.0`)  
> 如果 `dx` 大于等于 `0.0`，那么该范围是 (`x`, `x + dx + 1.0`)  
> 不论 `dx` 取何值，该范围与我们印象中的 (`x`, `x + dx`) 都不相同。
> 其中 `x`、`dx` 均为实体选择器参数。  
> （替换为 `y` 和 `dy`、`z` 和 `dz` 同理。）

还有一件诡异的事情，当你完全没有指定 `dx`、`dy`、`dz` 这三者，但是指定了 `distance` 的最大值（例如 `distance=..5`、`distance=3` 等）的时候，游戏会自动建立出一个判定区域 `Box`，判定实体的碰撞箱是否与这个 `Box` 相交：

```Java
float distanceMax = (Float)this.distance.getMax();
box = new Box(
    (double)(-distanceMax), (double)(-distanceMax), (double)(-distanceMax), 
    (double)(distanceMax + 1.0F), (double)(distanceMax + 1.0F), (double)(distanceMax + 1.0F)
);
```

这一段操作看似多出了不少没必要的操作，实则是对实体选择器极大的优化。为什么呢？且看下一部分，注意对比它与 `getPlayers()` 流程的差异。

### `getEntities()` 的流程

1. 当不满足 `includeNonPlayers`，即只允许选取玩家时，调用上面提到的 `getPlayers()` 函数，并**返回**。
2. 当指定了 `playerName` 时，遍历服务器玩家列表，找到玩家名和 `playerName` 一致的玩家（这一步和 `getPlayers()` 里相应步骤一致），并**返回**；
3. 当指定了 `uuid` 时，遍历所有加载的世界，获取该世界中 UUID 为 `uuid` 的实体（后半部分操作同样基于 `HashMap`，速度非常快）（见源码 `net.minecraft.server.world.ServerWorld#getEntity`），并**返回**；
4. 基于命令执行坐标等，建立谓词；
5. 当 `senderOnly` 为 `true`，即只选择命令执行者时，将检查命令执行者是否为实体、是否满足谓词，均满足则**返回**命令执行者，否则**返回**空列表。（由于是直接对命令执行者进行判断，而上文又提到过，命令的执行者被作为参数传入函数，因此该判断没有进行任何遍历，非常迅速）；
6. 当 `localWorldOnly` 为 `true`，即只选择命令执行者所在的世界的实体时，将调用该世界的 `getEntities()` 函数，**筛选**出满足谓词的实体（见源码 `net.minecraft.server.world.ServerWorld#getEntities`）；
7. 否则，遍历所有世界，调用每个世界的 `getEntities()` 函数，**筛选**出满足谓词的实体。
8. 将上述步骤（6 或 7）中**筛选**出的实体按照 `sorter` 排序，再按照 `limit` 限制的数量从列表中移除多余的实体（见源码 `net.minecraft.command.EntitySelector#getEntities`），并**返回**。

其中，世界的 `getEntities()` 函数代码如下：

```Java
// net.minecraft.world.World#getEntities
public List<Entity> getEntities(@Nullable EntityType<?> type, Box box, Predicate<? super Entity> predicate) {
    /* 所谓 box，是根据 x y z dx dy dz 这六个选择器参数计算出来的方块区域，
     * 而我们上文曾说过，如果指定 distance，游戏会自动计算出一个 box，这便是为了让这一步骤中能够不遍历不必要的区块。
     * 因此，只要参数中指定了 dx dy dz distance 中的任几个，并且 includingNonPlayer 为 true，都可以享受到由本函数带来的优化。
     */
    // 计算出 box 涉及到的区块坐标们。
    int chunkMinX = MathHelper.floor((box.minX - 2.0D) / 16.0D);
    int chunkMaxX = MathHelper.ceil((box.maxX + 2.0D) / 16.0D);
    int chunkMinZ = MathHelper.floor((box.minZ - 2.0D) / 16.0D);
    int chunkMaxZ = MathHelper.ceil((box.maxZ + 2.0D) / 16.0D);
    List<Entity> result = Lists.newArrayList();

    // 遍历这些区块坐标。
    for(int chunkX = chunkMinX; chunkX < chunkMaxX; ++chunkX) {
        for(int chunkZ = chunkMinZ; chunkZ < chunkMaxZ; ++chunkZ) {
        WorldChunk chunk = this.getChunkManager().getWorldChunk(chunkX, chunkZ, false);
        if (chunk != null) {
            /* 调用该区块的 appendEntities 函数，把该区块中满足谓词的实体加入返回的实体列表当中。
             * 而 net.minecraft.world.chunk.Chunk#appendEntities 函数中调用的是 net.minecraft.util.TypeFilterableList#getAllOfType，
             * 在类 net.minecraft.util.TypeFilterableList 中，元素以类型索引，
             * 说了这么多废话，就是想说，如果选择器参数中指定了 type，就只会遍历该类型实体的列表了。
             */
            chunk.appendEntities((EntityType)type, box, result, predicate);
        }
        }
    }

    return result;
}
```

可以看出，这一部分的流程大体与 `getPlayers()` 一致，但是在具体代码实现上，是从实体列表中遍历，还引入了针对实体类型 `type`、针对实体坐标所在区块的特殊优化，使得每次检索实体时不一定遍历整个实体列表，而是可以只获取某几个区块的指定类型的生物的实体列表。

### 小结

实体选择器在获取实体时的步骤，整合以后可以归类如下：

1. 对 UUID 的特殊处理，直接从 `HashMap` 索引，速度非常快；
2. 对 `@s`（`senderOnly`）的特殊处理，直接读取参数，速度非常快；
3. 一般情况下，将从列表中获取实体。字段 `includingNonPlayers` 决定使用实体列表还是玩家列表，字段 `localWorldOnly` 决定范围是当前世界还是全服，是否有字段 `box` 决定范围是某几个区块还是全地图，是否有字段 `type` 决定范围是指定类型的实体列表还是全部实体列表；
4. 把获取到的实体传入谓词，筛选出符合条件的实体；
5. 根据 `sorter` 排序，再根据 `limit` 移除多余实体。

如果你追求性能的话：

1. 在函数全篇大量使用某个相同实体（如 `@e[tag=marker]`）时，不如套一层 `execute as @e[tag=marker] run function xxx`，用极其高效的 `@s` 替换掉多次遍历全服实体列表；
2. 在选择确定类型的实体的时候，在选择器内显式指定不带感叹号且不是实体标签的 `type`；
3. 在选择确定位置的实体的时候，尽量指定 `distance`、`dx`、`dy`、`dz` 中的一个或几个，缩小遍历范围；
4. 如果你有些病态，可以用手动指定 UUID 来替代用 tag 标记 marker，但我个人不太推荐这么做。

另外，由常识：

1. 玩家列表通常比实体列表要小，至少不会比它大；
2. 当前世界的列表通常比全服列表小，至少不会比它大。

因此，如果你是一个十分病态、极致追求性能（其实一般情况下没有必要，真的涉及实体数量非常多的话，MC 自己就会卡得不行了，你命令再怎么高性能也挽救不回来）的玩家的话，可以通过限定选择器变量、选择器参数，在允许的情况下尽量使 `includingNonPlayers` 变为 `false`，使 `localWorldOnly` 变为 `true`。

## 后语

在最初观看 Wiki、看到实体选择器的相关介绍时，我曾有过各种各样的困惑。而如今，我也能够把握十足地回答它们了。请注意，以下内容适用于 Minecraft 1.14.4，可能颠覆您的三观：

> `@e[type=minecraft:player]`、`@a` 是否等价？性能呢？

并不等价。根据「解析」部分的字段表，`@e` 向谓词中自动加入了 `Entity::isAlive`，导致前者不能选中死亡的玩家；后者则没有这种限制。

性能区别不大。前者在解析完 `type` 后会设定 `includingNonPlayers` 为 `false`，后者 `@a` 自动设定 `includingNonPlayers` 为 `false`，两者都是从玩家列表中选择玩家。

> `@e[nbt={UUIDMost:1L,UUIDLeast:1L}]`、`00000000-0000-0001-0000-000000000001` 是否等价？ 性能呢？

效果不等价。如上所述，前者不能选中死亡的实体。不过鉴于实体死亡后很快就会从实体列表中移除，这个差别不是很大。

性能上前者慢于后者。因为前者将遍历全部世界和世界中的全部实体，而后者将在遍历全部世界时直接从 `HashMap` 中获取指定 UUID 对应的实体。

> `@p[name=SPGoding]`、`SPGoding` 是否等价？ 性能呢？

效果等价。都是选择名为 `SPGoding` 的玩家。

性能区别不大。两者都遍历了一遍全服玩家列表。

> `@e[tag=marker]`、`@e[tag=marker,type=minecraft:armor_stand]` 是否等价？ 性能呢？（假设只有盔甲架有 `marker` 标签。）

效果等价。

性能上前者慢于后者，因为前者将遍历各地图、各区块的全部实体，而后者将只遍历各地图、各区块的盔甲架。

> `@a[sort=nearest,limit=1]`、`@p` 是否等价？ 性能呢？

效果等价，都是选择最近的玩家。

性能几乎一致。不过，写那么一大长串的人似乎不太聪明…？

## 结语

不知道本帖是否具有很高的实用性，但我个人认为，从源代码的角度理解实体选择器的运作原理，能让人不那么「被 Mojang 牵着鼻子走」，至少在写下每一个选择器的时候，心里能有点底，知道它到底意味着什么。

由于笔者完全不会编程，只是对命令略有涉猎，文章中可能有不少错误，望各位 dalaoes 不吝指出，感谢。

## 你知道吗

本部分是本人在分析反编译后的源代码中发现的一些有趣的事情。

1. 在判断实体到指定坐标的距离是否满足选择器参数 `distance` 中规定的范围时，Mojang 进行比较的是距离的平方，省去了很多次开根运算。**其实我们在写一些数据包的时候也可以这样的，不一定非要算出距离是多少，距离的平方也许就够用了**；
2. 在判断实体 NBT 标签的谓词中，有一句向玩家的标签中添加 `SelectedItem` 的代码。也就是说，**我们平时检测得很爽的 `SelectedItem` 标签其实原本是不存在的，它是 Mojang 为了让我们方便检测，硬生生加出来的**。感动！
3. 对字符串进行的 `switch` 编译以后会变成对 `hashCode()` 的 `switch`。这是因为 `switch` 其实只能对数字进行，对字符串的支持是在 Java7 加入的补救措施。
