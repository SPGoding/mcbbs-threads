# 行为

行为组件，又称 AI 目标组件，简称 AI 组件。顾名思义，这一类组件可以定义该实体的 AI 目标。

所有的 AI 组件都共有一个参数：

- `priority`：（数字）该 AI 组件的优先级。数字越小，优先级越高。优先级高的 AI 组件会优先进行执行。

## minecraft:behavior.scared

当下雨时使该实体害怕。


- `sound_interval`：（整数，默认 0）声音播放的间隔。

```
{
    "priority": 1,
    "sound_interval": 20
}
```
## minecraft:behavior.avoid_mob_type

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
## minecraft:behavior.beg

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
## minecraft:behavior.break_door

允许该实体破坏门。

## minecraft:behavior.breed

允许该实体和其他实体交配。


- `speed_multiplier`：（数字，默认 1.0）在前往交配时的移动速度的倍数。`1.0` 表明速度未改变。

```
{
    "priority": 2,
    "speed_multiplier": 1
}
```
## minecraft:behavior.charge_attack

允许该实体追着目标打。

## minecraft:behavior.circle_around_anchor

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
## minecraft:behavior.controlled_by_player

允许该实体被玩家控制。

## minecraft:behavior.defend_village_target

允许该实体待在村庄，与对村民有害的敌对生物展开生死搏斗。


- `entity_types`：（数组）该实体攻击的实体类型。

- *（对象）*

- `filters`：（对象）滤器（有关滤器的详细内容请查看下方「滤器」部分）。
- `max_dist`：（整数，默认 16）该实体会被攻击的最远距离。
- `must_see`：（布尔值，默认 `false`）如果为 `true`，该实体必须在视野范围内才会被攻击。
- `sprint_speed_multiplier`：（数字，默认 1.0）在攻击时的奔跑速度的倍数。`1.0` 表明速度未改变。
- `walk_speed_multiplier`：（数字，默认 1.0）在攻击时的行走速度的倍数。`1.0` 表明速度未改变。



## minecraft:behavior.door_interact

允许该实体开关门。

## minecraft:behavior.dragonchargeplayer

允许末影龙通过快速飞行攻击玩家（玩家由 `minecraft:behavior.dragonscanning` 目标选择）。只能用于末影龙。

## minecraft:behavior.dragondeath

允许末影龙高调地死。控制末影龙的死亡动画。只对末影龙有效。

## minecraft:behavior.dragonflaming

允许末影龙喷火攻击。只对末影龙有效。

## minecraft:behavior.dragonholdingpattern

允许末影龙绕着中心飞行。只对末影龙有效。

## minecraft:behavior.dragonlanding

允许末影龙停止飞行，进入栖息模式。只对末影龙有效。

## minecraft:behavior.dragonscanning

允许末影龙在栖息时选择攻击的玩家对象。只对末影龙有效。

## minecraft:behavior.dragonstrafeplayer

允许末影龙绕着一个玩家并对他吐火球。只对末影龙有效。

## minecraft:behavior.dragontakeoff

允许末影龙停止栖息模式，进入飞行模式。只对末影龙有效。

## minecraft:behavior.drop_item_for

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
## minecraft:behavior.eat_block

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
## minecraft:behavior.enderman_leave_block

允许末影人放下它们拿着的方块。仅对末影人有效。

## minecraft:behavior.enderman_take_block

允许末影人拿起一个方块。仅对末影人有效。

## minecraft:behavior.find_mount

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
## minecraft:behavior.find_underwater_treasure

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
## minecraft:behavior.flee_sun

允许实体躲避太阳直射，寻找阴凉处。


- `speed_multiplier`：（数字，默认 1.0）在使用此 AI 目标组件时的移动速度的倍数。`1.0` 表明速度未改变。

```
{
    "priority": 2,
    "speed_multiplier": 1
}
```
## minecraft:behavior.float

允许实体在游泳时保持漂浮。

## minecraft:behavior.float_wander

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
## minecraft:behavior.follow_caravan

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
## minecraft:behavior.follow_mob

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
## minecraft:behavior.follow_owner

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
## minecraft:behavior.follow_parent

允许该实体跟随它们的父母。


