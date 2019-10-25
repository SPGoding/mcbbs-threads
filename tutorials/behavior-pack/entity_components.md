# 附录：组件

组件在 Wiki 的文档中被分为四类，一类是描述实体所固有**属性**（如碰撞箱等），一类描述实体的**行为**（即 **AI 目标**，例如能够跟随玩家、破坏方块等），一类是在指定情况下会被触发的**触发器**，还有一类其他。下面将分别介绍这四类下面都有哪些组件可以使用。在 `components` 以及每一个组件组下面，我们都可以直接使用下面提到的所有组件。用法为：

```
{
    "components": {
        "组件名": {
            "参数 1": 值, // 有些组件可以指定参数，但是你也可以不指定，依你的需求而定。
            "参数 2": 值,
            ...
        },
        "又一个组件名": {} // 如果某个组件没有参数，或者你不想指定任何参数，直接闭合大括号即可。
    }
}
```

下面将分别介绍这些组件，篇幅较长。其中示例均是从 Minecraft 原版行为包中抠出来的。

[spoiler]

## 触发器

首先讲解触发器，是因为在后面有些组件的参数中，使用到了与触发器完全相同的 JSON 格式。

触发器，是一种能够根据指定滤器来执行事件的东西（有关滤器、事件的详细内容，可以通过上方目录切换到相关附录）。它的工作流程如下所示：

