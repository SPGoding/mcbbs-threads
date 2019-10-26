# 其他组件

这一类组件能完成各种杂七杂八的东西，不好分类，格式上也没有什么共同点，因此单独使用一章。

## minecraft:addrider

为该实体添加一个骑乘者。需要有 `minecraft:rideable`。


- `entity_type`：（字符串）骑乘该实体的实体类型。

```
{
    "entity_type": "minecraft:skeleton"
}
```
## minecraft:ageable

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
## minecraft:angry

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
## minecraft:boostable

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
## minecraft:breathable

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
## minecraft:breedable

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
## minecraft:bribeable

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
## minecraft:damage_over_time

使实体每隔指定时间后受到指定伤害。


- `damagePerHurt`：（整数，默认 1）每次伤害的伤害量。
- `timeBetweenHurt`：（整数，默认 0）两次伤害的间隔秒数。

```
{
    "damagePerHurt": 1,
    "timeBetweenHurt": 0
}
```
## minecraft:damage_sensor

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
## minecraft:environment_sensor

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
## minecraft:equippable

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
## minecraft:explode

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
## minecraft:healable

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
## minecraft:insomnia

添加一个计时器记录玩家上一次睡觉的时间（用于决定是否生成幻翼）。


- `days_until_insomnia`：（数字，默认 4.0）玩家需要修仙的天数。

```
{
    "days_until_insomnia": 3
}
```
## minecraft:interact

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
## minecraft:inventory

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
## minecraft:item_hopper

决定该实体为一个物品漏斗。

## minecraft:jump.dynamic

定义一个动态的跳跃高度控制。将会基于实体的速度修饰器改变跳跃的属性。

## minecraft:jump.static

给予实体跳跃的能力。


- `jump_power`：（数字，默认 0.42）跳跃的初始垂直速度。

## minecraft:leashable

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
## minecraft:lookat

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
## minecraft:movement.amphibious

允许该实体在水中游泳或在陆地上行走。


- `max_turn`：（数字，默认 30.0）该生物一刻内能最大转过的度数。

## minecraft:movement.basic

实体的基础移动组件。


- `max_turn`：（数字，默认 30.0）该生物一刻内能最大转过的度数。

## minecraft:movement.fly

允许该实体飞行。


- `max_turn`：（数字，默认 30.0）该生物一刻内能最大转过的度数。

## minecraft:movement.generic

允许该实体飞行、游泳、爬行等。


- `max_turn`：（数字，默认 30.0）该生物一刻内能最大转过的度数。

## minecraft:movement.jump

允许该实体跳跃。


- `jump_delay`：（数组，默认 `[0.0, 0.0]`）两次跳跃之间的间隔时间。
- `max_turn`：（数字，默认 30.0）该生物一刻内能最大转过的度数。

## minecraft:movement.skip

允许实体在移动时跳跃。


- `max_turn`：（数字，默认 30.0）该生物一刻内能最大转过的度数。

## minecraft:movement.sway

使实体左右摇摆，给人以游泳的感觉。


- `max_turn`：（数字，默认 30.0）该生物一刻内能最大转过的度数。

## minecraft:nameable

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
## minecraft:navigation.*

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

## minecraft:peek

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
## minecraft:projectile

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
## minecraft:rail_movement

定义该实体在铁轨上的移动。拥有该组件的实体将只能在铁轨上移动。


- `max_speed`：（数字，默认 0.4）该实体在铁轨上移动时的最大速度。

## minecraft:rail_sensor

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
## minecraft:rideable

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
## minecraft:scaffolding_climber

允许在脚手架上爬行。只能被玩家使用。

## minecraft:scale_by_age

定义基于年龄的该实体大小的缩放。


- `start_scale`：（数字，默认 1.0）新生实体的起始缩放倍数。
- `end_scale`：（数字，默认 1.0）实体完全成熟时的终止缩放倍数。

```
{
    "start_scale": 0.5,
    "end_scale": 1
}
```
## minecraft:shareables

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
## minecraft:shooter

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
## minecraft:sittable

定义该实体的「坐姿」状态。


- `sit_event`：（对象，格式同触发器）当该实体坐下时触发。
- `stand_event`：（对象，格式同触发器）当该实体站起时触发。

## minecraft:spawn_entity

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
## minecraft:tameable

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
## minecraft:tamemount

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
## minecraft:target_nearby_sensor

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
## minecraft:teleport

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
## minecraft:tick_world

定义实体是否加载世界。


- `distance_to_players`：（数字，默认 128.0，最小值 128.0）该实体被移除时距离玩家的最短距离。如果 `never_despawn` 设置为 `true` 该选项将被忽略。
- `never_despawn`：（布尔值，默认 `false`）如果为 `true`，该实体即使距离玩家很远也不会被移除。
- `radius`：（整数，默认 2，可用范围 2-6）该实体加载的区域半径。

## minecraft:timer

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
## minecraft:trade_table

定义该实体与玩家交易的能力。


- `display_name`：（字符串）与此实体交易时显示的名称。可以使用资源包中定义的语言文件节点。
- `table`：（字符串）从行为包根目录起始的交易表的相对路径，包含后缀 `.json`。使用左斜杠 `/` 分割路径。

```
{
    "display_name": "entity.villager.leather",
    "table": "trading/leather_worker_trades.json"
}
```
## minecraft:transformation

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
## minecraft:trusting

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