- `speed_multiplier`：（数字，默认 1.0）在使用此 AI 目标组件时的移动速度的倍数。`1.0` 表明速度未改变。

```
{
    "priority": 4,
    "speed_multiplier": 1
}
```
## minecraft:behavior.go_home

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
## minecraft:behavior.guardian_attack

允许守卫者攻击。只能被守卫者或远古守卫者使用。

## minecraft:behavior.harvest_farm_block

允许村民收割附近的农田。只能被村民使用。


- `speed_multiplier`：（数字，默认 1.0）在使用此 AI 目标组件时的移动速度的倍数。`1.0` 表明速度未改变。

```
{
    "priority": 9,
    "speed_multiplier": 0.5
}
```
## minecraft:behavior.hurt_by_target

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
## minecraft:behavior.lay_down

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
## minecraft:behavior.lay_egg

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
## minecraft:behavior.leap_at_target

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
## minecraft:behavior.look_at_entity

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
## minecraft:behavior.look_at_player

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
## minecraft:behavior.look_at_target

允许实体看在它的目标上。


- `angle_of_view_horizontal`：（整数，默认 360）该实体在 Y 轴（上下）中能看到的生物的角度。
- `angle_of_view_vertical`：（整数，默认 360）该实体在 X 轴（左右）中能看到的生物的角度。
- `look_distance`：（数字，默认 8.0）该实体会看向的距离。
- `look_time`：（数组，默认 `[2, 4]`）该实体会看向某生物的时间范围。
- `probability`：（数字，默认 0.02）会看向生物的几率。`1.00` 为 100%。

## minecraft:behavior.look_at_trading_player

允许实体看向正在与它交易的玩家。


- `angle_of_view_horizontal`：（整数，默认 360）该实体在 Y 轴（上下）中能看到的生物的角度。
- `angle_of_view_vertical`：（整数，默认 360）该实体在 X 轴（左右）中能看到的生物的角度。
- `look_distance`：（数字，默认 8.0）该实体会看向的距离。
- `look_time`：（数组，默认 `[2, 4]`）该实体会看向某生物的时间范围。
- `probability`：（数字，默认 0.02）会看向生物的几率。`1.00` 为 100%。

## minecraft:behavior.make_love

允许村民找一个伴侣。只能被村民使用。

## minecraft:behavior.melee_attack

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
## minecraft:behavior.mount_pathing

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
## minecraft:behavior.move_indoors

允许村民在室内躲避。只能被村民使用。


- `speed_multiplier`：（数字，默认 1.0）在使用此 AI 目标组件时的移动速度的倍数。`1.0` 表明速度未改变。

```
{
    "priority": 4,
    "speed_multiplier": 0.8
}
```
## minecraft:behavior.move_through_village

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
## minecraft:behavior.move_to_land

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
## minecraft:behavior.move_to_water

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
## minecraft:behavior.move_towards_restriction

允许守卫者、铁傀儡和村民在它们预先设定的限制区域内移动。其他实体没有定义限制区域。


- `speed_multiplier`：（数字，默认 1.0）在使用此 AI 目标组件时的移动速度的倍数。`1.0` 表明速度未改变。

## minecraft:behavior.move_towards_target

允许该实体直接移向它的目标。


- `within_radius`：（数字，默认 0.0）该实体想要和目标的距离。`0.0` 表明它会尝试和目标占在同一格内。

## minecraft:behavior.nearest_attackable_target

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
## minecraft:behavior.ocelot_sit_on_block

允许实体像豹猫那样坐在一个地方。


- `speed_multiplier`：（数字，默认 1.0）在使用此 AI 目标组件时的移动速度的倍数。`1.0` 表明速度未改变。

## minecraft:behavior.ocelotattack

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
## minecraft:behavior.offer_flower

允许实体像铁傀儡那样递花。

## minecraft:behavior.open_door

允许实体开门。需要让实体能通过门，否则它不会去尝试开门。


- `close_door_after`：（布尔值，默认 `true`）如果为 `true`，该实体会随手关门。

```
{
    "priority": 6,
    "close_door_after": true
}
```
## minecraft:behavior.owner_hurt_by_target

允许该实体把伤害它主人的实体作为目标。


- `entity_types`：（数组）所有有效的实体类型。

- *（对象）*