![触发器.png](https://i.loli.net/2019/10/25/GVYvFQqrd2ih3I4.png)

### 基础格式

- `event`：（字符串）该触发器被触发后要执行的事件。
- `filters`：（对象）可选。一个滤器。如果指定了，则只有在该滤器通过的情况下才会执行 `event` 中指定的事件。
- `target`：（字符串）可选。执行该事件的目标。默认为 `self`，代表该实体自身。

再次强调，触发器属于组件的一种，所以要定义在 `components` 或是 `component_groups` 中的一个组件组之下。

### minecraft:on_death

触发时机：该实体死亡时。**只能被末影龙使用**。

```
{
    "event": "minecraft:start_death",
    "target": "self"
}
```

### minecraft:on_friendly_anger

触发时机：该实体附近有其他相同种类的实体愤怒时。

### minecraft:on_hurt

触发时机：该实体受到伤害时。

```
{
    "event": "minecraft:crystal_explode",
    "target": "self"
}
```

### minecraft:on_hurt_by_player

触发时机：该实体被玩家攻击时。

### minecraft:on_ignite

触发时机：该实体着火时。

### minecraft:on_start_landing

触发时机：该实体着陆时。**只能被末影龙使用**。

```
{
    "event": "minecraft:start_land",
    "target": "self"
}
```

### minecraft:on_start_takeoff

触发时机：该实体开始飞行时。**只能被末影龙使用**。

```
{
    "event": "minecraft:start_fly",
    "target": "self"
}
```

### minecraft:on_target_acquired

触发时机：该实体找到攻击目标时。

```
{
    "event": "minecraft:become_angry",
    "target": "self"
}
{
    "event": "minecraft:has_target",
    "target": "self"
}
{
    "filters": {
        "all_of": [
            {
                "test": "is_family",
                "subject": "target",
                "value": "wolf"
            },
            {
                "test": "has_component",
                "subject": "target",
                "operator": "!=",
                "value": "minecraft:is_tamed"
            }
        ]
    },
    "event": "minecraft:mad_at_wolf",
    "target": "self"
}
{
    "event": "minecraft:become_aggressive",
    "target": "self"
}
{
    "event": "minecraft:become_aggressive",
    "target": "self"
}
{
    "event": "minecraft:become_aggro",
    "target": "self"
}
```

### minecraft:on_target_escape

触发时机：该实体失去攻击目标时（目标已死亡，或逃出了该实体的视距等）。

```
{
    "event": "minecraft:stop_exploding",
    "target": "self"
}
{
    "event": "minecraft:on_calm",
    "target": "self"
}
{
    "event": "minecraft:lost_target",
    "target": "self"
}
{
    "filters": {
        "all_of": [
            {
                "test": "is_family",
                "subject": "target",
                "value": "wolf"
            },
            {
                "test": "has_component",
                "subject": "target",
                "operator": "!=",
                "value": "minecraft:is_tamed"
            }
        ]
    },
    "event": "minecraft:on_calm",
    "target": "self"
}
{
    "event": "minecraft:become_calm",
    "target": "self"
}
{
    "event": "minecraft:become_calm",
    "target": "self"
}
{
    "event": "minecraft:stop_aggro",
    "target": "self"
}
```

### minecraft:on_wake_with_owner

触发时机：该实体的主人在与该实体一起睡觉后醒来时。

## 属性

定义实体的固有属性。

### minecraft:attack

定义实体的近战攻击及其附加效果。


- `damage`：（数组）近战伤害的范围。格式为 `[最低伤害, 最高伤害]`。
- `effect_duration`：（数字，默认 0.0）对被打的实体施加的状态效果持续时间，以秒为单位。
- `effect_name`：（字符串）对被打的实体施加的状态效果的名称。


```
{
    "damage": 3,
    "effect_name": "hunger",
    "effect_duration": 30
}
{
    "damage": {
        "range_min": 7,
        "range_max": 21
    }
}
{
    "damage": 6
}
{
    "damage": 4,
    "effect_name": "wither",
    "effect_duration": 10
}
```
### minecraft:spell_effects

定义添加该组件时对该实体添加/移除的状态效果


- `add_effects`：（数组）要添加的状态效果。

- *（对象）一个状态效果*

- `ambient`：（布尔值）是否为环境效果。
- `amplifier`：（数字）效果的等级。
- `duration`：（数字）效果的持续时间。
- `visible`：（布尔值）效果是否可见（大概指粒子效果）。

- `remove_effects`：（字符串）要移除的状态效果的名字。

### minecraft:strength

定义该实体携带物品的「力量」。


- `max`：（整数，默认 5）实体的最大力量。
- `value`：（整数，默认 1）实体的初始力量。

### minecraft:ambient_sound_interval

设置实体的环境音效的播放间隔。


- `range`：（数字，默认 16.0）随机添加到播放间隔的时间最大值，单位为秒。
- `value`：（数字，默认 8.0）实体再次播放环境音效的时间最小值，单位为秒。

### minecraft:can_climb

允许实体爬楼梯。

### minecraft:can_fly

允许实体飞行。寻路系统将不再限制实体脚下的方块必须为固体方块。

### minecraft:can_power_jump

允许实体蓄力跳。（类似原版中马的表现。）

### minecraft:collision_box

设置该实体碰撞箱的宽和高。


- `height`：（数字，默认 1.0）碰撞箱的高度，单位为格。
- `width`：（数字，默认 1.0）碰撞箱的宽度，单位为格。

```
{
    "width": 0.6,
    "height": 1.8
}
```
### minecraft:color

定义实体的颜色。只对原版的有颜色预制的生物有效（例如羊、潜影贝）。


- `value`：（整数，默认 0）颜色值。

```
{
    "value": 0
}
```
### minecraft:color2

定义实体的第二个颜色。只对原版的有第二个颜色预制的生物有效（热带鱼）。


- `value`：（整数，默认 0）颜色值。

```
{
    "value": 0
}
```
### minecraft:default_look_angle

设置该实体默认状态下头的角度。


- `value`：（数字，默认 0.0）角度，单位为度。

### minecraft:equipment

设置该实体的装备使用的战利品表。（基岩版中，使用战利品表来设置实体的装备）


- `table`：（字符串）战利品表的相对路径，从行为包的根目录起始，包含 `.json` 后缀。
- `slot_drop_chance`：（字符串）可能掉落装备的物品的栏位。

```
{
    "table": "loot_tables/entities/drowned_ranged_equipment.json",
    "slot_drop_chance": [
        {
            "slot": "slot.weapon.offhand",
            "drop_chance": 1
        }
    ]
}
{
    "table": "loot_tables/entities/drowned_equipment.json",
    "slot_drop_chance": [
        {
            "slot": "slot.weapon.offhand",
            "drop_chance": 1
        }
    ]
}
{
    "table": "loot_tables/entities/zombie_equipment.json"
}
```
### minecraft:fire_immune

使实体不会受到火焰伤害。

### minecraft:floats_in_liquid

使实体能够在液体中悬浮。

### minecraft:flying_speed

设置实体的飞行速度。


- `value`：（数字，默认 0.02）每刻实体飞过的方块的格数。

```
{
    "value": 0.6
}
```
### minecraft:foot_size

设置实体在非跳跃时的步行速度。


- `value`：（数字，默认 0.5）实体迈一步的大小。

### minecraft:friction_modifier

定义「摩擦力」对该实体的影响程度。


- `value`：（数字，默认 1.0）数字越大，摩擦力影响越大。`1.0` 表示正常的摩擦，`2.0` 表示正常摩擦的二倍，`0.5` 表示一半，诸如此类。

### minecraft:ground_offset

设置实体从地面的偏移。


- `value`：（数字，默认 0.0）实体从地面向上偏移的距离，单位为格。

### minecraft:input_ground_controlled

如果该实体可以被骑乘，添加该组件可以使玩家能够使用方向键（WASD）控制实体移动。

### minecraft:is_baby

设置该实体为幼儿状态。

### minecraft:is_charged

设置该实体带电。

### minecraft:is_chested

设置该实体携带了一个箱子。

### minecraft:is_dyeable

允许染料改变该实体的颜色。


- `interact_text`：（字符串）触屏模式下用染料右键该实体时显示的文本。

```
{
    "interact_text": "action.interact.dye"
}
```
### minecraft:is_ignited

设置该实体正在着火。

### minecraft:is_saddled

设置该实体正被装以马鞍。

### minecraft:is_shaking

设置该实体正在抖动。

### minecraft:is_sheared

设置该实体已被剪（羊毛）。

### minecraft:is_stackable

设置该实体可以被堆放。

### minecraft:is_tamed

设置该实体已被驯服。

### minecraft:item_controllable

设置在骑乘时可以控制该实体的物品。


- `control_items`：（数组）可以控制该实体的物品

- *（字符串）一个物品*


```
{
    "control_items": "carrotOnAStick"
}
```
### minecraft:loot

设置该实体死亡时使用的战利品表。


- `table`：（字符串）战利品表的相对路径，从行为包的根目录起始，包含 `.json` 后缀。使用左斜杠 `/` 分割路径。

```
{
    "table": "loot_tables/entities/zombie.json"
}
```
### minecraft:mark_variant

附加的变种值。可以用于进一步区分不同的变种。


- `value`：（整数，默认 0）变种的 ID。按照惯例，0 是基础实体的 ID。

```
{
    "value": 1
}
```
### minecraft:push_through

设置实体可以通过的距离。


- `value`：（数字，默认 0.0）单位为方块。

### minecraft:scale

设置该实体在视觉效果上的缩放倍数。


- `value`：（数字，默认 1.0）该实体被放大的倍数。`1.0` 表示与模型里面定义的大小一致，`2.0` 表示二倍，`0.5` 表示一半，诸如此类。该选项并不会实际改变碰撞箱的大小。

```
{
    "value": 0.5
}
```
### minecraft:sound_volume

设置该实体音效的基础音量。


- `value`：（数字，默认 1.0）音量的大小。

### minecraft:type_family

设置该实体所属于的分类。


- `family`：（数组）分类。

- *（字符串）分类名*


```
{
    "family": [
        "leatherworker",
        "zombie_villager",
        "zombie",
        "undead",
        "monster"
    ]
}
```
### minecraft:variant

用于从一个实体其他的变种中，区分出某个变种的组件组（可以参考村民或豹猫的 JSON 文件）。


- `value`：（整数，默认 0）变种的 ID。按照惯例，0 是基础实体的 ID。

```
{
    "value": 0
}
```
### minecraft:walk_animation_speed

设置实体的行走动画的播放速度。


- `value`：（数字，默认 1.0）数字越大，动画播放越快。`1.0` 表示正常的播放速度，`2.0` 表示正常速度的二倍，`0.5` 表示一半，诸如此类。

### minecraft:wants_jockey

设置该实体想要成为骑士。

### AI 目标

定义该实体的 AI 目标。

所有 AI 目标都有的参数：


- `priority`：（数字）该 AI 组件的优先级。数字越小，优先级越高。优先级高的 AI 组件会优先进行执行。


### minecraft:behavior.scared

当下雨时使该实体害怕。


- `sound_interval`：（整数，默认 0）声音播放的间隔。

```
{
    "priority": 1,
    "sound_interval": 20
}
```
### minecraft:behavior.avoid_mob_type

允许该实体躲避某种实体。


- `entity_types`：（数组）该实体躲避的实体类型。

- *（对象）*

- `filters`：（对象）滤器（有关滤器的详细内容请查看下方「滤器」部分）。
- `max_dist`：（整数，默认 16）该实体会被躲避的最远距离。
- `must_see`：（布尔值，默认 `false`）如果为 `true`，该实体必须在视野范围内才会被躲避。
- `sprint_speed_multiplier`：（数字，默认 1.0）在躲避时的奔跑速度的倍数。`1.0` 表明速度未改变。
- `walk_speed_multiplier`：（数字，默认 1.0）在躲避时的行走速度的倍数。`1.0` 表明速度未改变。

- `ignore_visibility`：（布尔值，默认 `false`）如果为 `true`，则会无视实体的隐身效果，直接进行躲避。
- `max_dist`：（数字，默认 0.0）寻找实体的最大距离。
- `max_flee`：（数字，默认 0.5）实体停止躲避的距离。
- `probability_per_strength`：（数字，默认 1.0）决定该实体根据另一个实体的力量而停止躲避的可能性。
- `sprint_speed_multiplier`：（数字，默认 1.0）在躲避时的奔跑速度的倍数。`1.0` 表明速度未改变。
- `walk_speed_multiplier`：（数字，默认 1.0）在躲避时的行走速度的倍数。`1.0` 表明速度未改变。

```
{
    "priority": 3,
    "entity_types": [
        {
            "filters": {
                "test": "is_family",
                "subject": "other",
                "value": "llama"
            },
            "max_dist": 24,
            "walk_speed_multiplier": 1.5,
            "sprint_speed_multiplier": 1.5
        }
    ],
    "probability_per_strength": 0.14
}
```
### minecraft:behavior.beg

允许实体在玩家手里拿着它们喜欢的食物时，看着玩家并跟随玩家。


- `items`：（数组）该实体喜欢的食物。

- *（字符串）物品名*
- `look_distance`：（数字，默认 8.0）检测的距离。
- `look_time`：（数组，默认 `[2, 4]`）实体会看向玩家的时间所处的范围，单位为秒。

```
{
    "priority": 9,
    "look_distance": 8,
    "look_time": [
        2,
        4
    ],
    "items": [
        "bone",
        "porkchop",
        "cooked_porkchop",
        "chicken",
        "cooked_chicken",
        "beef",
        "cooked_beef",
        "rotten_flesh",
        "muttonraw",
        "muttoncooked",
        "rabbit",
        "cooked_rabbit"
    ]
}
```
### minecraft:behavior.break_door

允许该实体破坏门。

### minecraft:behavior.breed

允许该实体和其他实体交配。


- `speed_multiplier`：（数字，默认 1.0）在前往交配时的移动速度的倍数。`1.0` 表明速度未改变。

```
{
    "priority": 2,
    "speed_multiplier": 1
}
```
### minecraft:behavior.charge_attack

允许该实体追着目标打。

### minecraft:behavior.circle_around_anchor

允许该实体绕着某一点或目标转圈。


- `goal_radius`：（数字，默认 0.5）实体认为它已达成目标的距离。这是防止实体一直转圈尝试精确到达某一点的「回旋的余地」。
- `height_above_target_range`：（数组，默认 `[0.0, 0.0]`）当找到目标时，锚点位置在目标上方的高度的区间。
- `height_change_chance`：（整数，默认 350）决定改变锚点位置的几率。每刻有 1 / 值 的几率改变。
- `height_offset_range`：（数组，默认 `[0.0, 0.0]`）实体距离锚点的距离的区间。
- `radius_change_chance`：（整数，默认 250）决定将半径增加到最大半径的几率。每刻有 1 / 值 得几率增加。
- `radius_range`： （数组，默认 `[0.0, 0.0]`）实体旋转的圈的半径的区间。
- `speed_multiplier`：（数字，默认 1.0）使用该 AI 目标时移动速度的倍数。`1.0` 表明速度未改变。

```
{
    "priority": 3,
    "radius_range": [
        5,
        15
    ],
    "radius_change_chance": 250,
    "height_above_target_range": [
        20,
        40
    ],
    "height_offset_range": [
        -4,
        5
    ],
    "height_change_chance": 350,
    "goal_radius": 1
}
```
### minecraft:behavior.controlled_by_player

允许该实体被玩家控制。

### minecraft:behavior.defend_village_target

允许该实体待在村庄，与对村民有害的敌对生物展开生死搏斗。


- `entity_types`：（数组）该实体攻击的实体类型。

- *（对象）*

- `filters`：（对象）滤器（有关滤器的详细内容请查看下方「滤器」部分）。
- `max_dist`：（整数，默认 16）该实体会被攻击的最远距离。
- `must_see`：（布尔值，默认 `false`）如果为 `true`，该实体必须在视野范围内才会被攻击。
- `sprint_speed_multiplier`：（数字，默认 1.0）在攻击时的奔跑速度的倍数。`1.0` 表明速度未改变。
- `walk_speed_multiplier`：（数字，默认 1.0）在攻击时的行走速度的倍数。`1.0` 表明速度未改变。



### minecraft:behavior.door_interact

允许该实体开关门。

### minecraft:behavior.dragonchargeplayer

允许末影龙通过快速飞行攻击玩家（玩家由 `minecraft:behavior.dragonscanning` 目标选择）。只能用于末影龙。

### minecraft:behavior.dragondeath

允许末影龙高调地死。控制末影龙的死亡动画。只对末影龙有效。

### minecraft:behavior.dragonflaming

允许末影龙喷火攻击。只对末影龙有效。

### minecraft:behavior.dragonholdingpattern

允许末影龙绕着中心飞行。只对末影龙有效。

### minecraft:behavior.dragonlanding

允许末影龙停止飞行，进入栖息模式。只对末影龙有效。

### minecraft:behavior.dragonscanning

允许末影龙在栖息时选择攻击的玩家对象。只对末影龙有效。

### minecraft:behavior.dragonstrafeplayer

允许末影龙绕着一个玩家并对他吐火球。只对末影龙有效。

### minecraft:behavior.dragontakeoff

允许末影龙停止栖息模式，进入飞行模式。只对末影龙有效。

### minecraft:behavior.drop_item_for

允许该实体移向一个目标并为其丢下物品。


- `drop_item_chance`：（数字，默认 1.0）掉落物品的几率。
- `entity_types`：（数组）所有有效的实体类型。

- *（对象）*

- `filters`：（对象）滤器（有关滤器的详细内容请查看下方「滤器」部分）。
- `max_dist`：（整数，默认 16）该实体有效的最远距离。
- `must_see`：（布尔值，默认 `false`）如果为 `true`，该实体必须在视野范围内才有效。
- `sprint_speed_multiplier`：（数字，默认 1.0）奔跑速度的倍数。`1.0` 表明速度未改变。
- `walk_speed_multiplier`：（数字，默认 1.0）行走速度的倍数。`1.0` 表明速度未改变。

- `goal_radius`：（数字，默认 0.5）实体认为它已达成目标的距离。这是防止实体一直转圈尝试精确到达某一点的「回旋的余地」。
- `loot_table`：（字符串）物品所使用的战利品表。
- `max_dist`：（数字，默认 0.0）寻找为其丢下物品的实体的最大距离。
- `offering_distance`：（数字，默认 1.0）当丢下物品时实体尝试远离的距离。
- `on_drop_attempt`：（对象，格式同触发器）当实体尝试丢下物品时触发。
- `search_height`：（整数，默认 1）目标实体可在的高度范围。
- `search_range`：（整数，默认 0）目标实体可在的距离。
- `speed_multiplier`：（数字，默认 1.0）在前往丢下物品的路上时的移动速度的倍数。`1.0` 表明速度未改变。
- `time_of_day_range`：（数组，默认 `[0.0, 1.0]`）该 AI 目标组件可被使用的时间节点。`0.0` 表示正午，`0.25` 表示日落，`0.5` 表示午夜，`0.75` 表示日出，`1.0` 表示回到正午。

```
{
    "priority": 1,
    "speed_multiplier": 1,
    "search_range": 5,
    "search_height": 2,
    "search_count": 0,
    "goal_radius": 1,
    "entity_types": [
        {
            "filters": {
                "test": "is_family",
                "subject": "other",
                "value": "player"
            },
            "max_dist": 6
        }
    ],
    "max_dist": 5,
    "drop_item_chance": 0.7,
    "offering_distance": 5,
    "loot_table": "loot_tables/entities/cat_gift.json",
    "time_of_day_range": [
        0.74999,
        0.8
    ],
    "on_drop_attempt": {
        "event": "minecraft:cat_gifted_owner",
        "target": "self"
    }
}
```
### minecraft:behavior.eat_block

允许实体吃掉方块，例如羊吃草方块。


- `on_eat`：（对象，格式同触发器）当实体吃掉一个草方块时触发。

```
{
    "priority": 6,
    "on_eat": {
        "event": "minecraft:on_eat_block",
        "target": "self"
    }
}
```
### minecraft:behavior.enderman_leave_block

允许末影人放下它们拿着的方块。仅对末影人有效。

### minecraft:behavior.enderman_take_block

允许末影人拿起一个方块。仅对末影人有效。

### minecraft:behavior.find_mount

允许该实体找到另一个实体，并骑上去。


- `avoid_water`：（布尔值）如果为 `true`，实体会避免穿过水去寻找坐骑。
- `mount_distance`：（数字，默认 -1.0）该生物需要处于的距离。如果值小于 0，将使用默认的攻击距离。
- `start_delay`：（整数，默认 0）开始去往坐骑前等待的时间。
- `target_needed`：（布尔值，默认 `false`），如果为 `true`，生物只会在其有目标时寻找坐骑。
- `within_radius`：（数字，默认 0.0）该实体寻找的坐骑的距离限制。

```
{
    "priority": 3,
    "within_radius": 16,
    "avoid_water": true,
    "start_delay": 100,
    "target_needed": false,
    "mount_distance": 2
}
```
### minecraft:behavior.find_underwater_treasure

允许该实体移动到最近的沉船或海底废墟。


- `search_range`：（整数，默认 0）该实体会寻找的宝藏的距离限制。
- `speed_multiplier`：（数字，默认 1.0）在使用此 AI 目标组件时的移动速度的倍数。`1.0` 表明速度未改变。
- `stop_distance`：（数字，默认 2.0）实体在停止前会移动的距离。

```
{
    "priority": 2,
    "speed_multiplier": 2,
    "search_range": 30,
    "stop_distance": 50
}
```
### minecraft:behavior.flee_sun

允许实体躲避太阳直射，寻找阴凉处。


- `speed_multiplier`：（数字，默认 1.0）在使用此 AI 目标组件时的移动速度的倍数。`1.0` 表明速度未改变。

```
{
    "priority": 2,
    "speed_multiplier": 1
}
```
### minecraft:behavior.float

允许实体在游泳时保持漂浮。

### minecraft:behavior.float_wander

允许实体像恶魂一样悬浮。


- `float_duration`：（数组，默认 `[0.0, 0.0]`）实体在落地或做其他事情前漂浮的秒数。
- `must_reach`：（布尔值，默认 `false`）如果为 `true`，目标点必须可以到达。
- `random_reselect`：（布尔值，默认 `false`）如果为 `true`，实体会在移动过程中随机更换目标点。
- `xz_dist`：（整数，默认 10）实体寻找地面上的新目标点的距离。必须至少为 1。
- `y_dist`：（整数，默认 7）实体向上/下寻找的新目标点的距离。必须至少为 1。
- `y_offset`：（数字，默认 0.0）添加到选定的目标点的高度偏移。

```
{
    "xz_dist": 10,
    "y_dist": 7,
    "y_offset": -2,
    "random_reselect": true,
    "float_duration": [
        0.1,
        0.35
    ]
}
{
    "priority": 2,
    "must_reach": true
}
```
### minecraft:behavior.follow_caravan

允许实体跟随商队中的实体。


- `entity_count`：（整数，默认 1）商队中可有的实体数量。
- `entity_types`：（数组）所有有效的实体类型。

- *（对象）*

- `filters`：（对象）滤器（有关滤器的详细内容请查看下方「滤器」部分）。
- `max_dist`：（整数，默认 16）该实体有效的最远距离。
- `must_see`：（布尔值，默认 `false`）如果为 `true`，该实体必须在视野范围内才有效。
- `sprint_speed_multiplier`：（数字，默认 1.0）奔跑速度的倍数。`1.0` 表明速度未改变。
- `walk_speed_multiplier`：（数字，默认 1.0）行走速度的倍数。`1.0` 表明速度未改变。

- `speed_multiplier`：（数字，默认 1.0）在使用此 AI 目标组件时的移动速度的倍数。`1.0` 表明速度未改变。

```
{
    "priority": 3,
    "speed_multiplier": 2.1,
    "entity_count": 10,
    "entity_types": {
        "filters": {
            "test": "is_family",
            "subject": "other",
            "value": "llama"
        }
    }
}
```
### minecraft:behavior.follow_mob

允许实体跟随其他生物。


- `search_range`：（整数，默认 0）该实体会跟随的实体的距离限制。
- `speed_multiplier`：（数字，默认 1.0）在使用此 AI 目标组件时的移动速度的倍数。`1.0` 表明速度未改变。
- `stop_distance`：（数字，默认 2.0）实体在停止前会移动的距离。

```
{
    "priority": 3,
    "speed_multiplier": 1,
    "stop_distance": 3,
    "search_range": 20
}
```
### minecraft:behavior.follow_owner

允许该实体跟随它们的主人玩家。


- `speed_multiplier`：（数字，默认 1.0）在使用此 AI 目标组件时的移动速度的倍数。`1.0` 表明速度未改变。
- `start_distance`：（数字，默认 10.0）实体在开始移动前与玩家的最大距离。
- `stop_distance`：（数字，默认 2.0）实体在停止前会移动的距离。

```
{
    "priority": 4,
    "speed_multiplier": 1,
    "start_distance": 10,
    "stop_distance": 2
}
```
### minecraft:behavior.follow_parent

允许该实体跟随它们的父母。


- `speed_multiplier`：（数字，默认 1.0）在使用此 AI 目标组件时的移动速度的倍数。`1.0` 表明速度未改变。

```
{
    "priority": 4,
    "speed_multiplier": 1
}
```
### minecraft:behavior.go_home

允许该实体回到它的出生地。


- `goal_radius`：（数字，默认 0.5）实体认为它已达成目标的距离。这是防止实体一直转圈尝试精确到达某一点的「回旋的余地」。
- `interval`：（整数，默认 120）随机移向某处的几率。有 1 / interval 的几率这么做。
- `on_home`：（对象，格式同触发器）当实体到达目的地时触发。
- `speed_multiplier`：（数字，默认 1.0）在使用此 AI 目标组件时的移动速度的倍数。`1.0` 表明速度未改变。

```
{
    "priority": 1,
    "speed_multiplier": 1,
    "interval": 700,
    "goal_radius": 4,
    "on_home": {
        "event": "minecraft:go_lay_egg",
        "target": "self"
    }
}
```
### minecraft:behavior.guardian_attack

允许守卫者攻击。只能被守卫者或远古守卫者使用。

### minecraft:behavior.harvest_farm_block

允许村民收割附近的农田。只能被村民使用。


- `speed_multiplier`：（数字，默认 1.0）在使用此 AI 目标组件时的移动速度的倍数。`1.0` 表明速度未改变。

```
{
    "priority": 9,
    "speed_multiplier": 0.5
}
```
### minecraft:behavior.hurt_by_target

允许实体将伤害它的实体作为新目标。


- `alert_same_type`： （布尔值，默认 `false`）如果为 `true`，会使附近的同类型实体警惕。
- `entity_types`：（数组）所有有效的实体类型。

- *（对象）*

- `filters`：（对象）滤器（有关滤器的详细内容请查看下方「滤器」部分）。
- `max_dist`：（整数，默认 16）该实体有效的最远距离。
- `must_see`：（布尔值，默认 `false`）如果为 `true`，该实体必须在视野范围内才有效。
- `sprint_speed_multiplier`：（数字，默认 1.0）奔跑速度的倍数。`1.0` 表明速度未改变。
- `walk_speed_multiplier`：（数字，默认 1.0）行走速度的倍数。`1.0` 表明速度未改变。

- `hurt_owner`：（布尔值，默认 `false`）如果为 `true`，该生物会伤害它的主人，以及其他拥有相同主人的生物。

```
{
    "priority": 1,
    "alert_same_type": true
}
```
### minecraft:behavior.lay_down

允许生物躺下。


- `interval`：（整数，默认 120）随机躺下的几率。有 1 / interval 的几率这么做。
- `random_stop_interval`：（整数，默认 120）随机停止该行为的几率。有 1 / interval 的几率这么做。

```
{
    "priority": 5,
    "interval": 400,
    "random_stop_interval": 2000
}
```
### minecraft:behavior.lay_egg

允许怀孕的生物在沙子上下蛋。


- `goal_radius`：（数字，默认 0.5）实体认为它已达成目标的距离。这是防止实体一直转圈尝试精确到达某一点的「回旋的余地」。
- `on_lay`：（对象，格式同触发器）当实体下蛋时触发。
- `search_count`：（整数，默认 10）该实体每刻所检查的方块数量。如果为 0，该实体会在 1 刻内检查其范围内的所有方块。
- `search_height`：（整数，默认 1）该实体会查找沙子的高度限制。
- `search_range`：（整数，默认 0）该实体会寻找的沙子的距离限制。
- `speed_multiplier`：（数字，默认 1.0）在使用此 AI 目标组件时的移动速度的倍数。`1.0` 表明速度未改变。

```
{
    "priority": 1,
    "speed_multiplier": 1,
    "search_range": 16,
    "search_height": 4,
    "goal_radius": 1,
    "on_lay": {
        "event": "minecraft:laid_egg",
        "target": "self"
    }
}
```
### minecraft:behavior.leap_at_target

允许生物跳起并攻击目标。只能被敌对生物使用。


- `must_be_on_ground`：（布尔值，默认 `true`）如果为 `true`，实体只会在目标在地上时才会起跳。否则即使目标已经在空中，也会起跳。
- `yd`：（数字，默认 0.0）实体跳跃时的高度。

```
{
    "priority": 4,
    "yd": 0.4,
    "must_be_on_ground": false
}
```
### minecraft:behavior.look_at_entity

允许实体看在附近的实体上。


- `angle_of_view_horizontal`：（整数，默认 360）该实体在 Y 轴（上下）中能看到的实体的角度。
- `angle_of_view_vertical`：（整数，默认 360）该实体在 X 轴（左右）中能看到的实体的角度。
- `filters`：（对象）看在实体上的滤器（有关滤器的详细内容请查看下方「滤器」部分）。
- `look_distance`：（数字，默认 8.0）该实体会看向的距离。
- `look_time`：（数组，默认 `[2, 4]`）该实体会看向某实体的时间范围。
- `probability`：（数字，默认 0.02）会看向实体的几率。`1.00` 为 100%。

```
{
    "priority": 9,
    "look_distance": 6,
    "probability": 0.02,
    "filters": {
        "test": "is_family",
        "subject": "other",
        "value": "mob"
    }
}
```
### minecraft:behavior.look_at_player

允许实体看在附近的玩家上。


- `angle_of_view_horizontal`：（整数，默认 360）该实体在 Y 轴（上下）中能看到的生物的角度。
- `angle_of_view_vertical`：（整数，默认 360）该实体在 X 轴（左右）中能看到的生物的角度。
- `look_distance`：（数字，默认 8.0）该实体会看向的距离。
- `look_time`：（数组，默认 `[2, 4]`）该实体会看向某生物的时间范围。
- `probability`：（数字，默认 0.02）会看向生物的几率。`1.00` 为 100%。

```
{
    "priority": 9,
    "look_distance": 6,
    "probability": 0.02
}
```
### minecraft:behavior.look_at_target

允许实体看在它的目标上。


- `angle_of_view_horizontal`：（整数，默认 360）该实体在 Y 轴（上下）中能看到的生物的角度。
- `angle_of_view_vertical`：（整数，默认 360）该实体在 X 轴（左右）中能看到的生物的角度。
- `look_distance`：（数字，默认 8.0）该实体会看向的距离。
- `look_time`：（数组，默认 `[2, 4]`）该实体会看向某生物的时间范围。
- `probability`：（数字，默认 0.02）会看向生物的几率。`1.00` 为 100%。

### minecraft:behavior.look_at_trading_player

允许实体看向正在与它交易的玩家。


- `angle_of_view_horizontal`：（整数，默认 360）该实体在 Y 轴（上下）中能看到的生物的角度。
- `angle_of_view_vertical`：（整数，默认 360）该实体在 X 轴（左右）中能看到的生物的角度。
- `look_distance`：（数字，默认 8.0）该实体会看向的距离。
- `look_time`：（数组，默认 `[2, 4]`）该实体会看向某生物的时间范围。
- `probability`：（数字，默认 0.02）会看向生物的几率。`1.00` 为 100%。

### minecraft:behavior.make_love

允许村民找一个伴侣。只能被村民使用。

### minecraft:behavior.melee_attack

允许实体使用近战攻击。


- `attack_once`：（布尔值，默认 `false`）如果为 `true`，该实体只会攻击一次。
- `attack_types`：（字符串）定义该实体会攻击的实体的类型。
- `melee_fov`：（数字，默认 90.0）该实体攻击时使用的视角。
- `random_stop_interval`：（整数，默认 0）该实体停止攻击的几率。0 为不可能，1 为 50%。
- `reach_multiplier`：（数字，默认 2.0）能使该实体攻击到目标的碰撞箱的倍数。将该值改大可以做出长臂猿的效果。
- `speed_multiplier`：（数字，默认 1.0）在使用此 AI 目标组件时的移动速度的倍数。`1.0` 表明速度未改变。
- `track_target`：（布尔值，默认 `false`）如果为 `true`，只要目标还有效，该实体就会追在目标后面。

```
{
    "priority": 4,
    "target_dist": 1.2,
    "speed_multiplier": 1.25,
    "track_target": true
}
```
### minecraft:behavior.mount_pathing

允许在坐骑寻找目标时，该实体自行移动。


- `speed_multiplier`：（数字，默认 1.0）在使用此 AI 目标组件时的移动速度的倍数。`1.0` 表明速度未改变。
- `target_dist`：（数字，默认 0.0）该实体离目标的距离。
- `track_target`：（布尔值，默认 `false`）如果为 `true`，只要目标还有效，该实体就会追在目标后面。

```
{
    "priority": 5,
    "speed_multiplier": 1.25,
    "target_dist": 0,
    "track_target": true
}
```
### minecraft:behavior.move_indoors

允许村民在室内躲避。只能被村民使用。


- `speed_multiplier`：（数字，默认 1.0）在使用此 AI 目标组件时的移动速度的倍数。`1.0` 表明速度未改变。

```
{
    "priority": 4,
    "speed_multiplier": 0.8
}
```
### minecraft:behavior.move_through_village

允许村民在村庄周围创建路径。只能被村民使用。


- `only_at_night`：（布尔值，默认 `false`）如果为 `true`，该实体只会在夜晚移动。
- `speed_multiplier`：（数字，默认 1.0）在使用此 AI 目标组件时的移动速度的倍数。`1.0` 表明速度未改变。

```
{
    "priority": 3,
    "speed_multiplier": 0.6,
    "only_at_night": true
}
```
### minecraft:behavior.move_to_land

允许实体在水中时移回陆地。


- `goal_radius`：（数字，默认 0.5）实体认为它已达成目标的距离。这是防止实体一直转圈尝试精确到达某一点的「回旋的余地」。
- `search_count`：（整数，默认 10）该实体每刻所检查的方块数量。如果为 0，该实体会在 1 刻内检查其范围内的所有方块。
- `search_height`：（整数，默认 1）该实体会寻找的方块的高度限制。
- `search_range`：（整数，默认 0）该实体会寻找的方块的距离限制。
- `speed_multiplier`：（数字，默认 1.0）在使用此 AI 目标组件时的移动速度的倍数。`1.0` 表明速度未改变。

```
{
    "priority": 6,
    "search_range": 16,
    "search_height": 5,
    "goal_radius": 0.5
}
```
### minecraft:behavior.move_to_water

允许实体在陆地上时移回水中。


- `goal_radius`：（数字，默认 0.5）实体认为它已达成目标的距离。这是防止实体一直转圈尝试精确到达某一点的「回旋的余地」。
- `search_count`：（整数，默认 10）该实体每刻所检查的方块数量。如果为 0，该实体会在 1 刻内检查其范围内的所有方块。
- `search_height`：（整数，默认 1）该实体会寻找的方块的高度限制。
- `search_range`：（整数，默认 0）该实体会寻找的方块的距离限制。
- `speed_multiplier`：（数字，默认 1.0）在使用此 AI 目标组件时的移动速度的倍数。`1.0` 表明速度未改变。

```
{
    "priority": 1,
    "search_range": 15,
    "search_height": 5,
    "goal_radius": 0.1
}
```
### minecraft:behavior.move_towards_restriction

允许守卫者、铁傀儡和村民在它们预先设定的限制区域内移动。其他实体没有定义限制区域。


- `speed_multiplier`：（数字，默认 1.0）在使用此 AI 目标组件时的移动速度的倍数。`1.0` 表明速度未改变。

### minecraft:behavior.move_towards_target

允许该实体直接移向它的目标。


- `within_radius`：（数字，默认 0.0）该实体想要和目标的距离。`0.0` 表明它会尝试和目标占在同一格内。

### minecraft:behavior.nearest_attackable_target

允许该实体检查并追踪最近的有效目标。


- `attack_interval`：（整数，默认 0）攻击之间的间隔秒数。
- `entity_types`：（数组）所有有效的实体类型。

- *（对象）*

- `filters`：（对象）滤器（有关滤器的详细内容请查看下方「滤器」部分）。
- `max_dist`：（整数，默认 16）该实体有效的最远距离。
- `must_see`：（布尔值，默认 `false`）如果为 `true`，该实体必须在视野范围内才有效。
- `sprint_speed_multiplier`：（数字，默认 1.0）奔跑速度的倍数。`1.0` 表明速度未改变。
- `walk_speed_multiplier`：（数字，默认 1.0）行走速度的倍数。`1.0` 表明速度未改变。

- `must_reach`：（布尔值，默认 `false`）如果为 `true`，目标点必须可以到达。
- `must_see`：（布尔值，默认 `false`）如果为 `true`，该实体必须在视野范围内才有效。
- `must_see_forget_duration`：（数字，默认 4.0）决定当该实体离开视野后，选取新的目标前的秒数。
- `persist_time`：（数字，默认 0.0）当一个实体不再符合目标条件后，仍作为目标的秒数。
- `reselect_targets`：（布尔值，默认 `false`）如果为 `true`，该实体会把目标切换为更近的一个实体。
- `scan_interval`：（整数，默认 10）两次寻找目标之间间隔的刻数。
- `target_search_height`：（数字，默认 -1.0）目标实体可在的高度范围。-1.0 表示不限制高度。
- `within_radius`：（数字，默认 0.0）开始攻击目标的距离。

```
{
    "priority": 2,
    "reselect_targets": true,
    "entity_types": [
        {
            "filters": {
                "any_of": [
                    {
                        "test": "is_family",
                        "subject": "other",
                        "value": "player"
                    },
                    {
                        "test": "is_family",
                        "subject": "other",
                        "value": "snowgolem"
                    },
                    {
                        "test": "is_family",
                        "subject": "other",
                        "value": "irongolem"
                    },
                    {
                        "test": "is_family",
                        "subject": "other",
                        "value": "villager"
                    }
                ]
            },
            "max_dist": 35
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
            "max_dist": 35
        }
    ],
    "must_see": true
}
```
### minecraft:behavior.ocelot_sit_on_block

允许实体像豹猫那样坐在一个地方。


- `speed_multiplier`：（数字，默认 1.0）在使用此 AI 目标组件时的移动速度的倍数。`1.0` 表明速度未改变。

### minecraft:behavior.ocelotattack

允许豹猫进行潜行和猛扑攻击。只能被豹猫使用。


- `sneak_speed_multiplier`：（数字，默认 1.0）潜行速度的倍数。`1.0` 表明速度未改变。
- `sprint_speed_multiplier`：（数字，默认 1.0）疾跑速度的倍数。`1.0` 表明速度未改变。
- `walk_speed_multiplier`：（数字，默认 1.0）行走速度的倍数。`1.0` 表明速度未改变。

```
{
    "priority": 6,
    "speed_multiplier": 1
}
```
### minecraft:behavior.offer_flower

允许实体像铁傀儡那样递花。

### minecraft:behavior.open_door

允许实体开门。需要让实体能通过门，否则它不会去尝试开门。


- `close_door_after`：（布尔值，默认 `true`）如果为 `true`，该实体会随手关门。

```
{
    "priority": 6,
    "close_door_after": true
}
```
### minecraft:behavior.owner_hurt_by_target

允许该实体把伤害它主人的实体作为目标。


- `entity_types`：（数组）所有有效的实体类型。

- *（对象）*

- `filters`：（对象）滤器（有关滤器的详细内容请查看下方「滤器」部分）。
- `max_dist`：（整数，默认 16）该实体有效的最远距离。
- `must_see`：（布尔值，默认 `false`）如果为 `true`，该实体必须在视野范围内才有效。
- `sprint_speed_multiplier`：（数字，默认 1.0）奔跑速度的倍数。`1.0` 表明速度未改变。
- `walk_speed_multiplier`：（数字，默认 1.0）行走速度的倍数。`1.0` 表明速度未改变。



### minecraft:behavior.owner_hurt_target

允许该实体把它主人伤害的实体作为目标。


- `entity_types`：（数组）所有有效的实体类型。

- *（对象）*

- `filters`：（对象）滤器（有关滤器的详细内容请查看下方「滤器」部分）。
- `max_dist`：（整数，默认 16）该实体有效的最远距离。
- `must_see`：（布尔值，默认 `false`）如果为 `true`，该实体必须在视野范围内才有效。
- `sprint_speed_multiplier`：（数字，默认 1.0）奔跑速度的倍数。`1.0` 表明速度未改变。
- `walk_speed_multiplier`：（数字，默认 1.0）行走速度的倍数。`1.0` 表明速度未改变。



### minecraft:behavior.panic

允许实体进入恐慌状态。实体将会躲避使它进入恐慌状态的伤害源。


- `force`：（布尔值，默认 `false`）如果为 `true`，该实体不会停止恐慌状态，直到它不能移动或被移除了该 AI 目标组件。
- `prefer_water`：（布尔值，默认 `false`）如果为 `true`，该实体会更愿意躲到水中。
- `speed_multiplier`：（数字，默认 1.0）在使用此 AI 目标组件时的移动速度的倍数。`1.0` 表明速度未改变。

```
{
    "priority": 0,
    "speed_multiplier": 1.25
}
```
### minecraft:behavior.peek

允许该实体窥探。潜影贝使用该 AI 目标组件从壳中往外看。

### minecraft:behavior.pet_sleep_with_owner

允许宠物在主人睡觉时移动到床上。


- `goal_radius`：（数字，默认 0.5）实体认为它已达成目标的距离。这是防止实体一直转圈尝试精确到达某一点的「回旋的余地」。
- `search_height`：（整数，默认 1）该实体会寻找的方块的高度限制。
- `search_range`：（整数，默认 0）该实体会寻找的方块的距离限制。
- `speed_multiplier`：（数字，默认 1.0）在使用此 AI 目标组件时的移动速度的倍数。`1.0` 表明速度未改变。

```
{
    "priority": 2,
    "speed_multiplier": 1.2,
    "search_radius": 10,
    "search_height": 10,
    "goal_radius": 1
}
```
### minecraft:behavior.pickup_items

允许实体捡起地上的物品。


- `goal_radius`：（数字，默认 0.5）实体认为它已达成目标的距离。这是防止实体一直转圈尝试精确到达某一点的「回旋的余地」。
- `max_dist`：（数字，默认 0.0）寻找物品的最大距离。
- `speed_multiplier`：（数字，默认 1.0）在使用此 AI 目标组件时的移动速度的倍数。`1.0` 表明速度未改变。
- `track_target`：（布尔值，默认 `false`）如果为 `true`，只要目标还有效，该实体就会追在目标后面。

```
{
    "priority": 9,
    "max_dist": 3,
    "goal_radius": 2,
    "speed_multiplier": 0.5
}
```
### minecraft:behavior.play

允许村民与其他幼儿村民玩耍。只能被村民使用。


- `speed_multiplier`：（数字，默认 1.0）在使用此 AI 目标组件时的移动速度的倍数。`1.0` 表明速度未改变。

```
{
    "priority": 8,
    "speed_multiplier": 0.32
}
```
### minecraft:behavior.player_ride_tamed

允许该实体被驯服以后能被玩家骑乘。

### minecraft:behavior.raid_garden

允许实体吃掉农场中的农作物，直到饱了。


- `blocks`：（数组）该实体要吃的方块。

- *（字符串）一个方块*
- `eat_delay`：（整数，默认 2）在两次吃之间间隔的秒数。
- `full_delay`：（整数，默认 100）该实体想再吃之前的秒数。
- `goal_radius`：（数字，默认 0.5）实体认为它已达成目标的距离。这是防止实体一直转圈尝试精确到达某一点的「回旋的余地」。
- `max_to_eat`：（整数，默认 6）该实体想吃的东西的最大数量。
- `search_range`：（整数，默认 0）该实体会寻找的方块的距离限制。
- `speed_multiplier`：（数字，默认 1.0）在使用此 AI 目标组件时的移动速度的倍数。`1.0` 表明速度未改变。

```
{
    "priority": 5,
    "blocks": [
        "carrots"
    ],
    "search_range": 16,
    "goal_radius": 0.8
}
```
### minecraft:behavior.random_breach

允许实体随机跳出水面。


- `cooldown_time`：（数字，默认 0.0）在再次使用此 AI 目标组件前间隔的秒数。
- `interval`：（整数，默认 120）随机移向某处的几率。有 1 / interval 的几率这么做。
- `speed_multiplier`：（数字，默认 1.0）在使用此 AI 目标组件时的移动速度的倍数。`1.0` 表明速度未改变。
- `xz_dist`：（整数，默认 10）实体寻找地面上的新目标点的距离。必须至少为 1。
- `y_dist`：（整数，默认 7）实体向上/下寻找的新目标点的距离。必须至少为 1。

```
{
    "priority": 6,
    "interval": 50,
    "xz_dist": 6,
    "cooldown_time": 2
}
```
### minecraft:behavior.random_fly

允许实体随机飞行。


- `can_land_on_trees`：（布尔值，默认 `true`）如果为 `true`，实体可以停止飞行并停留在树上，而不是地上。
- `xz_dist`：（整数，默认 10）实体寻找地面上的新目标点的距离。必须至少为 1。
- `y_dist`：（整数，默认 7）实体向上/下寻找的新目标点的距离。必须至少为 1。

```
{
    "priority": 2,
    "xz_dist": 15,
    "y_dist": 1,
    "y_offset": 0,
    "speed_multiplier": 1,
    "can_land_on_trees": true,
    "avoid_damage_blocks": true
}
```
### minecraft:behavior.random_look_around

允许实体随机看向四周。


- `look_time`：（数组，默认 `[2, 4]`）实体会看向随机方向的时间所处的范围，单位为秒。

```
{
    "priority": 3,
    "look_distance": 8
}
```
### minecraft:behavior.random_sitting

允许实体随机坐一会儿。


- `cooldown`：（数字，默认 100.0）在再次坐下前间隔的秒数。
- `min_sit_time`：（数字，默认 10.0）在实体站起来前的最少秒数。
- `start_chance`：（数字，默认 0.1）实体开始坐的几率，取值需要在 [0, 1] 之间。
- `stop_chance`：（数字，默认 0.1）实体停止坐的几率，取值需要在 [0, 1] 之间。

```
{
    "priority": 5,
    "start_chance": 0.1,
    "stop_chance": 0.3,
    "cooldown": 30,
    "min_sit_time": 10
}
```
### minecraft:behavior.random_stroll

允许实体随机溜达。


- `interval`：（整数，默认 120）随机移向某处的几率。有 1 / interval 的几率这么做。
- `speed_multiplier`：（数字，默认 1.0）在使用此 AI 目标组件时的移动速度的倍数。`1.0` 表明速度未改变。
- `xz_dist`：（整数，默认 10）实体寻找地面上的新目标点的距离。必须至少为 1。
- `y_dist`：（整数，默认 7）实体向上/下寻找的新目标点的距离。必须至少为 1。

```
{
    "priority": 6,
    "speed_multiplier": 1
}
```
### minecraft:behavior.random_swim

允许实体随机在水下移动。


- `interval`：（整数，默认 120）随机移向某处的几率。有 1 / interval 的几率这么做。
- `speed_multiplier`：（数字，默认 1.0）在使用此 AI 目标组件时的移动速度的倍数。`1.0` 表明速度未改变。
- `xz_dist`：（整数，默认 10）实体寻找地面上的新目标点的距离。必须至少为 1。
- `y_dist`：（整数，默认 7）实体向上/下寻找的新目标点的距离。必须至少为 1。

```
{
    "priority": 7,
    "interval": 0,
    "xz_dist": 30,
    "y_dist": 15
}
```
### minecraft:behavior.ranged_attack

允许实体使用远程攻击，例如射箭。


- `attack_interval_max`：（整数，默认 0）实体在再一次攻击前等待的最大秒数。
- `attack_interval_min`：（整数，默认 0）实体在再一次攻击前等待的最小秒数。
- `attack_radius`：（数字，默认 0.0）目标被攻击的最大距离。如果超出这个距离，实体会先位移。
- `burst_interval`：（数字，默认 0.0）每次攻击发射多次时，两次射击见间隔的秒数。
- `burst_shots`：（整数，默认 1）实体每次蓄力攻击时发射的次数。
- `charge_charged_trigger`：（数字，默认 0.0）实体在发起蓄力攻击前等待的最小刻数。
- `charge_shoot_trigger`：（数字，默认 0.0）实体在开始蓄力前等待的最小刻数。必须大于 0 来启用 `burst_shots`。
- `ranged_fov`：（数字，默认 90.0）该实体攻击时使用的视角。
- `speed_multiplier`：（数字，默认 1.0）在使用此 AI 目标组件时的移动速度的倍数。`1.0` 表明速度未改变。

```
{
    "priority": 2,
    "speed_multiplier": 1,
    "attack_interval_min": 3,
    "attack_interval_max": 3,
    "attack_radius": 10
}
```
### minecraft:behavior.receive_love

允许村民站住等待和另一个村民繁殖。只能被村民使用。

### minecraft:behavior.restrict_open_door

允许实体在夜间躲在室内。

### minecraft:behavior.restrict_sun

允许实体在日出后自动躲避阳光。

### minecraft:behavior.roll

允许实体随机向前。


- `probability`：（数字，默认 1.0）实体使用此 AI 目标组件的几率。

```
{
    "priority": 12,
    "probability": 0.013
}
```
### minecraft:behavior.run_around_like_crazy

允许实体漫无目的地跑。


- `speed_multiplier`：（数字，默认 1.0）在使用此 AI 目标组件时的移动速度的倍数。`1.0` 表明速度未改变。

```
{
    "priority": 1,
    "speed_multiplier": 1.2
}
```
### minecraft:behavior.send_event

允许实体向另一个实体发送事件（有关事件的具体内容见下方介绍）。


- `cast_duration`：（数字，默认为所有步骤的总时长）整个事件被发送的秒数。
- `sequence`：（数组）要发送的所有事件。

- *（对象）*

- `base_delay`：（数字，默认 0.0）开始此步骤前等待的秒数。
- `event`：（字符串）发送给指定实体的事件。
- `sound_event`：（字符串）这一步发生时播放的声音事件。



```
{
    "priority": 3,
    "event_choices": [
        {
            "min_activation_range": 0,
            "max_activation_range": 16,
            "cooldown_time": 5,
            "cast_duration": 3,
            "particle_color": "#FFB38033",
            "weight": 3,
            "filters": {
                "all_of": [
                    {
                        "test": "is_family",
                        "subject": "other",
                        "value": "sheep"
                    },
                    {
                        "test": "is_color",
                        "subject": "other",
                        "value": "blue"
                    }
                ]
            },
            "start_sound_event": "cast.spell",
            "sequence": [
                {
                    "base_delay": 2,
                    "event": "wololo",
                    "sound_event": "prepare.wololo"
                }
            ]
        }
    ]
}
```
### minecraft:behavior.share_items

允许实体将它有的物品给予其他生物。


- `entity_types`：（数组）所有有效的实体类型。

- *（对象）*

- `filters`：（对象）滤器（有关滤器的详细内容请查看下方「滤器」部分）。
- `max_dist`：（整数，默认 16）该实体有效的最远距离。
- `must_see`：（布尔值，默认 `false`）如果为 `true`，该实体必须在视野范围内才有效。
- `sprint_speed_multiplier`：（数字，默认 1.0）奔跑速度的倍数。`1.0` 表明速度未改变。
- `walk_speed_multiplier`：（数字，默认 1.0）行走速度的倍数。`1.0` 表明速度未改变。

- `goal_radius`：（数字，默认 0.5）实体认为它已达成目标的距离。这是防止实体一直转圈尝试精确到达某一点的「回旋的余地」。
- `max_dist`：（数字，默认 0.0）寻找实体的最大距离。
- `speed_multiplier`：（数字，默认 1.0）在使用此 AI 目标组件时的移动速度的倍数。`1.0` 表明速度未改变。

```
{
    "priority": 8,
    "max_dist": 3,
    "goal_radius": 2,
    "speed_multiplier": 0.5,
    "entity_types": [
        {
            "filters": {
                "test": "is_family",
                "subject": "other",
                "value": "villager"
            }
        }
    ]
}
```
### minecraft:behavior.silverfish_merge_with_stone

允许实体想蠹鱼那样钻进方块。目前只能被蠹鱼使用。

### minecraft:behavior.silverfish_wake_up_friends

允许实体让附近方块里面的实体出来。目前只能被蠹鱼使用。

### minecraft:behavior.skeleton_horse_trap

允许马群成为马陷阱。当有玩家在附近时会生成闪电和许多马。只能被马、驴、骡子和骷髅马使用。


- `duration`：（数字，默认 1.0）该陷阱存在的秒数。当此时间过去以后，该陷阱若没有被激活将被从世界中移除。
- `within_radius`：（数字，默认 0.0）玩家触发此陷阱的最远距离。

```
{
    "within_radius": 10,
    "duration": 900,
    "priority": 2
}
```
### minecraft:behavior.slime_attack

允许实体像史莱姆那样进行近战攻击。只能被史莱姆或岩浆怪使用。

### minecraft:behavior.slime_float

允许实体浮在熔岩或水里。只能被史莱姆或岩浆怪使用。

### minecraft:behavior.slime_keep_on_jumping

允许实体持续跳跃。只能被史莱姆或岩浆怪使用。

### minecraft:behavior.slime_random_direction

允许实体随机移动。只能被史莱姆或岩浆怪使用。

### minecraft:behavior.snacking

允许该实体吃它在附近找到的食品。


- `items`：（数组）该实体感兴趣的食物列表。

- *（字符串）一个物品*
- `snacking_cooldown`：（数字，默认 7.5）该实体再次吃食前的冷却秒数。
- `snacking_cooldown_min`：（数字，默认 0.5）该实体再次吃食前的最小冷却秒数。
- `snacking_stop_chance`：（数字，默认 0.0017）该实体停止吃食的几率，取值范围在 [0, 1] 之中。

```
{
    "priority": 3,
    "snacking_cooldown": 22.5,
    "snacking_cooldown_min": 20,
    "snacking_stop_chance": 0.001334,
    "items": [
        "bamboo",
        "cake"
    ]
}
```
### minecraft:behavior.sneeze

允许该生物停下并打喷嚏。可能会丢下一个物品，并吓到附近生物。


- `cooldown_time`：（数字，默认 0.0）在再次使用此 AI 目标组件前间隔的秒数。
- `drop_item_chance`：（数字，默认 1.0）掉落物品的几率。
- `entity_types`：（数组）当打喷嚏时会被吓到（跳起来）的实体列表。

- *（对象）*

- `filters`：（对象）滤器（有关滤器的详细内容请查看下方「滤器」部分）。
- `max_dist`：（整数，默认 16）该实体有效的最远距离。
- `must_see`：（布尔值，默认 `false`）如果为 `true`，该实体必须在视野范围内才有效。
- `sprint_speed_multiplier`：（数字，默认 1.0）奔跑速度的倍数。`1.0` 表明速度未改变。
- `walk_speed_multiplier`：（数字，默认 1.0）行走速度的倍数。`1.0` 表明速度未改变。

- `loot_table`：（字符串）物品所使用的战利品表。
- `prepare_sound`：（字符串）当要打喷嚏时播放的音效。
- `prepare_time`：（数字，默认 1.0）打喷嚏前等待的时间（与此同时在播放 `prepare_sound`）。
- `probability`：（数字，默认 0.02）打喷嚏的几率。`1.00` 为 100%。
- `sound`：（字符串）打喷嚏时播放的音效。
- `within_radius`：（数字，默认 0.0）会被吓到的实体的最远距离。

```
{
    "priority": 7,
    "probability": 0.002,
    "cooldown_time": 1,
    "within_radius": 10,
    "entity_types": [
        {
            "filters": {
                "all_of": [
                    {
                        "test": "has_component",
                        "subject": "other",
                        "operator": "!=",
                        "value": "minecraft:is_baby"
                    },
                    {
                        "test": "is_family",
                        "subject": "other",
                        "value": "panda"
                    },
                    {
                        "test": "in_water",
                        "subject": "other",
                        "operator": "!=",
                        "value": true
                    },
                    {
                        "test": "on_ground",
                        "operator": "==",
                        "value": true
                    }
                ]
            },
            "max_dist": 10
        }
    ],
    "drop_item_chance": 0.001,
    "loot_table": "loot_tables/entities/panda_sneeze.json",
    "prepare_sound": "presneeze",
    "prepare_time": 1,
    "sound": "sneeze"
}
```
### minecraft:behavior.squid_dive

允许鱿鱼潜入水底。只能被鱿鱼使用。

### minecraft:behavior.squid_flee

允许鱿鱼游走。只能被鱿鱼使用。

### minecraft:behavior.squid_idle

允许鱿鱼悠闲地游泳。只能被鱿鱼使用。

### minecraft:behavior.squid_move_away_from_ground

允许鱿鱼从地面回到水中。只能被鱿鱼使用。

### minecraft:behavior.squid_out_of_water

允许鱿鱼在水外时贴在地面。只能被鱿鱼使用。

### minecraft:behavior.stay_while_sitting

允许实体在坐姿状态时保持不动，而不是做别的。

### minecraft:behavior.stomp_attack

允许实体使用北极熊的近战攻击。


- `attack_once`：（布尔值，默认 `false`）如果为 `true`，该实体只会攻击一次。
- `attack_types`：（字符串）定义该实体会攻击的实体的类型。
- `random_stop_interval`：（整数，默认 0）该实体停止攻击的几率。0 为不可能，1 为 50%。
- `reach_multiplier`：（数字，默认 2.0）能使该实体攻击到目标的碰撞箱的倍数。将该值改大可以做出长臂猿的效果。
- `speed_multiplier`：（数字，默认 1.0）在使用此 AI 目标组件时的移动速度的倍数。`1.0` 表明速度未改变。
- `track_target`：（布尔值，默认 `false`）如果为 `true`，只要目标还有效，该实体就会追在目标后面。

### minecraft:behavior.stomp_turtle_egg

允许实体踩爆海龟蛋。


- `goal_radius`：（数字，默认 0.5）实体认为它已达成目标的距离。这是防止实体一直转圈尝试精确到达某一点的「回旋的余地」。
- `interval`：（整数，默认 120）随机移向某处的几率。有 1 / interval 的几率这么做。
- `search_count`：（整数，默认 10）该实体每刻所检查的方块数量。如果为 0，该实体会在 1 刻内检查其范围内的所有方块。
- `search_height`：（整数，默认 1）该实体会寻找的方块的高度限制。
- `search_range`：（整数，默认 0）该实体会寻找的方块的距离限制。
- `speed_multiplier`：（数字，默认 1.0）在使用此 AI 目标组件时的移动速度的倍数。`1.0` 表明速度未改变。

```
{
    "priority": 4,
    "speed_multiplier": 1,
    "search_range": 24,
    "search_height": 3,
    "goal_radius": 1.14,
    "interval": 20
}
```
### minecraft:behavior.summon_entity

允许实体通过召唤其他实体来攻击玩家。


- `summon_choices`：（数组）生成的所有实体。

- *（对象）*

- `cast_duration`：（数字，默认为所有步骤的总时长）施法消耗的秒数。
- `cooldown_time`：（数字，默认 0.0）实体再次施法前等待的秒数。
- `do_casting`：（布尔值，默认 `true`）如果为 `true`，会有生成动画并带有粒子效果。
- `filters`：（对象）滤器（有关滤器的详细内容请查看下方「滤器」部分）。
- `max_activation_range`：（数字，默认 -1.0）激活施法的最大距离。
- `min_activation_range`：（数字，默认 1.0）激活施法的最小距离。
- `particle_color`：（整数，默认 0）此法术的粒子效果颜色。
- `sequence`：（数组）此法术的所有步骤。

- *（对象）*

- `base_delay`：（数字，默认 0.0）开始此步骤前等待的秒数。
- `delay_per_summon`：（数字，默认 0.0）此步骤中每一次生成实体前的秒数。
- `entity_lifespan`：（数字，默认 -1.0）生成的实体的存活时间。`-1.0` 表示实体会一直活到它能活到的时候。
- `entity_type`：（字符串）这一步中生成的实体的类型。
- `num_entities_spawned`：（整数，默认 1）这一步中生成的实体的数量。
- `shape`：（字符串，默认 `line`）这一步生成的实体所组成的形状。可选值为 `line`（线）和 `circle`（圆）。
- `size`：（数字，默认 1.0）该实体的基础大小。
- `sound_event`：（字符串）这一步所播放的声音事件。
- `summon_cap`：（整数）任一时间生成的实体的最大数量。
- `summon_cap_radius`：（数字，默认 0.0）检测实体生成最大数量的半径。
- `target`：（字符串，默认为 `self`）此法术的目标，将作为法术的起始位置（`shape` 如果为 `line` 将以这里作为起点，若为 `circle` 将以这里作为圆心）。

- `start_sound_event`：（字符串）使用此法术时播放的声音事件。
- `weight`：（数字，默认 0.0）此法术的权重。控制该实体在生成实体时选择此法术的可能性。



```
{
    "priority": 2,
    "summon_choices": [
        {
            "min_activation_range": 0,
            "max_activation_range": 3,
            "cooldown_time": 5,
            "weight": 3,
            "cast_duration": 2,
            "particle_color": "#FF664D59",
            "start_sound_event": "cast.spell",
            "sequence": [
                {
                    "shape": "circle",
                    "target": "self",
                    "base_delay": 1,
                    "delay_per_summon": 0,
                    "num_entities_spawned": 5,
                    "entity_type": "minecraft:evocation_fang",
                    "size": 1.5,
                    "entity_lifespan": 1.1,
                    "sound_event": "prepare.attack"
                },
                {
                    "shape": "circle",
                    "target": "self",
                    "base_delay": 0.15,
                    "delay_per_summon": 0,
                    "num_entities_spawned": 8,
                    "entity_type": "minecraft:evocation_fang",
                    "size": 2.5,
                    "entity_lifespan": 1.1
                }
            ]
        },
        {
            "min_activation_range": 3,
            "weight": 3,
            "cooldown_time": 5,
            "cast_duration": 2,
            "particle_color": "#FF664D59",
            "start_sound_event": "cast.spell",
            "sequence": [
                {
                    "shape": "line",
                    "target": "self",
                    "base_delay": 1,
                    "delay_per_summon": 0.05,
                    "num_entities_spawned": 16,
                    "entity_type": "minecraft:evocation_fang",
                    "size": 20,
                    "entity_lifespan": 1.1
                }
            ]
        },
        {
            "weight": 1,
            "cooldown_time": 17,
            "cast_duration": 5,
            "particle_color": "#FFB3B3CC",
            "sequence": [
                {
                    "shape": "circle",
                    "target": "self",
                    "base_delay": 5,
                    "num_entities_spawned": 3,
                    "entity_type": "minecraft:vex",
                    "summon_cap": 8,
                    "summon_cap_radius": 16,
                    "size": 1,
                    "sound_event": "prepare.summon"
                }
            ]
        }
    ]
}
```
### minecraft:behavior.swell

允许爬行者在附近有玩家时膨胀。只能被爬行者使用。


- `start_distance`：（数字，默认 10.0）当目标在至少此距离以内时，该实体开始膨胀。
- `stop_distance`：（数字，默认 2.0）当目标至少移出此距离时，该实体停止膨胀。

```
{
    "start_distance": 2.5,
    "stop_distance": 6,
    "priority": 2
}
```
### minecraft:behavior.swim_wander

当不能寻路时有鱼环绕。


- `speed_multiplier`：（数字，默认 10.0）实体的速度。

```
{
    "priority": 4,
    "speed_multiplier": 1,
    "interval": 10,
    "look_ahead": 2
}
```
### minecraft:behavior.swoop_attack

允许实体通过移动攻击目标。如果在水平方向上碰撞，此 AI 目标将会停止。用于为飞行实体使用。


- `delay_range`：（数组，默认 `[10.0, 20.0]`）再次使用此 AI 目标前的秒数。
- `speed_multiplier`：（数字，默认 1.0）在使用此 AI 目标组件时的移动速度的倍数。`1.0` 表明速度未改变。

```
{
    "priority": 2,
    "delay_range": [ 10, 20 ]
}
```
### minecraft:behavior.take_flower

允许该实体从铁傀儡处接受花朵。只能被村民使用。

### minecraft:behavior.tempt

允许该实体被它感兴趣的物品诱惑。


- `can_get_scared`：（布尔值，默认 `false`）如果为 `true`，该实体会在玩家快速接近它时不再被诱惑。
- `items`：（数组）该实体感兴趣的物品列表。

- *（字符串）一个物品*
- `speed_multiplier`：（数字，默认 1.0）在使用此 AI 目标组件时的移动速度的倍数。`1.0` 表明速度未改变。
- `within_radius`：（数字，默认 0.0）该实体会被玩家诱惑的最远距离。

```
{
    "priority": 3,
    "speed_multiplier": 1,
    "items": [
        "golden_carrot",
        "carrot",
        "yellow_flower"
    ]
}
```
### minecraft:behavior.trade_with_player

允许玩家与此实体交易。

### minecraft:behavior.vex_copy_owner_target

允许恼鬼将目标设置为与其主人相同的目标。


- `entity_types`：（数组）恼鬼可以从主人复制的实体列表。

- *（对象）*

- `filters`：（对象）滤器（有关滤器的详细内容请查看下方「滤器」部分）。
- `max_dist`：（整数，默认 16）该实体有效的最远距离。
- `must_see`：（布尔值，默认 `false`）如果为 `true`，该实体必须在视野范围内才有效。
- `sprint_speed_multiplier`：（数字，默认 1.0）奔跑速度的倍数。`1.0` 表明速度未改变。
- `walk_speed_multiplier`：（数字，默认 1.0）行走速度的倍数。`1.0` 表明速度未改变。



### minecraft:behavior.vex_random_move

允许凋零像恼鬼一样随机移动。

### minecraft:behavior.wither_random_attack_pos_goal

允许凋零随机攻击。只能被凋零使用。

### minecraft:behavior.wither_target_highest_damage

允许凋零攻击对它造成伤害最大的实体。


- `entity_types`：（数组）所有凋零要检查的实体。凋零会从中挑出对它造成伤害最大的实体。

- *（对象）*

- `filters`：（对象）滤器（有关滤器的详细内容请查看下方「滤器」部分）。
- `max_dist`：（整数，默认 16）该实体有效的最远距离。
- `must_see`：（布尔值，默认 `false`）如果为 `true`，该实体必须在视野范围内才有效。
- `sprint_speed_multiplier`：（数字，默认 1.0）奔跑速度的倍数。`1.0` 表明速度未改变。
- `walk_speed_multiplier`：（数字，默认 1.0）行走速度的倍数。`1.0` 表明速度未改变。




### 其他组件


### minecraft:addrider

为该实体添加一个骑乘者。需要有 `minecraft:rideable`。


- `entity_type`：（字符串）骑乘该实体的实体类型。

```
{
    "entity_type": "minecraft:skeleton"
}
```
### minecraft:ageable

为该实体添加一个用于成长的计时器。可以通过喂食 `feedItems` 中定义的物品来加速生长。


- `drop_items`：（数组）当实体生长时掉落的物品。

- *（字符串）一个物品*
- `duration`：（数字，默认 1200.0）实体成长前的时间。
- `feedItems`：（数组）可以促进生长的物品列表。

- *（对象）一个物品*

- `item`：（字符串）物品名。
- `growth`：（数字）促进的时间。

- `grow_up`：（对象，格式同触发器）当长大时触发。

```
{
    "duration": 1200,
    "feedItems": [
        "chicken",
        "cooked_chicken",
        "beef",
        "cooked_beef",
        "muttonRaw",
        "muttonCooked",
        "porkchop",
        "cooked_porkchop",
        "rabbit",
        "cooked_rabbit",
        "rotten_flesh"
    ],
    "grow_up": {
        "event": "minecraft:ageable_grow_up",
        "target": "self"
    }
}
{
    "duration": 1200,
    "grow_up": {
        "event": "minecraft:ageable_grow_up",
        "target": "self"
    }
}
```
### minecraft:angry

通过计时器定义实体的「愤怒」状态。


- `broadcastAnger`：（布尔值，默认 `false`）如果为 `true`，其他相同并且有 `broadcastAnger` 的实体也会愤怒。
- `broadcastRange`：（整数，默认 20）也会愤怒的实体的最大距离。
- `calm_event`：（对象，格式同触发器）当实体不再愤怒时触发。
- `duration`：（整数，默认 25）该实体会保持愤怒的秒数。
- `duration_delta`：（整数，默认 0）添加到 `duration` 的变化量：[-delta, delta]。

```
{
    "duration": 25,
    "broadcastAnger": true,
    "broadcastRange": 20,
    "calm_event": {
        "event": "minecraft:on_calm",
        "target": "self"
    }
}
```
### minecraft:boostable

定义可骑乘实体的提升。


- `feedItems`：（数组）可以使该实体提升的物品列表。

- *（对象）一个物品*

- `damage`：（整数，默认 1）每次使用时消耗的物品耐久。
- `item`：（字符串）物品名。
- `replaceItem`：（字符串）替换的物品名。当原物品耐久耗尽以后，会替换为这个物品（参考骑乘猪的胡萝卜钓竿变钓鱼竿）。

- `duration`：（整数，默认 3）该提升的持续时间。
- `speed_multiplier`：（数字，默认 1.0）在使用此组件时的移动速度的倍数。`1.0` 表明速度未改变，`2.0` 表明二倍，诸如此类。

```
{
    "speed_multiplier": 2,
    "duration": 3,
    "boost_items": [
        {
            "item": "carrotOnAStick",
            "item_damage": 2,
            "replaceItem": "fishing_rod"
        }
    ]
}
```
### minecraft:breathable

定义该实体能够在其中呼吸/窒息的方块。


- `breatheBlocks`：（数组）该实体能在其中呼吸的方块。

- *（字符串）一个方块*
- `breathesAir`：（布尔值，默认为 `true`）如果为 `true`，该实体能够在空气中呼吸。
- `breathesLava`：（布尔值，默认为 `false`）如果为 `true`，该实体能够在熔岩中呼吸。
- `breathesSolids`：（布尔值，默认为 `false`）如果为 `true`，该实体能够在固体方块中呼吸。
- `breathesWater`：（布尔值，默认为 `false`）如果为 `true`，该实体能够在水中呼吸。
- `generatesBubbles`：（布尔值，默认为 `true`）如果为 `true`，该实体在水中时会产生气泡。
- `inhaleTime`：（数字，默认为 0.0）回复满呼吸值的秒数。
- `nonBreatheBlocks`：（数组）该实体不能能在其中呼吸的方块。

- *（字符串）一个方块*
- `suffocateTime`：（整数，默认为 -20）在窒息伤害之间的秒数。
- `totalSupply`：（整数，默认为 15）实体能屏住呼吸的秒数。

```
{
    "totalSupply": 15,
    "suffocateTime": 0,
    "breathesWater": true
}
```
### minecraft:breedable

定义实体进入「求爱」状态的方式。


- `allowSitting`：（布尔值，默认为 `true`）如果为 `true`，该实体能在坐着的时候繁殖。
- `blend_attributes`：（布尔值，默认为 `true`）如果为 `true`，该实体会将性状融合到后代当中。例如，马在子代中融合了它们的血量、运动和跳跃力量。
- `breedCooldown`：（数字，默认 60.0）该实体能再次繁殖的秒数。
- `breedItems`：（数组）能使实体进入求爱状态的物品。

- *（字符串）一个物品*
- `breedsWith`：（数组）能够和该实体繁殖的生物。

- *（对象）*

- `mateType`：（字符串）配偶的实体类型。
- `babyType`：（字符串）子代的实体类型。
- `breed_event`：（对象，格式同触发器）繁殖时触发。

- `causes_pregnancy`：（布尔值，默认为 `false`）如果为 `true`，该实体会怀孕，而不是立刻生下子代。
- `environment_requirements`：（数组）对附近的环境要求。

- *（对象）*

- `block`：（字符串）要求的方块类型。
- `count`：（整数）要求的方块最低数量。
- `radius`：（整数）检查的半径，取值应在 [0, 16] 之间。

- `extraBabyChance`：（数字，默认 0.0）出生多个生物的几率，最多出生 16 个。取值应在 [0.0, 1.0] 之间。
- `inheritTamed`：（布尔值，默认为 `true`）如果为 `true`，在父母已被驯服时，子代会被自动驯服。
- `mutation_factor`：（对象）变异。

- `color`：（数字。默认 0.0）实体颜色变异的几率。取值应在 [0.0, 1.0] 之间。
- `extra_variant`：（数字。默认 0.0）实体拓展变种变异的几率。取值应在 [0.0, 1.0] 之间。
- `variant`：（数字。默认 0.0）实体变种变异的几率。取值应在 [0.0, 1.0] 之间。
- `requireTame`：（布尔值，默认为 `true`）如果为 `true`，该实体在被繁殖前需要被驯服。

```
{
    "requireTame": true,
    "breedsWith": {
        "mateType": "minecraft:wolf",
        "babyType": "minecraft:wolf",
        "breed_event": {
            "event": "minecraft:entity_born",
            "target": "baby"
        }
    },
    "breedItems": [
        "chicken",
        "cooked_chicken",
        "beef",
        "cooked_beef",
        "muttonRaw",
        "muttonCooked",
        "porkchop",
        "cooked_porkchop",
        "rabbit",
        "cooked_rabbit",
        "rotten_flesh"
    ]
}
```
### minecraft:bribeable

定义一个实体进入「行贿」状态的方式。


- `bribe_cooldown`：（数字，默认 60.0）该实体能再次被贿赂的秒数。
- `bribe_items`：（数组）能用来行贿的物品。

- *（字符串）一个物品*


```
{
    "bribe_items": [
        "fish",
        "salmon"
    ]
}
```
### minecraft:damage_over_time

使实体每隔指定时间后受到指定伤害。


- `damagePerHurt`：（整数，默认 1）每次伤害的伤害量。
- `timeBetweenHurt`：（整数，默认 0）两次伤害的间隔秒数。

```
{
    "damagePerHurt": 1,
    "timeBetweenHurt": 0
}
```
### minecraft:damage_sensor

当实体被指定物品或实体伤害时执行的事件。


- `cause`：（字符串）能够执行该事件的伤害的类型。已知值有：`all`。
- `deals_damage`：（布尔值，默认为 `true`）如果为 `true`，该实体会受到该伤害；设置为 `false` 会让实体忽略该伤害。
- `on_damage`：（字符串）要执行的事件（有关事件的具体内容见下方介绍）。可以使用针对实体的滤器（有关滤器的具体内容见下方介绍）。
- `on_damage`：（数组）要执行的事件列表。

- *（字符串）要执行的事件（有关事件的具体内容见下方介绍）*


```
[
    {
        "on_damage": {
            "filters": {
                "test": "is_family",
                "subject": "other",
                "value": "lightning"
            }
        },
        "deals_damage": false
    }
]
[
    {
        "on_damage": {
            "filters": {
                "test": "is_family",
                "subject": "other",
                "value": "lightning"
            },
            "event": "become_witch"
        },
        "deals_damage": false
    },
    {
        "on_damage": {
            "filters": {
                "any_of": [
                    {
                        "test": "is_family",
                        "subject": "other",
                        "value": "zombie"
                    },
                    {
                        "test": "is_family",
                        "subject": "other",
                        "value": "husk"
                    }
                ],
                "all_of": [
                    {
                        "test": "has_damage",
                        "value": "fatal"
                    }
                ]
            },
            "event": "become_zombie"
        }
    }
]
```
### minecraft:environment_sensor

创建一个基于环境条件的感应器。


- `on_environment`：（数组）

- *（对象，格式同触发器）*


```

{
    "filters": {
        "test": "is_underwater",
        "operator": "==",
        "value": true
    },
    "event": "minecraft:start_transforming"
}
{
    "on_environment": {
        "filters": {
            "test": "is_underwater",
            "subject": "self",
            "operator": "==",
            "value": true
        },
        "event": "minecraft:melee_mode"
    }
}
[
    {
        "filters": {
            "test": "in_water",
            "operator": "==",
            "value": true
        },
        "event": "minecraft:navigation_off_land"
    },
    {
        "filters": {
            "test": "in_water_or_rain",
            "operator": "!=",
            "value": true
        },
        "event": "minecraft:start_dryingout"
    }
]
```
### minecraft:equippable

允许实体装备物品。


- `slots`：（数组）可以装备物品的栏位。

- *（对象）*

- `accepted_items`：（数组）可以放到该栏位的所有物品。

- *（字符串）一个物品*
- `interact_text`：（字符串）触屏模式下与该实体交互时显示的文本。
- `item`：（字符串）可以放到该栏位的物品名。
- `on_equip`：（对象，格式同触发器）当该栏位被装备物品时触发。
- `on_unequip`：（对象，格式同触发器）当该栏位被取下物品时触发。
- `slot`：（整数）该栏位的 ID。



```
{
    "slots": [
        {
            "slot": 1,
            "item": "carpet",
            "accepted_items": [
                "carpet"
            ]
        }
    ]
}
{
    "slots": [
        {
            "slot": 0,
            "item": "saddle",
            "accepted_items": [
                "saddle"
            ],
            "on_equip": {
                "event": "minecraft:mule_saddled"
            },
            "on_unequip": {
                "event": "minecraft:mule_unsaddled"
            }
        }
    ]
}
```
### minecraft:explode

定义该实体的爆炸方式。


- `breaks_blocks`：（布尔值，默认为 `true`）如果为 `true`，该爆炸会摧毁爆炸半径内的方块。
- `causesFire`：（布尔值，默认为 `false`）如果为 `true`，爆炸半径内的方块会被点燃。
- `destroyAffectedByGriefing`：（布尔值，默认为 `false`）如果为 `true`，是否破坏方块会受到游戏规则 `mobGriefing` 的影响。
- `fireAffectedByGriefing`：（布尔值，默认为 `false`）如果为 `true`，是否产生火会受到游戏规则 `mobGriefing` 的影响。
- `fuseLength`：（数组，默认为 `[0.0， 0.0]`）自点燃后爆炸前的时间范围。负数会使其立刻爆炸。
- `fuseLit`：（布尔值，默认为 `false`）如果为 `true`，当该组件被添加时立刻点燃。
- `maxResistance`：（数字，默认为 `Infinite`）爆炸抗性比这个值低的方块会被破坏。
- `power`：（数字，默认为 `4.0`）爆炸半径，及爆炸的伤害。

```
{
    "fuseLength": 0,
    "fuseLit": true,
    "power": 1,
    "causesFire": false,
    "maxResistance": 4,
    "destroyAffectedByGriefing": true
}
```
### minecraft:healable

定义治疗该实体的交互。


- `filters`：（对象）该触发器的滤器（有关滤器的具体内容见下方介绍）。
- `force_use`：（布尔值，默认为 `false`）定义物品能否在该实体血量为满时使用。
- `items`：（数组）能够治疗此实体的物品。

- *（对象）*

- `filters`：（对象）应用治疗效果前的滤器（有关滤器的具体内容见下方介绍）。
- `heal_amount`：（数字，默认 1.0）使用此物品时对实体的治疗量。
- `item`：（字符串）能够治疗此实体的物品。



```
{
    "items": [
        {
            "item": "wheat",
            "heal_amount": 2
        },
        {
            "item": "sugar",
            "heal_amount": 1
        },
        {
            "item": "tile.hay_block",
            "heal_amount": 20
        },
        {
            "item": "apple",
            "heal_amount": 3
        },
        {
            "item": "golden_carrot",
            "heal_amount": 4
        },
        {
            "item": "golden_apple",
            "heal_amount": 10
        },
        {
            "item": "appleEnchanted",
            "heal_amount": 10
        }
    ]
}
{
    "force_use": true,
    "filters": {
        "test": "is_riding",
        "operator": "!=",
        "value": true
    },
    "items": [
        {
            "item": "cookie",
            "heal_amount": 0,
            "effects": [
                {
                    "name": "fatal_poison",
                    "chance": 1,
                    "duration": 1000,
                    "amplifier": 0
                }
            ]
        }
    ]
}
```
### minecraft:insomnia

添加一个计时器记录玩家上一次睡觉的时间（用于决定是否生成幻翼）。


- `days_until_insomnia`：（数字，默认 4.0）玩家需要修仙的天数。

```
{
    "days_until_insomnia": 3
}
```
### minecraft:interact

定义与此实体的交互。


- `add_items`：（对象）成功交互以后添加到玩家物品栏的战利品表。

- `table`：（字符串）战利品表的相对路径，从行为包的根目录起始，包含 `.json` 后缀。
- `cooldown`：（数字，默认 0.0）该实体再次能被交互的秒数。
- `hurt_item`：（整数，默认 0）使用物品交互时物品损失的耐久。设置为 0 则不会损失任何耐久。
- `interact_text`:（字符串）触屏模式下与该实体交互时显示的文本。
- `on_interact`：（对象，格式同触发器）交互时触发。
- `particle_on_start`：（对象）在开始交互时产生的粒子效果。

- `particle_offset_towards_interactor`：（布尔值）粒子是否会靠近与该实体交互的玩家。
- `particle_type`：（字符串）会被显示的粒子。
- `particle_y_offset`：（数字）粒子在 y 轴上的偏转方向。
- `play_sounds`：（字符串）交互发生时播放的音效。
- `spawn_entities`：（字符串）交互发生时生成的实体。
- `spawn_items`：（对象）交互发生时根据战利品表产生的物品实体。

- `table`：（字符串）战利品表的相对路径，从行为包的根目录起始，包含 `.json` 后缀。
- `swing`：（布尔值，默认为 `false`）如果为 `true`，玩家与该实体交互时会做「摆动」动画。
- `transform_to_item`：（字符串）使用的物品会被转换为指定物品。格式为 `<物品名>:<数据值>`。
- `use_item`：（布尔值，默认为 `false`）如果为 `true`，该交互将会使用物品。

```
{
    "on_interact": {
        "filters": {
            "all_of": [
                {
                    "test": "has_equipment",
                    "domain": "hand",
                    "subject": "other",
                    "value": "golden_apple"
                },
                {
                    "test": "has_component",
                    "subject": "self",
                    "value": "minecraft:effect.weakness"
                }
            ]
        },
        "event": "villager_converted",
        "target": "self"
    },
    "use_item": true,
    "interact_text": "action.interact.cure"
}
[
    {
        "on_interact": {
            "filters": {
                "any_of": [
                    {
                        "test": "has_equipment",
                        "subject": "other",
                        "domain": "hand",
                        "value": "fireball:0"
                    },
                    {
                        "test": "has_equipment",
                        "subject": "other",
                        "domain": "hand",
                        "value": "flint_and_steel"
                    }
                ],
                "all_of": [
                    {
                        "test": "is_family",
                        "subject": "other",
                        "value": "player"
                    },
                    {
                        "test": "is_game_rule",
                        "domain": "tntexplodes",
                        "operator": "==",
                        "value": true
                    }
                ]
            },
            "event": "minecraft:on_prime",
            "target": "self"
        },
        "swing": true,
        "play_sounds": "ignite",
        "interact_text": "action.interact.creeper"
    },
    {
        "on_interact": {
            "filters": {
                "any_of": [
                    {
                        "test": "has_component",
                        "subject": "other",
                        "value": "fire_aspect"
                    }
                ],
                "all_of": [
                    {
                        "test": "is_game_rule",
                        "domain": "tntexplodes",
                        "operator": "==",
                        "value": true
                    }
                ]
            },
            "event": "minecraft:on_prime",
            "target": "self"
        },
        "swing": true,
        "interact_text": "action.interact.creeper"
    }
]
```
### minecraft:inventory

定义该实体的物品栏属性。


- `additional_slots_per_strength`：（整数，默认 0）每一点力量该实体可获得的栏位数量。
- `can_be_siphoned_from`：（布尔值，默认为 `false`）如果为 `true`，该物品栏的内容可以被漏斗取出。
- `table`：（字符串）该实体的容器类型。可以是 `horse`（马）、`minecart_chest`（箱子矿车）、`minecart_hopper`（漏斗矿车）、`inventory`（背包）、`container`（容器）或 `hopper`（漏斗）。
- `inventory_size`：（整数，默认 5）定义该容器所有的栏位数量。
- `private`：（布尔值，默认为 `false`）如果为 `true`，只有该实体可以访问物品栏。
- `restrict_to_owner`：（布尔值，默认为 `false`）如果为 `true`，只有该实体或它的主人可以访问物品栏。

```
{
    "container_type": "minecart_chest",
    "inventory_size": 27,
    "can_be_siphoned_from": true
}
{
    "inventory_size": 16,
    "container_type": "horse",
    "additional_slots_per_strength": 3
}
{
    "inventory_size": 16,
    "container_type": "horse"
}
{
    "inventory_size": 8,
    "private": true
}
```
### minecraft:item_hopper

决定该实体为一个物品漏斗。

### minecraft:jump.dynamic

定义一个动态的跳跃高度控制。将会基于实体的速度修饰器改变跳跃的属性。

### minecraft:jump.static

给予实体跳跃的能力。


- `jump_power`：（数字，默认 0.42）跳跃的初始垂直速度。

### minecraft:leashable

允许该实体可以被栓绳拴住。


- `hard_distance`：（数字，默认 6.0）。栓绳开始变硬并限制实体移动的距离。
- `max_distance`：（数字，默认 10.0）。栓绳断开的距离。
- `on_leash`：（对象，格式同触发器）。当该实体被拴住时触发。
- `on_unleash`：（对象，格式同触发器）。当该实体被解拴时触发。
- `soft_distance`：（数字，默认 4.0）。栓绳开始有弹性的距离，使得该实体不断接近拴着它的实体。

```
{
    "soft_distance": 4,
    "hard_distance": 6,
    "max_distance": 10,
    "on_leash": {
        "event": "minecraft:on_leash",
        "target": "self"
    },
    "on_unleash": {
        "event": "minecraft:on_unleash",
        "target": "self"
    }
}
```
### minecraft:lookat

定义当另一个实体看向这个实体时的行为（参考末影人 `enderman.json` 被玩家看而激怒）。


- `filters`：（对象）定义能够触发此组件的实体的滤器（有关滤器的具体内容见下方介绍）。
- `look_cooldown`：（数字）该实体不再愤怒的时间。
- `look_cooldown`：（数组，默认 `[0.0, 0.0]`）该实体不再愤怒的时间范围。
- `look_event`：（对象，格式同触发器）当有满足条件的实体看向该实体时触发。
- `mAllowInvulnerable`：（布尔值，默认 `false`）如果为 `true`，无敌状态的实体（例如创造模式的玩家）也会触发。
- `searchRadius`：（数字，默认 10.0）搜寻看向该实体的实体的最大距离。
- `setTarget`：（布尔值，默认 `true`）如果为 `true`，该实体会将看向它的实体作为攻击目标。

```
{
    "searchRadius": 64,
    "setTarget": true,
    "look_cooldown": 5,
    "filters": {
        "all_of": [
            {
                "subject": "other",
                "test": "is_family",
                "value": "player"
            },
            {
                "test": "has_equipment",
                "domain": "head",
                "subject": "other",
                "operator": "not",
                "value": "carved_pumpkin"
            }
        ]
    }
}
```
### minecraft:movement.amphibious

允许该实体在水中游泳或在陆地上行走。


- `max_turn`：（数字，默认 30.0）该生物一刻内能最大转过的度数。

### minecraft:movement.basic

实体的基础移动组件。


- `max_turn`：（数字，默认 30.0）该生物一刻内能最大转过的度数。

### minecraft:movement.fly

允许该实体飞行。


- `max_turn`：（数字，默认 30.0）该生物一刻内能最大转过的度数。

### minecraft:movement.generic

允许该实体飞行、游泳、爬行等。


- `max_turn`：（数字，默认 30.0）该生物一刻内能最大转过的度数。

### minecraft:movement.jump

允许该实体跳跃。


- `jump_delay`：（数组，默认 `[0.0, 0.0]`）两次跳跃之间的间隔时间。
- `max_turn`：（数字，默认 30.0）该生物一刻内能最大转过的度数。

### minecraft:movement.skip

允许实体在移动时跳跃。


- `max_turn`：（数字，默认 30.0）该生物一刻内能最大转过的度数。

### minecraft:movement.sway

使实体左右摇摆，给人以游泳的感觉。


- `max_turn`：（数字，默认 30.0）该生物一刻内能最大转过的度数。

### minecraft:nameable

允许该实体被命名（如使用命名牌）。


- `allowNameTagRenaming`：（布尔值，默认 `true`）如果为 `true`，该实体可以被命名牌命名。
- `alwaysShow`：（布尔值，默认 `false`）如果为 `true`，该实体会一直显示名称。
- `default_trigger`：（字符串）当该实体被命名时执行的触发器。
- `name_actions`：（数组）当实体有特殊的名称时触发的触发器。

- *（对象）*

- `name_filter`：（数组）能够触发 `on_named` 定义的触发器的特殊的名字。
- `on_named`：（对象，格式同触发器）当实体拥有 `name_filter` 中定义的一个名字时触发。



```
{
    "alwaysShow": true,
    "allowNameTagRenaming": false
}
{
    "default_trigger": {
        "event": "minecraft:stop_johnny",
        "target": "self"
    },
    "name_actions": [
        {
            "name_filter": "Johnny",
            "on_named": {
                "event": "minecraft:start_johnny",
                "target": "self"
            }
        }
    ]
}
```
### minecraft:navigation.*

下面是有关寻路的一些组件。「寻路」是实体在移动前对自己移动路线的规划。「规划」和「实际」有可能会不一致，例如你在寻路组件中允许该实体通过门，那么实体在规划路线时就有可能穿过门，但假设你没有在 AI 目标组件中允许该实体开门，那么实体到了门前以后只会傻站着，而不会像它原先规划地那样通过门。换句话说就是，实体在规划路径时以为自己可以开门，但真正到了门前才发现自己菜到连门都打不开（大雾。

[size=2]**minecraft:navigation.climb

允许实体规划的路径能够穿过垂直的方块（如原版的蜘蛛）。


- `avoid_portals`：（布尔值，默认 `false`）如果为 `true`，该实体规划的路径能够穿过传送门（如地狱传送门）。
- `avoid_sun`：（布尔值，默认 `false`）如果为 `true`，该实体规划的路径能够躲避开阳光。
- `avoid_water`：（布尔值，默认 `false`）如果为 `true`，该实体规划的路径能够避开水。
- `can_open_doors`：（布尔值，默认 `false`）如果为 `true`，该实体规划的路径能够穿过关上的门。
- `can_pass_doors`：（布尔值，默认 `true`）如果为 `true`，该实体规划的路径能够穿过打开的门。
- `can_path_over_water`：（布尔值，默认 `false`）如果为 `true`，该实体规划的路径能够通过水面。
- `can_sink`：（布尔值，默认 `true`）如果为 `true`，该实体规划的路径会在水里受到重力影响下沉。

[size=2]**minecraft:navigation.float

允许实体规划的路径能飘在空中（如原版的恶魂）。


- `avoid_portals`：（布尔值，默认 `false`）如果为 `true`，该实体规划的路径能够穿过传送门（如地狱传送门）。
- `avoid_sun`：（布尔值，默认 `false`）如果为 `true`，该实体规划的路径能够躲避开阳光。
- `avoid_water`：（布尔值，默认 `false`）如果为 `true`，该实体规划的路径能够避开水。
- `can_open_doors`：（布尔值，默认 `false`）如果为 `true`，该实体规划的路径能够穿过关上的门。
- `can_pass_doors`：（布尔值，默认 `true`）如果为 `true`，该实体规划的路径能够穿过打开的门。
- `can_path_over_water`：（布尔值，默认 `false`）如果为 `true`，该实体规划的路径能够通过水面。
- `can_sink`：（布尔值，默认 `true`）如果为 `true`，该实体规划的路径会在水里受到重力影响下沉。

[size=2]**minecraft:navigation.fly

允许实体规划的路径能飞（如原版的鹦鹉）。


- `avoid_portals`：（布尔值，默认 `false`）如果为 `true`，该实体规划的路径能够穿过传送门（如地狱传送门）。
- `avoid_sun`：（布尔值，默认 `false`）如果为 `true`，该实体规划的路径能够躲避开阳光。
- `avoid_water`：（布尔值，默认 `false`）如果为 `true`，该实体规划的路径能够避开水。
- `can_open_doors`：（布尔值，默认 `false`）如果为 `true`，该实体规划的路径能够穿过关上的门。
- `can_pass_doors`：（布尔值，默认 `true`）如果为 `true`，该实体规划的路径能够穿过打开的门。
- `can_path_over_water`：（布尔值，默认 `false`）如果为 `true`，该实体规划的路径能够通过水面。
- `can_sink`：（布尔值，默认 `true`）如果为 `true`，该实体规划的路径会在水里受到重力影响下沉。

[size=2]**minecraft:navigation.generic

允许实体规划的路径能行走、游泳、飞行和/或爬行以及跳上方块/走下方块。


- `avoid_portals`：（布尔值，默认 `false`）如果为 `true`，该实体规划的路径能够穿过传送门（如地狱传送门）。
- `avoid_sun`：（布尔值，默认 `false`）如果为 `true`，该实体规划的路径能够躲避开阳光。
- `avoid_water`：（布尔值，默认 `false`）如果为 `true`，该实体规划的路径能够避开水。
- `can_open_doors`：（布尔值，默认 `false`）如果为 `true`，该实体规划的路径能够穿过关上的门。
- `can_pass_doors`：（布尔值，默认 `true`）如果为 `true`，该实体规划的路径能够穿过打开的门。
- `can_path_over_water`：（布尔值，默认 `false`）如果为 `true`，该实体规划的路径能够通过水面。
- `can_sink`：（布尔值，默认 `true`）如果为 `true`，该实体规划的路径会在水里受到重力影响下沉。

[size=2]**minecraft:navigation.swim

允许实体规划的路径能包括水。


- `avoid_portals`：（布尔值，默认 `false`）如果为 `true`，该实体规划的路径能够穿过传送门（如地狱传送门）。
- `avoid_sun`：（布尔值，默认 `false`）如果为 `true`，该实体规划的路径能够躲避开阳光。
- `avoid_water`：（布尔值，默认 `false`）如果为 `true`，该实体规划的路径能够避开水。
- `can_open_doors`：（布尔值，默认 `false`）如果为 `true`，该实体规划的路径能够穿过关上的门。
- `can_pass_doors`：（布尔值，默认 `true`）如果为 `true`，该实体规划的路径能够穿过打开的门。
- `can_path_over_water`：（布尔值，默认 `false`）如果为 `true`，该实体规划的路径能够通过水面。
- `can_sink`：（布尔值，默认 `true`）如果为 `true`，该实体规划的路径会在水里受到重力影响下沉。

[size=2]**minecraft:navigation.walk

允许实体规划的路径能在地面上跳跃或走下方块（如常见的那些生物）。


- `avoid_portals`：（布尔值，默认 `false`）如果为 `true`，该实体规划的路径能够穿过传送门（如地狱传送门）。
- `avoid_sun`：（布尔值，默认 `false`）如果为 `true`，该实体规划的路径能够躲避开阳光。
- `avoid_water`：（布尔值，默认 `false`）如果为 `true`，该实体规划的路径能够避开水。
- `can_open_doors`：（布尔值，默认 `false`）如果为 `true`，该实体规划的路径能够穿过关上的门。
- `can_pass_doors`：（布尔值，默认 `true`）如果为 `true`，该实体规划的路径能够穿过打开的门。
- `can_path_over_water`：（布尔值，默认 `false`）如果为 `true`，该实体规划的路径能够通过水面。
- `can_sink`：（布尔值，默认 `true`）如果为 `true`，该实体规划的路径会在水里受到重力影响下沉。

### minecraft:peek

定义实体的「窥视」行为（参考原版的潜影贝）。


- `on_close`：（对象，格式同触发器）当实体结束窥视时触发。
- `on_open`：（对象，格式同触发器）当实体开始窥视时触发。
- `on_target_open`：（对象，格式同触发器）当该实体的目标开始窥视时触发。

```
{
    "on_open": {
        "event": "minecraft:on_open"
    },
    "on_close": {
        "event": "minecraft:on_close"
    },
    "on_target_open": {
        "event": "minecraft:on_open"
    }
}
```
### minecraft:projectile

定义该实体为弹射物（如原版的鸡蛋、投掷药水等）。


- `angleoffset`：（数字，默认 0.0）定义该实体的抛出角度。
- `catchFire`：（布尔值，默认 `false`）如果为 `true`，该实体击中的实体会被点燃。
- `critParticleOnHurt`：（布尔值，默认 `false`）如果为 `true`，在会心一击时会产生更多粒子。
- `critParticleOnHurt`：（布尔值，默认 `false`）如果为 `true`，该实体撞击以后会消失。
- `filter`：（字符串）不能被该弹射物击中的实体。
- `fireAffectedByGriefing`：（布尔值，默认 `false`）如果为 `true`，该实体是否会产生火将受到 `mobGriefing` 游戏规则的影响。
- `gravity`：（数字，默认 0.05）定义该实体的重力。数字越大，该实体下落越快。
- `hitSound`：（字符串）该实体撞击时的音效。
- `homing`：（布尔值，默认 `false`）如果为 `true`，该实体会追踪最近的实体。
- `inertia`：（数字，默认 0.99）该实体每在空中一刻所剩下的速度的比值。
- `isdangerous`：（布尔值，默认 `false`）如果为 `true`，该实体会被视作对玩家有威胁。
- `knockback`：（布尔值，默认 `true`）如果为 `true`，该实体会击退它击中的实体。
- `lightning`：（布尔值，默认 `false`）如果为 `true`，被击中的实体会被雷劈。
- `liquid_inertia`：（数字，默认 0.6）该实体每在水中一刻所剩下的速度的比值。
- `multipletargets`：（布尔值，默认 `false`）如果为 `true`，该实体一次飞行可以击中多个实体。
- `offset`：（数组，默认 `[0.0, 0.5, 0.0]`）该实体生成时的偏移位置。
- `onFireTime`：（数字，默认 5.0）被击中的实体着火的秒数。
- `particle`：（字符串，默认 `iconcrack`）击中时的粒子效果。
- `potionEffect`：（整数，默认 -1）被击中的实体被附加的状态效果。
- `power`：（数字，默认 1.3）该实体的速度。
- `reflectOnHurt`：（布尔值，默认 `false`）如果为 `true`，该实体撞击时会反弹。
- `semirandomdiffdamage`：（布尔值，默认 `false`）如果为 `true`，伤害量会基于伤害和速度随机施加。
- `shootSound`：（字符串）该实体被射出时的音效。
- `shoottarget`：（布尔值，默认 `true`）如果为 `true`，该弹射物会射向攻击实体的实体。
- `shouldbounce`：（布尔值，默认 `false`）如果为 `true`，该实体会在撞击时弹跳。
- `splashPotion`：（布尔值，默认 `false`）如果为 `true`，该实体会被认为是喷溅药水。
- `splashRange`：（数字，默认 4.0）喷溅效果的范围。
- `uncertaintyBase`：（数字，默认 0.0）基准精度。精度由公式 `uniqueBase - <游戏难度> * uncertaintyMultiplier` 计算。
- `uncertaintyMultiplier`：（数字，默认 0.0）决定游戏难度对精度的影响。精度由公式 `uniqueBase - <游戏难度> * uncertaintyMultiplier` 计算。

```
{
    "onHit": {
        "douse_fire": {},
        "thrown_potion_effect": {},
        "remove_on_hit": {}
    },
    "power": 0.5,
    "gravity": 0.05,
    "angleoffset": -20,
    "hitSound": "glass"
}
{
    "onHit": {
        "impact_damage": {
            "damage": 8,
            "knockback": true,
            "semirandomdiffdamage": false,
            "destroyonhit": false
        },
        "stick_in_ground": {
            "shake_time": 0
        }
    },
    "liquid_inertia": 0.99,
    "hitSound": "item.trident.hit",
    "hitGroundSound": "item.trident.hit_ground",
    "power": 4,
    "gravity": 0.1,
    "uncertaintyBase": 1,
    "uncertaintyMultiplier": 0,
    "stopOnHurt": true,
    "anchor": 1,
    "shouldbounce": true,
    "multipletargets": false,
    "offset": [
        0,
        -0.1,
        0
    ]
}
```
### minecraft:rail_movement

定义该实体在铁轨上的移动。拥有该组件的实体将只能在铁轨上移动。


- `max_speed`：（数字，默认 0.4）该实体在铁轨上移动时的最大速度。

### minecraft:rail_sensor

定义当铁轨被激活/取消激活时该实体的行为。


- `check_block_types`：（布尔值，默认 `false`）如果为 `true`，该实体会在每一刻触发 `on_deactivate` 事件。
- `eject_on_activate`：（布尔值，默认 `true`）如果为 `true`，该实体会在铁轨激活时将所有乘客丢出。
- `eject_on_deactivate`：（布尔值，默认 `false`）如果为 `true`，该实体会在铁轨取消激活时将所有乘客丢出。
- `on_activate`：（对象，格式同触发器）当铁轨激活时触发。
- `on_deactivate`：（对象，格式同触发器）当铁轨取消激活时触发。
- `tick_command_block_on_activate`：（布尔值，默认 `true`）如果为 `true`，该实体会在通过激活铁轨时每刻执行命令方块。
- `tick_command_block_on_deactivate`：（布尔值，默认 `false`）如果为 `true`，该实体会在通过非激活的铁轨时每刻执行命令方块。

```
{
    "check_block_types": false,
    "eject_on_activate": false,
    "eject_on_deactivate": false,
    "tick_command_block_on_activate": true,
    "tick_command_block_on_deactivate": false,
    "on_activate": {
        "event": "minecraft:command_block_activate"
    }
}
{
    "on_activate": {
        "filters": {
            "all_of": [
                {
                    "test": "is_game_rule",
                    "domain": "tntexplodes",
                    "operator": "==",
                    "value": true
                }
            ]
        },
        "event": "minecraft:on_prime"
    }
}
```
### minecraft:rideable

允许该实体被骑乘。可以定义多个不同位置的座位。


- `controlling_seat`：（整数，默认 0）能够控制该实体移动方向的座位在 `seats` 中的索引。
- `crouching_skip_interact`：（布尔值，默认 `true`）如果为 `true`，潜行时不能与该实体交互。
- `family_types`：（数组）能够骑乘该实体的实体类型。

- *（字符串）一个实体类型*
- `interact_text`：（字符串）使用触摸屏与该实体交互时显示的文字。
- `pull_in_entities`：（布尔值，默认 `false`）如果为 `true`，该实体会将符合条件的实体拉入任一可用的座位。
- `rider_can_interact`：（布尔值，默认 `false`）如果为 `true`，乘客可以通过看向该实体与之交互。
- `seat_count`：（整数，默认 1）能同时骑乘该实体的最大实体数目。
- `seats`：（数组）座位列表。

- *（对象）一个座位*

- `lock_rider_rotation`：（数字，默认 181.0）当骑乘该实体时乘客所能转向的角度限制。如不指定则无限制。
- `max_rider_count`：（整数，默认 0）当骑乘该实体的实体数量小于此值时，该座位才可用。
- `min_rider_count`：（整数，默认 0）当骑乘该实体的实体数量大于此值时，该座位才可用。
- `position`：（数组，默认 `[0.0, 0.0, 0.0]`）座位相对于该实体位置的位置。
- `rotate_rider_by`：（数字，默认 0.0）乘客旋转的偏移角度。



```
{
    "seat_count": 1,
    "family_types": [
        "zombie"
    ],
    "seats": {
        "position": [
            0,
            0.9,
            -0.25
        ]
    }
}
{
    "seat_count": 2,
    "family_types": [
        "parrot_tame"
    ],
    "pull_in_entities": true,
    "seats": [
        {
            "position": [
                0.4,
                -0.15,
                0.04
            ],
            "min_rider_count": 0,
            "max_rider_count": 0,
            "lock_rider_rotation": 0
        },
        {
            "position": [
                -0.4,
                -0.15,
                0.04
            ],
            "min_rider_count": 1,
            "max_rider_count": 2,
            "lock_rider_rotation": 0
        }
    ]
}
```
### minecraft:scaffolding_climber

允许在脚手架上爬行。只能被玩家使用。

### minecraft:scale_by_age

定义基于年龄的该实体大小的缩放。


- `start_scale`：（数字，默认 1.0）新生实体的起始缩放倍数。
- `end_scale`：（数字，默认 1.0）实体完全成熟时的终止缩放倍数。

```
{
    "start_scale": 0.5,
    "end_scale": 1
}
```
### minecraft:shareables

定义该实体想要交换的物品。


- `items`：（数组）想要交换的物品列表。

- *（对象）一个物品*

- `item`：（字符串）该实体想要的物品。
- `craft_into`：（字符串）该实体能给出的物品。
- `want_amount`：（整数）该实体能给出的物品的数量。
- `surplus_amount`：（整数）该实体能给出的额外物品的数量。



```
{
    "items": [
        {
            "item": "minecraft:bread",
            "want_amount": 3,
            "surplus_amount": 6
        },
        {
            "item": "minecraft:carrot",
            "want_amount": 12,
            "surplus_amount": 24
        },
        {
            "item": "minecraft:potato",
            "want_amount": 12,
            "surplus_amount": 24
        },
        {
            "item": "minecraft:beetroot",
            "want_amount": 12,
            "surplus_amount": 24
        }
    ]
}
```
### minecraft:shooter

定义该实体的远程攻击表现。


- `auxVal`：（整数，默认 -1）击中时所应用的状态效果的 ID。
- `def`：（字符串）作为远程攻击的弹射物的实体 ID。指定的实体必须有 `minecraft:projectile` 组件。

```
{
    "def": "minecraft:snowball"
}
{
    "type": "Arrow",
    "def": "minecraft:arrow",
    "auxVal": 19
}
```
### minecraft:sittable

定义该实体的「坐姿」状态。


- `sit_event`：（对象，格式同触发器）当该实体坐下时触发。
- `stand_event`：（对象，格式同触发器）当该实体站起时触发。

### minecraft:spawn_entity

该实体在一段时间后生成另一个实体。类似原版鸡下蛋的表现。


- `max_wait_time`：（整数，默认 600）随机生成另一个实体的最长秒数。
- `min_wait_time`：（整数，默认 300）随机生成另一个实体的最短秒数。
- `spawn_entity`：（字符串）生成实体的 ID。如果留空则会依据 `spawn_item` 生成物品。
- `spawn_event`：（字符串，默认 `minecraft:entity_born`）当实体生成时其触发的事件（有关事件的具体内容见下方介绍）。
- `spawn_item`：（字符串，默认 `egg`）生成的物品的名字。仅在 `spawn_entity` 未被指定时有效。
- `spawn_method`：（字符串，默认 `born`）生成该实体的方式。
- `spawn_sound`：（字符串，默认 `plop`）实体生成后播放的音效。

```
{
    "min_wait_time": 300,
    "max_wait_time": 600,
    "spawn_sound": "plop",
    "spawn_item": "egg"
}
```
### minecraft:tameable

允许该实体被玩家驯服。


- `probability`：（数字，默认 1.0）每使用一个物品使该实体被驯服的可能性。取值应在 [0.0, 1.0] 之中。
- `tameItems`：（数组）能够驯服该实体的物品列表。

- *（字符串）一个物品*
- `tame_event`：（对象，格式同触发器）当该实体被驯服时触发。

```
{
    "probability": 0.33,
    "tameItems": [
        "fish",
        "salmon"
    ],
    "tame_event": {
        "event": "minecraft:on_tame",
        "target": "self"
    }
}
{
    "probability": 0.33,
    "tameItems": [
        "wheat_seeds",
        "pumpkin_seeds",
        "melon_seeds",
        "beetroot_seeds"
    ],
    "tame_event": {
        "event": "minecraft:on_tame",
        "target": "self"
    }
}
```
### minecraft:tamemount

允许该实体能通过骑乘被驯服。


- `attemptTemperMod`：（整数，默认 5）每次尝试骑乘时增加的 temper。
- `autoRejectItems`：（对象）该实体不喜欢的物品。当携带这些物品与实体交互时会使其愤怒。

- `item`：（字符串）物品名。
- `feedItems`：（数组）该实体喜欢的物品。可以喂食这些物品提升实体的 temper，加速驯化过程。

- *（对象）*

- `item`：（字符串）物品名。
- `temperMod`：（整数，默认 0）当喂食此物品时增加的 temper。

- `feed_text`：（字符串）显示在喂食交互按钮上的文本。
- `ride_text`：（字符串）显示在骑乘交互按钮上的文本。
- `maxTemper`：（整数，默认 100）实体 temper 的随机初始值的最大值。当 temper 到达此值时即可被驯服。
- `minTemper`：（整数，默认 0）实体 temper 的随机初始值的最小值。
- `tame_event`：（对象，格式同触发器）当成功驯服时触发。

```
{
    "minTemper": 0,
    "maxTemper": 30,
    "feed_text": "action.interact.feed",
    "ride_text": "action.interact.mount",
    "feedItems": [
        {
            "item": "wheat",
            "temperMod": 3
        },
        {
            "item": "tile.hay_block",
            "temperMod": 6
        }
    ],
    "autoRejectItems": [
        {
            "item": "horsearmorleather"
        },
        {
            "item": "horsearmoriron"
        },
        {
            "item": "horsearmorgold"
        },
        {
            "item": "horsearmordiamond"
        },
        {
            "item": "saddle"
        }
    ],
    "tame_event": {
        "event": "minecraft:on_tame",
        "target": "self"
    }
}
```
### minecraft:target_nearby_sensor

定义实体能将其他实体视为目标的范围。


- `inside_range`：（数字，默认 1.0）将另一个实体列入「内部」范围的最远距离。
- `outside_range`：（数字，默认 5.0）将另一个实体列入「外部」范围的最远距离。
- `on_inside_range`：（对象，格式同触发器）当另一个实体进入「内部」范围时触发。
- `on_outside_range`：（对象，格式同触发器）当另一个实体进入「外部」范围时触发。

```
{
    "inside_range": 3,
    "outside_range": 5,
    "on_inside_range": {
        "event": "minecraft:switch_to_melee",
        "target": "self"
    },
    "on_outside_range": {
        "event": "minecraft:switch_to_ranged",
        "target": "self"
    }
}
```
### minecraft:teleport

定义一个实体的传送行为。


- `darkTeleportChance`：（数字，默认 0.01）定义实体在黑暗中传送的可能性。取值应在 [0.0, 1.0] 之中。
- `lightTeleportChance`：（数字，默认 0.01）定义实体在光照中传送的可能性。取值应在 [0.0, 1.0] 之中。
- `maxRandomTeleportTime`：（数字，默认 20.0）定义实体两次随机传送间隔的最大秒数。
- `minRandomTeleportTime`：（数字，默认 0.0）定义实体两次随机传送间隔的最小秒数。
- `randomTeleportCube`：（数组，默认 `[32.0, 16.0, 32.0]`）实体会在该大小的区域内随机传送。
- `randomTeleports`：（布尔值，默认 `true`）如果为 `true`，该实体会随机传送。
- `targetDistance`：（数字，默认 16.0）定义实体追踪目标时最远传送距离。
- `target_teleport_chance`：（数字，默认 1.0）定义实体在追踪目标时传送的可能性。取值应在 [0.0, 1.0] 之中。

```
{
    "randomTeleports": true,
    "maxRandomTeleportTime": 30,
    "randomTeleportCube": [
        32,
        32,
        32
    ],
    "targetDistance": 16,
    "target_teleport_chance": 0.05,
    "lightTeleportChance": 0.05
}
```
### minecraft:tick_world

定义实体是否加载世界。


- `distance_to_players`：（数字，默认 128.0，最小值 128.0）该实体被移除时距离玩家的最短距离。如果 `never_despawn` 设置为 `true` 该选项将被忽略。
- `never_despawn`：（布尔值，默认 `false`）如果为 `true`，该实体即使距离玩家很远也不会被移除。
- `radius`：（整数，默认 2，可用范围 2-6）该实体加载的区域半径。

### minecraft:timer

定义一个一段时间后触发触发器的组件。


- `looping`：（布尔值，默认 `true`）如果为 `true`，该实体会在每次触发以后重新计时。
- `randomInterval`：（布尔值，默认 `true`）如果为 `true`，该计时器每次的时间会在范围内随机生成。
- `time`：（数字，默认 0.0）秒数。
- `time`：（数组，默认 `[0.0, 0.0]`）秒数的范围。
- `time_down_event`：（对象，格式同触发器）当时间到达时触发。

```
{
    "looping": false,
    "time": 30,
    "time_down_event": {
        "event": "minecraft:convert_to_drowned"
    }
}
```
### minecraft:trade_table

定义该实体与玩家交易的能力。


- `display_name`：（字符串）与此实体交易时显示的名称。可以使用资源包中定义的语言文件节点。
- `table`：（字符串）从行为包根目录起始的交易表的相对路径，包含后缀 `.json`。使用左斜杠 `/` 分割路径。

```
{
    "display_name": "entity.villager.leather",
    "table": "trading/leather_worker_trades.json"
}
```
### minecraft:transformation

定义实体从当前实体转变为另一个实体的行为。


- `add`：（对象）转变后该实体启用的组件组。

- `component_groups`：（数组）启用的组件组的名字。

- *（字符串）一个组件组*

- `remove`：（对象）转变后该实体禁用的组件组。

- `component_groups`：（数组）禁用的组件组的名字。

- *（字符串）一个组件组*

- `begin_transform_sound`：（字符串）开始转变时的音效。
- `delay`：（对象）定义该实体转变的延迟。

- `block_assist_chance`：（数字，默认 0.0）实体寻找附近的能够提升转变速度的方块的几率。取值应在 [0.0, 1.0] 之中。
- `block_chance`：（数字，默认 0.0）实体找到此方块后，该方块能够提升转变速度的几率。取值应在 [0.0, 1.0] 之中。
- `block_radius`：（整数，默认 0）实体会寻找辅助转变的方块的半径。
- `block_max`：（整数，默认 0）实体会寻找的最大的辅助转变的方块数量。如果设置为 0 或不设置，将采用 `block_radius` 的值。
- `trust_items`：（数组）能辅助转变的方块列表。

- *（字符串）一个方块*
- `keep_owner`：（布尔值）如果为 `true`，该实体转变后会保留原有的主人。
- `value`：（数字，默认 0.0）实体完成转变所消耗的秒数。
- `drop_equipment`：（布尔值）如果为 `true`，该实体转变后会掉落所有装备。
- `into`：（字符串）该实体会转变成的实体。
- `transformation_sound`：（字符串）完成转变时的音效。

```
{
    "into": "minecraft:drowned<minecraft:as_baby>",
    "transformation_sound": "convert_to_drowned",
    "drop_equipment": true,
    "delay": {
        "value": 15
    }
}
{
    "into": "minecraft:villager",
    "begin_transform_sound": "remedy",
    "transformation_sound": "unfect",
    "delay": {
        "value": 100,
        "block_assist_chance": 0.01,
        "block_radius": 4,
        "block_chance": 0.3,
        "block_types": [
            "minecraft:bed",
            "minecraft:iron_bars"
        ]
    }
}
```
### minecraft:trusting

定义该实体信任玩家的条件。


- `probability`：（数字，默认 1.0）每使用一个物品使该实体信任玩家的可能性。取值应在 [0.0, 1.0] 之中。
- `trust_event`：（对象，格式同触发器）当该实体信任玩家时触发。
- `trust_items`：（数组）能使该实体信任玩家的物品列表。

- *（字符串）一个物品*


```
{
    "probability": 0.33,
    "trust_items": [
        "fish",
        "salmon"
    ],
    "trust_event": {
        "event": "minecraft:on_trust",
        "target": "self"
    }
}
```

[/spoiler]
