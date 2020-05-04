# 【CBL|SPG】[1.16] 断言 —— 数据包的组成文件之一

------
# 何为断言

**断言（predicate）**，社区常见译名「谓词」「判据」「条件」「特征函数」「皮迪凯特」，在平常交流时可以任意采用合适的、不易与其他词语产生混淆的叫法。其是在 [19w38a](https://www.mcbbs.net/thread-914349-1-1.html) 中加入的新内容，是数据包的组成文件之一。

说是新内容，其实断言**就是**原先[战利品表](https://www.mcbbs.net/thread-831542-1-1.html)中的条件，只不过现在可以被单独抽离出来形成新的文件而已。没有接触过战利品表？没关系，本文将从零开始。

简单来讲，断言储存的是一系列对实体、位置、天气、环境等**上下文**（context）的**判定条件**。首先写好一个断言，再给定一个上下文，游戏就能判定该上下文是否是你想要的。好了，我说完了，大概没什么难理解的。

------
# 文件格式

所有断言文件**均为 JSON 格式**，放置在数据包的 `./data/<命名空间>/predicates` 文件夹下。

该文件内容是一个对象。其中含有 `condition` 字段，指明所用条件的名称；同时还需含有该条件所需的各种附加参数：

- （对象）根对象。
    - （字符串）`condition`：所用条件的名称。详细条件列表请见附录。
    - （任意）*参数名*：该条件所指定的附加参数。

------
# 应用

## 命令 `execute (if|unless) predicate <断言的命名空间 ID>`

该命令能够检测命令的执行者和执行位置等上下文是否满足指定的断言。在 `if` 模式下，只有满足指定断言时，测试才能通过；在 `unless` 模式下则相反。特别地，当命令执行者并不是实体（例如命令方块、控制台等），且断言中有对实体的限定条件时，在 `unless` 模式下也能导致测试通过。

## 实体选择器参数 `predicate`

通过指定该选择器参数，能够筛选出满足（或不满足）指定断言的实体。例如 `@a[predicate=minecraft:test]` 能够选中所有满足放置在数据包下 `./data/minecraft/predicates/test.json` 的文件中条件的玩家，而在等号后方添加一个半角感叹号（`!`）则将选中所有**不**满足该断言的玩家。

像 `tag` 等选择器参数一样，选择器中能够指定多个 `predicate` 参数，只有满足所有参数的实体才能被选中。例如，`@e[predicate=do,predicate=!not,sort=nearest,limit=1]` 会选中最近的、满足 `do` 断言、且不满足 `not` 断言的 1 个实体。

相比于用 `nbt` 选择器参数，`predicate` 选择器参数在大多数情况下性能都会更好。这是因为，`nbt` 选择器参数的检测原理是，先把实体的所有数据转换为 NBT，再将转换后的 NBT 与用户输入的 NBT 进行比较，这些操作十分耗时。而断言会在游戏加载数据包以后就初始化完毕，在选择实体时会直接调用实体的各种方法进行比较，省去了转换为 NBT 的过程。

## 战利品表

战利品表的条件（`conditions`）数组中的每一个对象就是断言。有关战利品表的详细内容可以参阅[【CBL|SPG】[1.15] 战利品表 —— 数据包的组成文件之一](https://www.mcbbs.net/thread-831542-1-3.html)。

------
# 附录 1：通用谓词

## 说明

在断言文件（以及进度的触发器、战利品表的条件）中有许多共通的 JSON 结构。这些结构能够对某个单一的输入（如物品、实体、位置等）根据指定条件做出判定，符合计算机界术语**谓词**（predicate）的定义，因此在本文中我将其称为谓词。

本文中这些通用结构的提炼都是以游戏源代码为基础，而非以「形似」为基础的，能够保证一定的可靠性。

## 范围谓词

「范围谓词」能够判断一个数字是否在你所指定的范围之中。

其格式可以是一个数字。当一个数字与你指定的数字相等时，该范围谓词判定成功。

- （数字）

其格式也可以是一个对象，表明了该数字应当位于的范围。当一个数字处于你指定的范围中时，该范围谓词判定成功。

- （对象）根对象。
    - （数字）`min`：最小值（含）。
    - （数字）`max`：最大值（含）。

以下是几个例子。

### 用数字表示精确的「等于」

```json
"xxx": 5
```

当 `xxx` 的值确实为 5 时，该谓词测试通过。

### 用对象表示精确的「等于」

```json
"xxx": { "min": 5, "max": 5 }
```

当 `xxx` 的值确实为 5 时，该谓词测试通过。

### 用对象表示「大于等于」

```json
"xxx": { "min": 5 }
```

当 `xxx` 的值大于等于 5 时，该谓词测试通过。

### 用对象表示「小于等于」

```json
"xxx": { "max": 5 }
```

当 `xxx` 的值大于等于 5 时，该谓词测试通过。

### 用对象表示「在两数之间」

```json
"xxx": { "min": 5, "max": 10 }
```

当 `xxx` 的值大于等于 5 并且小于等于 10 时，该谓词测试通过。

## 状态谓词

判断一个方块的方块状态或流体的流体状态是否满足你所指定的各个值。

- （对象）根对象。
    - （字符串或布尔或数字）_`名称`_：状态「_`名称`_」对应的值。游戏在比较时会先将这一项的内容转换为字符串再进行比较。
    - （对象）_`名称`_：状态「_`名称`_」对应的值应处的范围。
        - （数字）`min`：最小值。
        - （数字）`max`：最大值。

## 方块谓词

判断一个方块是否满足你所指定的各个条件。

- （对象）根对象。
    - （字符串）`block`：一个方块 ID。方块需要为此处指定的方块，才能使该谓词测试通过。
    - （字符串）`tag`：一个方块标签 ID。方块需要位于该方块标签当中，才能使该谓词测试通过。
    - （字符串）`nbt`：一串 NBT。方块对应的方块实体要能与此处指定的 NBT 匹配，才能使该谓词测试通过。
    - （状态谓词）`state`：该方块的方块状态所需满足的条件。

## 流体谓词

判断一个流体是否满足你所指定的各个条件。

- （对象）根对象。
    - （字符串）`fluid`：一个流体 ID。流体需要为此处指定的流体，才能使该谓词测试通过。
    - （字符串）`tag`：一个流体标签 ID。流体需要位于该流体标签当中，才能使该谓词测试通过。
    - （状态谓词）`state`：该流体的流体状态所需满足的条件。

## 位置谓词

判断当前的位置是否满足你所指定的各个条件。

- （对象）根对象。
    - （字符串）`biome`：该位置所属的生态群系的 ID。
    - （字符串）`dimension`：该位置所属的维度的 ID。
    - （字符串）`feature`：该位置所属的结构的 ID。
    - （方块谓词）`block`：对处于该位置的方块的判定条件。
    - （流体谓词）`fluid`：对处于该位置的流体的判定条件。
    - （对象）`light`：对该位置的光照的判定条件。
        - （范围谓词）`light`：该位置**可见光**所需满足的范围。可见光计算公式为 `max{天空亮度 - 衰减, 方块亮度}`。
    - （对象）`position`：坐标。
        - （范围谓词）`x`：X 坐标。
        - （范围谓词）`y`：Y 坐标。
        - （范围谓词）`z`：Z 坐标。

## 物品谓词

判断一个物品是否满足你所指定的各个条件。

- （对象）根对象。
    - （范围谓词）`count`：物品数量。
    - （范围谓词）`durability`：物品剩余耐久度。
    - （数组）`enchantments`：魔咒列表。如果该列表为空，则任何魔咒都能成功匹配。
        - *（对象）一个魔咒。*
            - （字符串）`enchantment`：魔咒 ID。
            - （范围谓词）`levels`：魔咒等级。
    - （字符串）`item`：物品 ID。
    - （字符串）`nbt`：物品 NBT。
    - （字符串）`potion`：药水 ID，即物品的 `Potion` 标签中指定的内容。就算该物品不是药水，只要有这个标签并且内容匹配，即可通过。
    - （字符串）`tag`：数据包物品标签 ID。

## 实体谓词

判断一个实体是否满足你所指定的各个条件。

- （对象）根对象。
    - （字符串）`catType`：当前猫应当满足的种类。应为一个指向相应猫的材质的路径，例如 `minecraft:textures/entity/cat/tabby.png`。
    - （对象）`distance`：`entity` 中指定实体所在坐标到执行位置的距离。执行位置即为命令的执行位置 `[E|S]`；执行位置即为击杀此实体的实体所在的坐标 `[L:entity]`。
        - （范围谓词）`absolute`：绝对距离。
        - （范围谓词）`horizontal`：水平距离。
        - （范围谓词）`x`：在 X 轴方向上的距离。
        - （范围谓词）`y`：在 Y 轴方向上的距离。
        - （范围谓词）`z`：在 Z 轴方向上的距离。
    - （数组）`effects`：检测的实体所需具有的状态效果列表。
        - *（对象）一个状态效果。*
            - （字符串）`id`：状态效果名称。
            - （范围谓词）`amplifier`：效果倍率。
            - （范围谓词）`duration`：效果持续时间，以刻为单位。
    - （对象）`equipment`：检测的实体所需穿有的装备。
        - （物品谓词）`mainhand`：对实体主手物品的判定条件。
        - （物品谓词）`offhand`：对实体副手物品的判定条件。
        - （物品谓词）`head`：对实体头部装备的判定条件。
        - （物品谓词）`chest`：对实体胸部装备的判定条件。
        - （物品谓词）`legs`：对实体腿部装备的判定条件。
        - （物品谓词）`feet`：对实体脚部装备的判定条件。
    - （对象）`fishing_hook`：对鱼钩实体的判定条件。
        - （布尔） `in_open_wate`：该鱼钩实体是否位于开阔水域当中。
    - （对象）`flags`：检测的实体应具备的特质。
        - （布尔）`is_on_fire`：是否正在着火。
        - （布尔）`is_sneaking`：是否正在潜行。
        - （布尔）`is_sprinting`：是否正在疾跑。
        - （布尔）`is_swimming`：是否正在游泳。
        - （布尔）`is_baby`：是否是幼体。
    - （位置谓词）`location`：检测的实体所在位置。
    - （字符串）`nbt`：检测的实体所需有的 NBT。
    - （对象）`player`：对玩家实体的各种判定条件。一旦指定了该参数，即使内容完全为空，也会要求当前实体必须为一名玩家。
        - （对象）`advancements`：进度。
            - （布尔）*进度 ID*：玩家是否完成了该进度。
            - （对象）*进度 ID*：对该进度中各判据的完成情况进行进一步判断。
                - （布尔）*进度判据名称*：玩家是否完成了该进度的该判据。
        - （字符串）`gamemode`：游戏模式。可选值：`adventure`（冒险模式）、`creative`（创造模式）、`spectator`（旁观者模式）、`survival`（生存模式）。
        - （范围谓词）`level`：经验等级的范围。
        - （对象）`recipes`：配方。
            - （布尔）*配方 ID*：玩家是否解锁了该配方。
        - （数组）`stats`：统计信息。
            - *（对象）一个统计信息*
                - （字符串）`type`：统计信息所属种类。可选值与记分板判据中的一致，但请注意分割符号为冒号（`:`），而不是记分板判据中的点（`.`）：`minecraft:custom`、`minecraft:crafted`、`minecraft:used`、`minecraft:broken`、`minecraft:mined`、`minecraft:killed`、`minecraft:picked_up`、`minecraft:dropped`、`minecraft:killed_by`。
                - （字符串）`stat`：统计信息的 ID。可选值与记分板判据中的一致，但请注意分割符号为冒号（`:`），而不是记分板判据中的点（`.`）。如 `minecraft:leave_game` 等。
                - （范围谓词）`value`：玩家在该统计信息上应满足的数值范围。该数值可以通过 ESC → 「统计信息」菜单查询。
    - （字符串）`team`：检测的实体需所在队伍的队伍名。
    - （字符串）`type`：检测的实体需满足的 ID。如果以 `#` 开头，则会检测实体是否处在相应的实体类型标签当中。

## 伤害类型谓词

判断一个伤害的类型是否满足你所指定的各个条件。

- （对象）根对象。
    - （布尔）`bypasses_armor`：是否破甲。
    - （布尔）`bypasses_invulnerability`：是否无视无敌状态（如虚空伤害）。
    - （布尔）`bypasses_magic`：是否由饥饿引起。
    - （布尔）`is_explosion`：是否由爆炸引起。
    - （布尔）`is_fire`：是否由火焰引起。
    - （布尔）`is_magic`：是否由魔法引起。
    - （布尔）`is_projectile`：是否由弹射物引起。
    - （布尔）`is_lightning`：是否由闪电引起。
    - （实体谓词）`direct_entity`：伤害的直接来源实体（如弹射物）。
    - （实体谓词）`source_entity`：伤害的根本来源实体（如射箭的骷髅）。

------
# 附录 2：条件列表

该列表最终更新时间：1.15.1

## 说明

通常，断言中的参数大多是可以省略的。一旦省略该参数，表明你对该参数的值没有任何要求：即不论实际情况中该参数为何值，甚至在该参数没有值的情况下，断言都绝不会因为这个参数的原因而不通过。但是，有些参数是不能省略的，一旦省略游戏就会报错。可以在断言没有正确读取时，打开游戏日志查看报错信息，并回帖提醒本人为该参数添加注释。

## 适用范围

每个条件都只能在一定的范围下使用。在本文中，将使用 `[E]` 表示能在 **e**xecute (if|unless) predicate 命令中正常运作，用 `[S]` 表示能在**实**体选择器（**s**elector）参数中正常运作，用 `[L:xxx]` 表示能在 `minecraft:xxx` 以及 `minecraft:generic` 类型的战利品表（**l**oot table）正常运作。

在不适用的地方使用某个条件，会导致该条件永远不通过。如果用在不合适的战利品表中，还会引起游戏在加载该数据包时在日志中显示警告：`Found validation problem in {战利品表的命名空间 ID}.出现该错误的 JSON 路径: Parameters [<parameter 出问题的参数名>] are not provided in this context`

## minecraft:alternative `[E|S|L:*]`

析取范式。即，若指定的条件中只要有任一个通过，则该条件通过；如果指定的所有条件都不通过，则该条件不通过。

- （数组）`terms`：条件列表。
    - *（对象）一个条件。格式同条件的格式。*

例子：
```
{
    "condition": "minecraft:alternative",
    "terms": [
        {
            "condition": "minecraft:weather_check",
            "raining": false
        },
        {
            "condition": "minecraft:match_tool",
            "predicate": {
                "item": "minecraft:diamond_axe"
            }
        }
    ]
}
```
当玩家使用的工具为钻石斧，或当前天气为晴天时，条件通过。

## minecraft:block_state_property `[L:block]`

匹配被破坏方块。

- （字符串）`block`：方块 ID。
- （方块状态谓词）`properties `：方块状态。

例子：
```
{
    "condition": "minecraft:block_state_property",
    "block": "minecraft:yellow_bed",
    "properties": {
        "part": "head"
    }
}
```
当被破坏的方块是黄色床的头的一部分时，条件通过。**床在被破坏时，头和脚两部分会分别执行战利品表。为了避免掉落两个床，Minecraft 在写床的战利品表的时候加了一个 `"part": "head"` 的限制。**

## minecraft:damage_source_properties `[L:entity]`

匹配该实体死亡时受到的伤害。

- （伤害类型谓词）`predicate`：对该伤害的判定条件。

例子：
```
{
    "condition": "minecraft:damage_source_properties",
    "predicate": {
        "taken": {
            "min": 20
        }
    }
}
```
当实际造成伤害大于等于 20 时，条件通过。

## minecraft:entity_properties `[E|S|L:entity]`

在指定的实体满足指定条件时，该条件通过。

- （字符串）`entity`：检测的实体。使用 `this` 指代命令的执行者 `[E]`；使用 `this` 指代被选择的实体 `[S]`；使用 `this` 指代死亡的实体。使用 `killer` 指代击杀此实体的实体。使用 `killer_player` 指代击杀此实体的玩家 `[L:entity]`。
- （实体谓词）`predicate`：对 `entity` 中指定的实体的判定条件。

例子：
```
{
    "condition": "minecraft:entity_properties",
    "predicate": {
        "flags": {
            "is_on_fire": true
        }
    },
    "entity": "this"
}
```
该条件会在 `this` 指定的实体着火时通过。可以在战利品表中利用它做出生物被烧死后掉落熟食的效果。
```
{
    "condition": "minecraft:entity_properties",
    "predicate": {
        "equipment": {
            "mainhand": {
                "item": "minecraft:air"
            }
        }
    },
    "entity": "this"
}
```
该条件会在 `this` 指定的实体主手中没有拿任何物品时通过。

## minecraft:entity_scores `[E|S|L:entity]`

在指定的实体的分数满足指定条件时，该条件通过。

- （字符串）`entity`：检测的实体。使用 `this` 指代命令的执行者 `[E]`；使用 `this` 指代被选择的实体 `[S]`；使用 `this` 指代死亡的实体。使用 `killer` 指代击杀此实体的实体。使用 `killer_player` 指代击杀此实体的玩家 `[L:entity]`。
- （对象）`scores`：要检测的分数。必须全部满足，条件才能通过。
    - （范围谓词）_分数名_：该分数的值。

例子：
```
{
    "condition": "minecraft:entity_scores",
    "entity": "killer_player",
    "scores": {
        "foo": {
            "min": 10
        },
        "bar": 15
    }
}
```
在击杀该实体的玩家 foo 分数大于 10，且 bar 分数等于 15 时，条件通过 `[L:entity]`。

## minecraft:inverted `[E|S|L:*]`

取反。即，若指定的条件通过，则该条件不通过；否则，该条件通过。

- （对象）`term`：指定的条件。格式同条件的格式。

例子：
```
{
    "condition": "minecraft:inverted",
    "term": {
        "condition": "minecraft:random_chance",
        "chance": 0.3
    }
}
```
`term` 内指定的条件有 30% 的概率通过，因此整个 `inverted` 条件有 70% 的概率通过。

## minecraft:killed_by_player `[L:entity]`

当前实体被玩家击杀时，条件通过。

- （布尔）`inverse`：如果设置为 `true`，则变为击杀者不是玩家时条件通过。

例子：
```
{
    "condition": "minecraft:killed_by_player"
}
```

## minecraft:location_check `[E|S|L:*]`

当前位置满足指定条件时，条件通过。

- （数字）`offsetX`：在检测位置前，先将当前位置在 X 轴上进行的偏移数目。
- （数字）`offsetY`：在检测位置前，先将当前位置在 Y 轴上进行的偏移数目。
- （数字）`offsetZ`：在检测位置前，先将当前位置在 Z 轴上进行的偏移数目。
- （位置谓词）`predicate`：对该位置的判定条件。

例子：
```
{
    "condition": "minecraft:location_check",
    "predicate": {
        "biome": "minecraft:jungle"
    },
    "offsetX": -10
}
```
将当前位置向 X 轴负方向偏移 10 米后如果处于丛林生态群系，则条件通过。

## minecraft:match_tool `[L:block,fishing]`

匹配玩家使用的工具。

- （物品谓词）`predicate`：对该工具的判定条件。

例子：
```
{
    "condition": "minecraft:match_tool",
    "predicate": {
        "enchantments": [
            {
                "enchantment": "minecraft:silk_touch",
                "levels": {
                    "min": 1
                }
            }
        ]
    }
}
```
当玩家使用的工具具有「精准采集Ⅰ」及以上的魔咒，条件通过。

## minecraft:random_chance `[E|S|L:*]`

游戏会生成一个 0.0 到 1.0 的随机数，当该数字小于等于**被比数**时，条件通过。

- （数字）`chance`：被比数。值域 [0.0, 1.0]。

例子：
```
{
    "condition": "minecraft:random_chance",
    "chance": 0.3
}
```
该条件有 30% 的几率通过

## minecraft:random_chance_with_looting `[E|S|L:entity]`

游戏会生成一个 0.0 到 1.0 的随机数，当该数字小于等于**被比数**时，条件通过 `[E|S|L:entity]`。

击杀者的抢夺附魔等级会影响被比数的大小 `[L:entity]`。

- （数字）`chance`：被比数的基础值。值域 [0.0, 1.0] `[E|S|L:entity]`。
- （数字）`looting_multiplier`：每一级抢夺附魔所增加的被比数 `[L:entity]`。

例子：
```
{
    "condition": "minecraft:random_chance_with_looting",
    "chance": 0.3,
    "looting_multiplier": 0.1
}
```
该条件有 30% 的几率通过 `[E|S|L:entity]`。每多一级抢夺魔咒，通过几率增加 10 个百分点 `[L:entity]`。

## minecraft:reference `[E|S|L:*]`

引用定义的其他断言文件。

- （字符串）`name`：判据的命名空间 ID。

例子：
```
{
    "condition": "minecraft:reference",
    "name": "test:abc"
}
```
该条件将会引用定义在数据包 `./data/test/predicates/abc.json` 文件中的断言。当被引用的断言能够通过时，该条件通过。

## minecraft:survives_explosion `[E|S|L:block]`

如果该方块是被爆炸破坏的，该条件有 1/爆炸半径 的几率通过；如果该方块是被玩家正常挖掘的，该条件永远通过 `[L:block]`。

该条件永远通过 `[E|S]`。

例子：
```
{
    "condition": "minecraft:survives_explosion"
}
```

## minecraft:table_bonus `[L:block]`

分别定义不同级别的附魔等级下条件的通过几率。

- （字符串）`enchantment`：探测的魔咒 ID。
- （数组）`chances`：每个魔咒等级下的通过几率。
    - *（数字）无该魔咒的几率*
    - *（数字）该魔咒为 1 级的几率*
    - *（数字）该魔咒为 2 级的几率*
    - *...*
    - *（数字）该魔咒为 (n-1) 级的几率*

例子：
```
{
    "condition": "minecraft:table_bonus",
    "enchantment": "minecraft:fortune",
    "chances": [
        0.05,
        0.0625,
        0.083333336,
        0.1
    ]
}
```
当玩家没有时运附魔时，有 5% 的几率通过；当玩家有 1 级时运时，有 6.25% 的几率通过；当玩家有 2 级时运时，有 8.3333336% 的几率通过；当玩家有 3 级及以上的时运时，有 10% 的几率通过。

## minecraft:time_check `[E|S|L:*]`

检测当前游戏内的时间。

- （范围谓词）`value`：允许的值。
- （数字）`period`：可选。如果指定，在比较前会先将游戏的时间以该数取模。

例子：
```
{
    "condition": "minecraft:time_check",
    "value": 6000
}
```
该条件会在游戏内每天正午（`6000`）的那一刻通过。
```
{
    "condition": "minecraft:time_check",
    "value": 6000,
    "period": 12000
}
```
该条件会在游戏内每天正午（`6000`）或午夜（`18000`）的那一刻通过。

## minecraft:weather_check `[E|S|L:*]`

检测当时的天气。

- （布尔）`raining`：是否下雨。
- （布尔）`thundering`：是否打雷。

例子：
```
{
    "condition": "minecraft:weather_check",
    "raining": true,
    "thundering": false
}
```
该条件会在普通的雨天通过，在晴天或雷雨天均不会通过。

------
# 附录 3：Datapack Helper Plus (JSON)

[Datapack Helper Plus (JSON)](https://www.mcbbs.net/thread-897610-1-1.html)（简称 `DHP (JSON)`，中文名`大憨批杰森`）是一款优秀的 [VSCode](https://code.visualstudio.com) 插件，提供适用于 Minecraft 1.15 数据包 JSON 文件的自动补全与格式校验。在该款插件的帮助下，编写断言以及其他各种文件（如进度、配方、战利品表、各种标签）将变得十分方便。请您坐和放宽，前往该插件的[发布帖](https://www.mcbbs.net/thread-897610-1-1.html)一览究竟。

------
# 致谢

- chyx - 挑错
- ruhuasiyu - 帮忙补充了点东西
- 柘木铃 - 挑错