- `filters`：（对象）滤器（有关滤器的详细内容请查看下方「滤器」部分）。
- `max_dist`：（整数，默认 16）该实体有效的最远距离。
- `must_see`：（布尔值，默认 `false`）如果为 `true`，该实体必须在视野范围内才有效。
- `sprint_speed_multiplier`：（数字，默认 1.0）奔跑速度的倍数。`1.0` 表明速度未改变。
- `walk_speed_multiplier`：（数字，默认 1.0）行走速度的倍数。`1.0` 表明速度未改变。



## minecraft:behavior.owner_hurt_target

允许该实体把它主人伤害的实体作为目标。


- `entity_types`：（数组）所有有效的实体类型。

- *（对象）*

- `filters`：（对象）滤器（有关滤器的详细内容请查看下方「滤器」部分）。
- `max_dist`：（整数，默认 16）该实体有效的最远距离。
- `must_see`：（布尔值，默认 `false`）如果为 `true`，该实体必须在视野范围内才有效。
- `sprint_speed_multiplier`：（数字，默认 1.0）奔跑速度的倍数。`1.0` 表明速度未改变。
- `walk_speed_multiplier`：（数字，默认 1.0）行走速度的倍数。`1.0` 表明速度未改变。



## minecraft:behavior.panic

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
## minecraft:behavior.peek

允许该实体窥探。潜影贝使用该 AI 目标组件从壳中往外看。

## minecraft:behavior.pet_sleep_with_owner

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
## minecraft:behavior.pickup_items

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
## minecraft:behavior.play

允许村民与其他幼儿村民玩耍。只能被村民使用。


- `speed_multiplier`：（数字，默认 1.0）在使用此 AI 目标组件时的移动速度的倍数。`1.0` 表明速度未改变。

```
{
    "priority": 8,
    "speed_multiplier": 0.32
}
```
## minecraft:behavior.player_ride_tamed

允许该实体被驯服以后能被玩家骑乘。

## minecraft:behavior.raid_garden

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
## minecraft:behavior.random_breach

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
## minecraft:behavior.random_fly

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
## minecraft:behavior.random_look_around

允许实体随机看向四周。


- `look_time`：（数组，默认 `[2, 4]`）实体会看向随机方向的时间所处的范围，单位为秒。

```
{
    "priority": 3,
    "look_distance": 8
}
```
## minecraft:behavior.random_sitting

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
## minecraft:behavior.random_stroll

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
## minecraft:behavior.random_swim

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
## minecraft:behavior.ranged_attack

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
## minecraft:behavior.receive_love

允许村民站住等待和另一个村民繁殖。只能被村民使用。

## minecraft:behavior.restrict_open_door

允许实体在夜间躲在室内。

## minecraft:behavior.restrict_sun

允许实体在日出后自动躲避阳光。

## minecraft:behavior.roll

允许实体随机向前。


- `probability`：（数字，默认 1.0）实体使用此 AI 目标组件的几率。

```
{
    "priority": 12,
    "probability": 0.013
}
```
## minecraft:behavior.run_around_like_crazy

允许实体漫无目的地跑。


- `speed_multiplier`：（数字，默认 1.0）在使用此 AI 目标组件时的移动速度的倍数。`1.0` 表明速度未改变。

```
{
    "priority": 1,
    "speed_multiplier": 1.2
}
```
## minecraft:behavior.send_event

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
## minecraft:behavior.share_items

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
## minecraft:behavior.silverfish_merge_with_stone

允许实体想蠹鱼那样钻进方块。目前只能被蠹鱼使用。

## minecraft:behavior.silverfish_wake_up_friends

允许实体让附近方块里面的实体出来。目前只能被蠹鱼使用。

## minecraft:behavior.skeleton_horse_trap

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
## minecraft:behavior.slime_attack

允许实体像史莱姆那样进行近战攻击。只能被史莱姆或岩浆怪使用。

## minecraft:behavior.slime_float

允许实体浮在熔岩或水里。只能被史莱姆或岩浆怪使用。

## minecraft:behavior.slime_keep_on_jumping

允许实体持续跳跃。只能被史莱姆或岩浆怪使用。

## minecraft:behavior.slime_random_direction

允许实体随机移动。只能被史莱姆或岩浆怪使用。

## minecraft:behavior.snacking

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
## minecraft:behavior.sneeze

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
## minecraft:behavior.squid_dive

允许鱿鱼潜入水底。只能被鱿鱼使用。

## minecraft:behavior.squid_flee

允许鱿鱼游走。只能被鱿鱼使用。

## minecraft:behavior.squid_idle

允许鱿鱼悠闲地游泳。只能被鱿鱼使用。

## minecraft:behavior.squid_move_away_from_ground

允许鱿鱼从地面回到水中。只能被鱿鱼使用。

## minecraft:behavior.squid_out_of_water

允许鱿鱼在水外时贴在地面。只能被鱿鱼使用。

## minecraft:behavior.stay_while_sitting

允许实体在坐姿状态时保持不动，而不是做别的。

## minecraft:behavior.stomp_attack

允许实体使用北极熊的近战攻击。


- `attack_once`：（布尔值，默认 `false`）如果为 `true`，该实体只会攻击一次。
- `attack_types`：（字符串）定义该实体会攻击的实体的类型。
- `random_stop_interval`：（整数，默认 0）该实体停止攻击的几率。0 为不可能，1 为 50%。
- `reach_multiplier`：（数字，默认 2.0）能使该实体攻击到目标的碰撞箱的倍数。将该值改大可以做出长臂猿的效果。
- `speed_multiplier`：（数字，默认 1.0）在使用此 AI 目标组件时的移动速度的倍数。`1.0` 表明速度未改变。
- `track_target`：（布尔值，默认 `false`）如果为 `true`，只要目标还有效，该实体就会追在目标后面。

## minecraft:behavior.stomp_turtle_egg

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
## minecraft:behavior.summon_entity

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
## minecraft:behavior.swell

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
## minecraft:behavior.swim_wander

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
## minecraft:behavior.swoop_attack

允许实体通过移动攻击目标。如果在水平方向上碰撞，此 AI 目标将会停止。用于为飞行实体使用。


- `delay_range`：（数组，默认 `[10.0, 20.0]`）再次使用此 AI 目标前的秒数。
- `speed_multiplier`：（数字，默认 1.0）在使用此 AI 目标组件时的移动速度的倍数。`1.0` 表明速度未改变。

```
{
    "priority": 2,
    "delay_range": [ 10, 20 ]
}
```
## minecraft:behavior.take_flower

允许该实体从铁傀儡处接受花朵。只能被村民使用。

## minecraft:behavior.tempt

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
## minecraft:behavior.trade_with_player

允许玩家与此实体交易。

## minecraft:behavior.vex_copy_owner_target

允许恼鬼将目标设置为与其主人相同的目标。


- `entity_types`：（数组）恼鬼可以从主人复制的实体列表。

- *（对象）*

- `filters`：（对象）滤器（有关滤器的详细内容请查看下方「滤器」部分）。
- `max_dist`：（整数，默认 16）该实体有效的最远距离。
- `must_see`：（布尔值，默认 `false`）如果为 `true`，该实体必须在视野范围内才有效。
- `sprint_speed_multiplier`：（数字，默认 1.0）奔跑速度的倍数。`1.0` 表明速度未改变。
- `walk_speed_multiplier`：（数字，默认 1.0）行走速度的倍数。`1.0` 表明速度未改变。



## minecraft:behavior.vex_random_move

允许凋零像恼鬼一样随机移动。

## minecraft:behavior.wither_random_attack_pos_goal

允许凋零随机攻击。只能被凋零使用。

## minecraft:behavior.wither_target_highest_damage

允许凋零攻击对它造成伤害最大的实体。


- `entity_types`：（数组）所有凋零要检查的实体。凋零会从中挑出对它造成伤害最大的实体。

- *（对象）*

- `filters`：（对象）滤器（有关滤器的详细内容请查看下方「滤器」部分）。
- `max_dist`：（整数，默认 16）该实体有效的最远距离。
- `must_see`：（布尔值，默认 `false`）如果为 `true`，该实体必须在视野范围内才有效。
- `sprint_speed_multiplier`：（数字，默认 1.0）奔跑速度的倍数。`1.0` 表明速度未改变。
- `walk_speed_multiplier`：（数字，默认 1.0）行走速度的倍数。`1.0` 表明速度未改变。
