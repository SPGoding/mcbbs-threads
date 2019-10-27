# 其他组件

这一类组件能完成各种杂七杂八的东西，不好分类，格式上也没有什么共同点，因此单独使用一章。

## minecraft:addrider

为该实体添加一个骑乘者。需要该实体具有 `minecraft:rideable` 组件。

- `entity_type`：（字符串）骑乘该实体的实体 ID。

示例（蜘蛛 `entities/spider.json`）：
```json
{
    "minecraft:addrider": {
        "entity_type": "minecraft:skeleton"
    }
}
```

## minecraft:ageable

为该实体添加一个用于成长的计时器。可以通过喂食 `feed_items` 中定义的物品来加速生长。

- `duration`：（数字）可选。该实体成年所需要的时间，单位为秒。默认为 `1200.0`。
- `drop_items`：（数组）可选。当该实体生长后要掉落的物品。
    - *（字符串）一个物品 ID*
- `feed_items`：（数组）可选。可以促进该实体生长的物品列表。
    - *（字符串）一个物品 ID*
    - *（对象）一个物品*
        - `item`：（字符串）物品名。
        - `growth`：（数字）喂食该物品能够促进的时间占总时间（`duration`）的比例。取值范围应在 `[0.0, 1.0]` 之间。
- `grow_up`：（对象，格式同触发器）在该实体长大的那一刻触发。

示例（海龟 `entities/turtle.json`）：
```json
{
    "minecraft:ageable": {
        "duration": 1200,
        "feed_items": [
            "seagrass"
        ],
        "drop_items": [
            "turtle_shell_piece"
        ],
        "grow_up": {
            "event": "minecraft:ageable_grow_up",
            "target": "self"
        }
    }
}
```

示例（羊驼 `entities/llama.json`）：
```json
{
    "minecraft:ageable": {
        "duration": 1200,
        "feed_items": [
            {
                "item": "wheat",
                "growth": 0.1
            },
            {
                "item": "hay_block",
                "growth": 0.9
            }
        ],
        "grow_up": {
            "event": "minecraft:ageable_grow_up",
            "target": "self"
        }
    }
}
```

## minecraft:angry

通过计时器定义实体的「愤怒」状态。

- `broadcastAnger`：（布尔）可选。如果为 `true`，其他相同种类并且有 `broadcastAnger` 的实体也会愤怒。默认为 `false`。
- `broadcastRange`：（数字）可选。也会愤怒的实体的最大距离。默认为 `20`。
- `calm_event`：（对象，格式同触发器）当实体不再愤怒时触发。
- `duration`：（数字）可选。该实体会保持愤怒的秒数。默认为 `25`。
- `duration_delta`：（数字）可选。添加到 `duration` 的变化量：[-delta, delta]。默认为 `0`。

示例（蜜蜂 `entities/bee.json`）：
```json
{
    "minecraft:angry": {
        "duration": 25,
        "broadcastAnger": true,
        "broadcastRange": 20,
        "calm_event": {
            "event": "calmed_down",
            "target": "self"
        }
    },
}
```

## minecraft:boostable

定义被拿着指定物品骑乘时，可骑乘实体速度的提升。

- `boost_items`：（数组）可以使该实体速度提升的物品列表。
    - *（对象）一个物品*
        - `item`：（字符串）物品 ID。
        - `damage`：（数字）可选。每次使用时消耗的物品耐久。默认为 `1`。
        - `replace_item`：（字符串）用于替换的物品 ID。当原物品耐久耗尽以后，会被替换为这个物品（例如骑乘猪时胡萝卜钓竿耐久用尽变成钓鱼竿）。
- `duration`：（数字）可选。该提升的持续时间。默认为 `3.0`。
- `speed_multiplier`：（数字）可选。在使用此组件时实体移动速度增加的倍数。`1.0` 表明速度未改变，`2.0` 表明二倍，诸如此类。默认为 `1.0`。

示例（猪 `entities/pig.json`）：
```json
{
    "minecraft:boostable": {
        "speed_multiplier": 2.0,
        "duration": 3.0,
        "boost_items": [
            {
                "item": "carrotOnAStick",
                "damage": 2,
                "replace_item": "fishing_rod"
            }
        ]
    }
}
```

## minecraft:breathable

定义该实体能够在其中呼吸/窒息的方块。

- `breathe_blocks`：（数组）该实体能在其中呼吸的方块。
    - *（字符串）一个方块 ID*
- `breathes_air`：（布尔）可选。如果为 `true`，该实体将能够在空气中呼吸。默认为 `true`。
- `breathes_lava`：（布尔）可选。如果为 `true`，该实体将能够在熔岩中呼吸。默认为 `false`。
- `breathes_solids`：（布尔）可选。如果为 `true`，该实体将能够在固体方块中呼吸。默认为 `false`。
- `breathes_water`：（布尔）可选。如果为 `true`，该实体将能够在水中呼吸。默认为 `false`。
- `generates_bubbles`：（布尔）可选。如果为 `true`，该实体在水中时将会产生气泡。默认为 `true`。
- `inhale_time`：（数字）可选。该实体回复满呼吸值所需秒数。默认为 `0.0`。
- `non_breathe_blocks`：（数组）该实体不能能在其中呼吸的方块。
    - *（字符串）一个方块 ID*
- `suffocate_time`：（数字）可选。在窒息伤害之间的秒数。默认为 `-20`。
- `total_supply`：（数字）可选。实体能屏住呼吸的秒数。默认为 `15`。

示例（海豚 `entities/dolphin.json`）：
```json
{
    "minecraft:breathable": {
        "total_supply": 240,
        "suffocate_time": 0,
        "breathes_air": true,
        "breathes_water": false,
        "generates_bubbles": false
    }
}
```

## minecraft:breedable

定义实体进入繁殖状态的方式。

- `allow_sitting`：（布尔）可选。如果为 `true`，该实体能在坐着的时候繁殖。默认为 `true`。
- `blend_attributes`：（布尔）可选。如果为 `true`，该实体会将性状融合到后代当中。例如，马在子代中融合了它们的血量、运动和跳跃力量。默认为 `true`。
- `breed_cooldown`：（数字）可选。该实体能再次繁殖的秒数。默认为 `60.0`。
- `breed_items`：（数组）能使实体进入繁殖状态的物品。
    - *（字符串）一个物品 ID*
- `breeds_with`：（数组或对象）能够和该实体繁殖的生物。
    - *（对象）*
        - `mate_type`：（字符串）配偶的实体 ID。
        - `baby_type`：（字符串）子代的实体 ID。
        - `breed_event`：（对象，格式同触发器）繁殖时触发。可以使用 `baby` 作为主体（`subject`）来指代出生的实体。
- `causes_pregnancy`：（布尔）可选。如果为 `true`，该实体会怀孕，而不是立刻生下子代。默认为 `false`。
- `environment_requirements`：（数组）对附近的环境要求。
    - *（对象）*
        - `block`：（字符串）要求的方块类型。
        - `count`：（数字）要求的方块最低数量。
        - `radius`：（数字）检查的半径，取值应在 [0, 16] 之间。
- `extra_baby_chance`：（数字）可选。出生多个生物的几率，最多出生 16 个。取值应在 [0.0, 1.0] 之间。默认为 `0.0`。
- `inherit_tamed`：（布尔）可选。如果为 `true`，在父母已被驯服时，子代会被自动驯服。默认为 `true`。
- `mutation_factor`：（对象）变异。
    - `color`：（数字。默认 0.0）实体颜色变异的几率。取值应在 [0.0, 1.0] 之间。
    - `extra_variant`：（数字。默认 0.0）实体拓展变种变异的几率。取值应在 [0.0, 1.0] 之间。
    - `variant`：（数字。默认 0.0）实体变种变异的几率。取值应在 [0.0, 1.0] 之间。
- `require_tame`：（布尔）可选。如果为 `true`，该实体在被繁殖前需要被驯服。默认为 `true`。

示例（蜜蜂 `entities/bee.json`）：
```json
{
    "minecraft:breedable": {
        "require_tame": false,
        "breeds_with": {
            "mate_type": "minecraft:bee",
            "baby_type": "minecraft:bee",
            "breed_event": {
                "event": "minecraft:entity_born",
                "target": "baby"
            }
        },
        "breed_items": [ 
            "minecraft:red_flower",     // All small flowers except Dandelion
            "minecraft:yellow_flower",  // Dandelion
            "minecraft:wither_rose",
            "minecraft:double_plant:0", // Sunflower
            "minecraft:double_plant:1", // Lilac
            "minecraft:double_plant:4", // Rose Bush
            "minecraft:double_plant:5"  // Peony
        ]
    }
}
```
## minecraft:bribeable

当该实体被喂食指定物品后可以将玩家带到附近宝藏。由海豚使用。

- `bribe_cooldown`：（数字）可选。距离该实体能再次被喂食物品的秒数。默认为 `60.0`。
- `bribe_items`：（数组）能用来喂食的物品。
    - *（字符串）一个物品 ID*

示例（海豚 `entities/.json`）：
```json
{
    "bribe_items": [
        "fish",
        "salmon"
    ]
}
```
## minecraft:damage_over_time

使实体每隔指定时间后受到指定伤害。

- `damage_per_hurt`：（数字）可选。每次伤害的伤害量。默认为 `1`。
- `time_between_hurt`：（数字）可选。两次伤害的间隔秒数。默认为 `0`。

示例（海豚 `entities/.json`）：
```json
{
    "minecraft:damage_over_time": {
        "damage_per_hurt": 1,
        "time_between_hurt": 0
    }
}
```
## minecraft:damage_sensor

当实体被指定物品或实体伤害时执行指定事件。

- `triggers`：（对象）
    - `cause`：（字符串）能够执行该事件的伤害的类型。所有伤害类型可以参考滤器中的  `has_damage` 测试项目的介绍。
    - `deals_damage`：（布尔）可选。如果为 `true`，该实体会受到该伤害；设置为 `false` 会让实体忽略该伤害。默认为 `true`。
    - `on_damage`：（对象，格式同触发器）当受到伤害时触发。可以在滤器的主体中使用 `other` 来表示造成该伤害的实体。

示例（苦力怕 `entities/creeper.json`）：
```json
{
    "minecraft:damage_sensor": {
        "triggers": {
            "on_damage": {
                "filters": {
                    "test": "is_family",
                    "subject": "other",
                    "value": "lightning"
                },
                "event": "minecraft:become_charged"
            },
            "deals_damage": false
        }
    }
}
```

## minecraft:economy_trade_table

定义该实体与玩家交易的能力。似乎只被新版村民使用。另见旧版村民使用的 `minecraft:trade_table` 组件。

- `display_name`：（字符串）与此实体交易时显示的名称。
- `table`：（字符串）从行为包根目录起始的交易表的相对路径，包含后缀 `.json`。使用左斜杠 `/` 分割路径。
- `new_screen`：（布尔）是否使用新的交易 UI。
- `persist_trades`：（布尔）不明。

示例（新版村民 `entities/villager_2.json`）：
```json
{
    "minecraft:trade_table": {
        "display_name": "entity.villager.farmer",
        "table": "trading/economy_trades/farmer_trades.json",
        "new_screen": true,
        "persist_trades": true
    }
}
```

## minecraft:environment_sensor

创建一个基于环境条件的感应器。

- `triggers`：（数组或对象）存放所有触发器。

示例（尸壳 `entities/husk.json`）：
```json
{
    "minecraft:environment_sensor": {
        "triggers": {
            "filters": { "test": "is_underwater", "subject": "self", "operator": "==", "value": true },
            "event": "minecraft:start_transforming"
        }
    }
}
```
## minecraft:equippable

允许该实体装备物品。

- `slots`：（数组）可以装备物品的栏位。
    - *（对象）*
        - `slot`：（数字）该栏位的 ID。
        - `item`：（字符串）可以放到该栏位的物品的 ID。
        - `accepted_items`：（数组）可以放到该栏位的所有物品。
            - *（字符串）一个物品 ID*
        - `on_equip`：（对象，格式同触发器）当该栏位被装备物品时触发。
        - `on_unequip`：（对象，格式同触发器）当该栏位被取下物品时触发。
        - `interact_text`：（字符串）触屏模式下与该实体交互时显示的文本。

示例（驴 `entities/donkey.json`）：
```json
{
    "minecraft:equippable": {
        "slots": [
            {
                "slot": 0,
                "item": "saddle",
                "accepted_items": ["saddle"],
                "on_equip": {
                    "event": "minecraft:donkey_saddled"
                },
                "on_unequip": {
                    "event": "minecraft:donkey_unsaddled"
                }
            }
        ]
    }
}
```
## minecraft:explode

定义该实体的爆炸方式。

- `breaks_blocks`：（布尔）可选。如果为 `true`，该爆炸会摧毁爆炸半径内的方块。默认为 `true`。
- `causesFire`：（布尔）可选。如果为 `true`，爆炸半径内的方块会被点燃。默认为 `false`。
- `destroyAffectedByGriefing`：（布尔）可选。如果为 `true`，是否破坏方块会受到游戏规则 `mobGriefing` 的影响。默认为 `false`。
- `fireAffectedByGriefing`：（布尔）可选。如果为 `true`，是否产生火会受到游戏规则 `mobGriefing` 的影响。默认为 `false`。
- `fuseLength`：（数组，默认为 `[0.0， 0.0]`）自点燃后爆炸前的时间范围。负数会使其立刻爆炸。
- `fuseLit`：（布尔）可选。如果为 `true`，当该组件被添加时立刻点燃。默认为 `false`。
- `maxResistance`：（数字）可选。爆炸抗性比这个值低的方块会被破坏。默认为 `Infinite`。
- `power`：（数字）可选。爆炸半径，及爆炸的伤害。默认为 ``4.0``。

示例（苦力怕 `entities/creeper.json`）：
```json
{
    "minecraft:explode": {
        "fuseLength": 1.5,
        "fuseLit": true,
        "power": 3,
        "causesFire": false,
        "destroyAffectedByGriefing": true
    }
}
```

## minecraft:healable

定义治疗该实体的交互。

- `force_use`：（布尔）可选。定义物品能否在该实体血量为满时使用。默认为 `false`。
- `items`：（数组）能够治疗此实体的物品。
    - *（对象）一个物品*
        - `item`：（字符串）物品的 ID。
        - `heal_amount`：（数字）可选。使用此物品时对实体的治疗量。默认为 `1.0`。
        - `filters`：（对象）可选。应用治疗效果前的滤器。

示例（猫 `entities/cat.json`）：
```json
{
    "minecraft:healable": {
        "items": [
            {
                "item": "fish",
                "heal_amount": 2
            },
            {
                "item": "salmon",
                "heal_amount": 2
            }
        ]
    }
}
```

## minecraft:insomnia

添加一个计时器记录玩家上一次睡觉的时间（被原版游戏用于决定是否生成幻翼）。

- `days_until_insomnia`：（数字）可选。玩家需要修仙的天数。默认为 `4.0`。

示例（玩家 `entities/player.json`）：
```json
{
    "minecraft:insomnia": {
        "days_until_insomnia": 3
    }
}
```

## minecraft:interact

定义与此实体的交互行为。

- `add_items`：（对象）可选。成功交互以后要添加到玩家物品栏的战利品表。
    - `table`：（字符串）战利品表的相对路径，从行为包的根目录起始，包含 `.json` 后缀。
- `spawn_items`：（对象）可选。成功交互以后要生成的物品实体。
    - `table`：（字符串）战利品表的相对路径，从行为包的根目录起始，包含 `.json` 后缀。
- `cooldown`：（数字）可选。该实体再次能被交互的秒数。默认为 `0.0`。
- `hurt_item`：（数字）可选。使用物品交互时物品损失的耐久。设置为 0 则不会损失任何耐久。默认为 `0`。
- `interact_text`:（字符串）可选。触屏模式下与该实体交互时显示的文本。
- `on_interact`：（对象，格式同触发器）交互时触发。
- `particle_on_start`：（对象）在开始交互时产生的粒子效果。
    - `particle_offset_towards_interactor`：（布尔）粒子是否会靠近与该实体交互的玩家。
    - `particle_type`：（字符串）会被显示的粒子。
    - `particle_y_offset`：（数字）粒子在 y 轴上的偏转方向。
- `play_sounds`：（字符串）交互发生时播放的音效的名称。
- `spawn_entities`：（字符串）交互发生时生成的实体。
- `swing`：（布尔）可选。如果为 `true`，玩家与该实体交互时会做「摆手」的动画。默认为 `true`。
- `use_item`：（布尔）可选。如果为 `true`，该交互将会使用玩家手中的物品。默认为 `false`。
- `transform_to_item`：（字符串）可选。使用的物品会被转换为指定物品。格式为 `<物品名>:<数据值>`。

示例（牛 `entities/cow.json`）：
```json
{
    "minecraft:interact": {
        "interactions": [
            {
                "on_interact": {
                    "filters": {
                        "all_of": [
                            { "test": "is_family", "subject": "other", "value": "player" },
                            { "test": "has_equipment", "domain": "hand", "subject": "other", "value": "bucket:0" }
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
```
## minecraft:inventory

定义该实体的物品栏属性。

- `additional_slots_per_strength`：（数字）可选。该实体每获得一点力量可获得的栏位数量（力量可以通过 `minecraft:strength` 属性组件定义）。默认为 `0`。
- `container_type`：（字符串）该实体的容器类型。可以是 `horse`（马）、`minecart_chest`（箱子矿车）、`minecart_hopper`（漏斗矿车）、`inventory`（背包）、`container`（容器）或 `hopper`（漏斗）。
- `inventory_size`：（数字）可选。定义该容器具有的物品栏位的数量。默认为 `5`。
- `can_be_siphoned_from`：（布尔）可选。如果为 `true`，该实体中的物品可以被漏斗取出。默认为 `false`。
- `private`：（布尔）可选。如果为 `true`，只有该实体自身可以打开它的物品栏。默认为 `false`。
- `restrict_to_owner`：（布尔）可选。如果为 `true`，只有该实体或它的主人可以打开它的物品栏。默认为 `false`。

示例（箱子矿车 `entities/chest_minecart.json`）：
```json
{
    "minecraft:inventory": {
        "container_type": "minecart_chest",
        "inventory_size": 27,
        "can_be_siphoned_from": true
    }
}
```

## minecraft:item_hopper

定义该实体为一个漏斗。被原版的漏斗矿车使用。

## minecraft:jump.dynamic

定义一个动态的跳跃高度控制。将会基于实体的速度修饰器改变跳跃的属性。被原版的兔子使用。

## minecraft:jump.static

给予该实体跳跃的能力。

- `jump_power`：（数字）可选。跳跃的初始垂直速度。默认为 `0.42`。

示例（蝙蝠 `entities/bee.json`）：
```json
{
    "minecraft:jump.static": {}
}
```

## minecraft:leashable

使该实体可以被栓绳拴住。

- `soft_distance`：（数字）可选。栓绳开始有弹性的距离。从这个距离开始该实体会跟着拴着它的实体移动。默认为 `4.0`。
- `hard_distance`：（数字）可选。栓绳开始变硬的距离。该实体的运动范围会被限制在这个距离之内。默认为 `6.0`。
- `max_distance`：（数字）可选。栓绳断开的距离。默认为 `10.0`。
- `on_leash`：（对象，格式同触发器）可选。当该实体被拴住时触发。
- `on_unleash`：（对象，格式同触发器）可选。当该实体被解拴时触发。

示例（蜜蜂 `entities/bee.json`）：
```json
{
    "minecraft:leashable": {
        "soft_distance": 4.0,
        "hard_distance": 6.0,
        "max_distance": 10.0
    }
}
```

## minecraft:lookat

定义当另一个实体看向这个实体时的行为。

- `filters`：（对象）定义能够触发此组件的实体的滤器。可以使用在主体（`subject`）中使用 `other` 来指代看向当前实体的实体。
- `look_cooldown`：（数字）该实体不再愤怒的时间。
- `look_event`：（对象，格式同触发器）当有满足条件的实体看向该实体时触发。
- `mAllowInvulnerable`：（布尔）可选。如果为 `true`，无敌状态的实体（例如创造模式的玩家）也会触发。默认为 `false`。
- `search_radius`：（数字）可选。搜寻看向该实体的实体的半径。默认为 `10.0`。
- `set_target`：（布尔）可选。如果为 `true`，该实体会将看向它的实体作为攻击目标。默认 `true`。

示例（末影人 `entities/enderman.json`）：
```json
{
    "minecraft:lookat": {
        "search_radius": 64.0,
        "set_target": true,
        "look_cooldown": 5.0,
        "filters": {
            "all_of": [
                { "subject": "other", "test": "is_family", "value": "player" },
                { "test": "has_equipment", "domain": "head", "subject": "other", "operator": "not", "value": "carved_pumpkin" }
            ]
        }
    }
}
```
## minecraft:movement.amphibious

允许该实体在水中游泳或在陆地上行走。

- `max_turn`：（数字）可选。该生物一刻内能最大转过的度数。默认为 `30.0`。

## minecraft:movement.basic

实体的基础移动组件。

- `max_turn`：（数字）可选。该生物一刻内能最大转过的度数。默认为 `30.0`。

## minecraft:movement.fly

允许该实体飞行。

- `max_turn`：（数字）可选。该生物一刻内能最大转过的度数。默认为 `30.0`。

## minecraft:movement.generic

允许该实体飞行、游泳、爬行等。

- `max_turn`：（数字）可选。该生物一刻内能最大转过的度数。默认为 `30.0`。

## minecraft:movement.jump

允许该实体跳跃。

- `jump_delay`：（数组）可选。两次跳跃之间的间隔时间。默认 `[0.0, 0.0]`。
- `max_turn`：（数字）可选。该生物一刻内能最大转过的度数。默认为 `30.0`。

## minecraft:movement.skip

允许实体在移动时跳跃。

- `max_turn`：（数字）可选。该生物一刻内能最大转过的度数。默认为 `30.0`。

## minecraft:movement.sway

使实体左右摇摆，给人以游泳的感觉。

- `max_turn`：（数字）可选。该生物一刻内能最大转过的度数。默认为 `30.0`。

## minecraft:nameable

允许该实体被命名（如使用命名牌）。

- `allowNameTagRenaming`：（布尔）可选。如果为 `true`，该实体可以被命名牌命名。默认 `true`。
- `alwaysShow`：（布尔）可选。如果为 `true`，该实体会一直显示着名称。默认 `false`。
- `default_trigger`：（字符串）当该实体被命名，且名称不满足 `name_actions` 中定义的特殊名字时时执行的触发器。
- `name_actions`：（数组）当实体有特殊的名称时触发的触发器。
    - *（对象）*
        - `name_filter`：（字符串或数组）能够触发 `on_named` 中定义的触发器的特殊的名字。
        - `on_named`：（对象，格式同触发器）当实体拥有 `name_filter` 中定义的一个名字时触发。

示例（卫道士 `entities/vindicator.json`）：
```json
{
    "minecraft:nameable": {
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
}
```

## minecraft:navigation.*

下面是有关寻路的一些组件。「寻路」是实体在移动前对自己移动路线的规划。「规划」和「实际」有可能会不一致，例如你在寻路组件中允许该实体通过门，那么实体在规划路线时就有可能穿过门，但假设你没有在 AI 目标组件中允许该实体开门，那么实体到了门前以后只会傻站着，而不会像它原先规划地那样通过门。换句话说就是，实体在规划路径时以为自己可以开门，但真正到了门前才发现自己菜到连门都打不开（雾。

### minecraft:navigation.climb

使实体规划的路径能够垂直穿过的方块（如原版的蜘蛛爬墙）。

- `avoid_portals`：（布尔）可选。如果为 `true`，该实体规划的路径能够穿过传送门（如地狱传送门）。默认为 `false`。
- `avoid_sun`：（布尔）可选。如果为 `true`，该实体规划的路径能够躲避开阳光。默认为 `false`。
- `avoid_water`：（布尔）可选。如果为 `true`，该实体规划的路径能够避开水。默认为 `false`。
- `can_open_doors`：（布尔）可选。如果为 `true`，该实体规划的路径能够穿过关上的门。默认为 `false`。
- `can_pass_doors`：（布尔）可选。如果为 `true`，该实体规划的路径能够穿过打开的门。默认为 `true`。
- `can_path_over_water`：（布尔）可选。如果为 `true`，该实体规划的路径能够通过水面。默认为 `false`。
- `can_sink`：（布尔）可选。如果为 `true`，该实体规划的路径会在水里受到重力影响下沉。默认为 `true`。

### minecraft:navigation.float

使实体规划的路径能飘在空中（如原版的恶魂）。

- `avoid_portals`：（布尔）可选。如果为 `true`，该实体规划的路径能够穿过传送门（如地狱传送门）。默认为 `false`。
- `avoid_sun`：（布尔）可选。如果为 `true`，该实体规划的路径能够躲避开阳光。默认为 `false`。
- `avoid_water`：（布尔）可选。如果为 `true`，该实体规划的路径能够避开水。默认为 `false`。
- `can_open_doors`：（布尔）可选。如果为 `true`，该实体规划的路径能够穿过关上的门。默认为 `false`。
- `can_pass_doors`：（布尔）可选。如果为 `true`，该实体规划的路径能够穿过打开的门。默认为 `true`。
- `can_path_over_water`：（布尔）可选。如果为 `true`，该实体规划的路径能够通过水面。默认为 `false`。
- `can_sink`：（布尔）可选。如果为 `true`，该实体规划的路径会在水里受到重力影响下沉。默认为 `true`。

### minecraft:navigation.fly

使实体规划的路径能飞（如原版的鹦鹉）。

- `avoid_portals`：（布尔）可选。如果为 `true`，该实体规划的路径能够穿过传送门（如地狱传送门）。默认为 `false`。
- `avoid_sun`：（布尔）可选。如果为 `true`，该实体规划的路径能够躲避开阳光。默认为 `false`。
- `avoid_water`：（布尔）可选。如果为 `true`，该实体规划的路径能够避开水。默认为 `false`。
- `can_open_doors`：（布尔）可选。如果为 `true`，该实体规划的路径能够穿过关上的门。默认为 `false`。
- `can_pass_doors`：（布尔）可选。如果为 `true`，该实体规划的路径能够穿过打开的门。默认为 `true`。
- `can_path_over_water`：（布尔）可选。如果为 `true`，该实体规划的路径能够通过水面。默认为 `false`。
- `can_sink`：（布尔）可选。如果为 `true`，该实体规划的路径会在水里受到重力影响下沉。默认为 `true`。

### minecraft:navigation.generic

使实体规划的路径能行走、游泳、飞行和/或爬行以及跳上方块/走下方块。

- `avoid_portals`：（布尔）可选。如果为 `true`，该实体规划的路径能够穿过传送门（如地狱传送门）。默认为 `false`。
- `avoid_sun`：（布尔）可选。如果为 `true`，该实体规划的路径能够躲避开阳光。默认为 `false`。
- `avoid_water`：（布尔）可选。如果为 `true`，该实体规划的路径能够避开水。默认为 `false`。
- `can_open_doors`：（布尔）可选。如果为 `true`，该实体规划的路径能够穿过关上的门。默认为 `false`。
- `can_pass_doors`：（布尔）可选。如果为 `true`，该实体规划的路径能够穿过打开的门。默认为 `true`。
- `can_path_over_water`：（布尔）可选。如果为 `true`，该实体规划的路径能够通过水面。默认为 `false`。
- `can_sink`：（布尔）可选。如果为 `true`，该实体规划的路径会在水里受到重力影响下沉。默认为 `true`。

### minecraft:navigation.swim

使实体规划的路径能包括水。

- `avoid_portals`：（布尔）可选。如果为 `true`，该实体规划的路径能够穿过传送门（如地狱传送门）。默认为 `false`。
- `avoid_sun`：（布尔）可选。如果为 `true`，该实体规划的路径能够躲避开阳光。默认为 `false`。
- `avoid_water`：（布尔）可选。如果为 `true`，该实体规划的路径能够避开水。默认为 `false`。
- `can_open_doors`：（布尔）可选。如果为 `true`，该实体规划的路径能够穿过关上的门。默认为 `false`。
- `can_pass_doors`：（布尔）可选。如果为 `true`，该实体规划的路径能够穿过打开的门。默认为 `true`。
- `can_path_over_water`：（布尔）可选。如果为 `true`，该实体规划的路径能够通过水面。默认为 `false`。
- `can_sink`：（布尔）可选。如果为 `true`，该实体规划的路径会在水里受到重力影响下沉。默认为 `true`。

### minecraft:navigation.walk

使实体规划的路径能在地面上跳跃或走下方块（如常见的那些生物）。

- `avoid_portals`：（布尔）可选。如果为 `true`，该实体规划的路径能够穿过传送门（如地狱传送门）。默认为 `false`。
- `avoid_sun`：（布尔）可选。如果为 `true`，该实体规划的路径能够躲避开阳光。默认为 `false`。
- `avoid_water`：（布尔）可选。如果为 `true`，该实体规划的路径能够避开水。默认为 `false`。
- `can_open_doors`：（布尔）可选。如果为 `true`，该实体规划的路径能够穿过关上的门。默认为 `false`。
- `can_pass_doors`：（布尔）可选。如果为 `true`，该实体规划的路径能够穿过打开的门。默认为 `true`。
- `can_path_over_water`：（布尔）可选。如果为 `true`，该实体规划的路径能够通过水面。默认为 `false`。
- `can_sink`：（布尔）可选。如果为 `true`，该实体规划的路径会在水里受到重力影响下沉。默认为 `true`。

## minecraft:peek

定义实体的「窥视」行为。

- `on_close`：（对象，格式同触发器）可选。当实体结束窥视时触发。
- `on_open`：（对象，格式同触发器）可选。当实体开始窥视时触发。
- `on_target_open`：（对象，格式同触发器）可选。当该实体的目标开始窥视时触发（存疑，Wiki 上是这么写的）。

示例（潜影贝 `entities/shulker.json`）：
```json
{
    "minecraft:peek": {
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
}
```

## minecraft:projectile

定义该实体为弹射物（如原版的鸡蛋、投掷药水等）。

> 以下列表仅供参考。这个组件太复杂，我也找不到靠谱的官方文档。所以说基岩版的文档和*一样。

- `angleoffset`：（数字）可选。定义该实体的抛出角度。默认为 `0.0`。
- `catch_fire`：（布尔）可选。如果为 `true`，该实体击中的实体会被点燃。默认为 `false`。
- `crit_particle_on_hurt`：（布尔）可选。如果为 `true`，在会心一击时会产生更多粒子。默认为 `false`。
- `destroy_on_hurt`：（布尔）可选。如果为 `true`，该实体撞击以后会消失。默认为 `false`。
- `reflect_on_hurt`：（布尔）可选。如果为 `true`，该实体撞击时会反弹。默认为 `false`。
- `filter`：（字符串）不能被该弹射物击中的实体。
- `fire_affected_by_griefing`：（布尔）可选。如果为 `true`，该实体是否会产生火将受到 `mobGriefing` 游戏规则的影响。默认为 `false`。
- `gravity`：（数字）可选。定义该实体的重力。数字越大，该实体下落越快。默认为 `0.05`。
- `hit_sound`：（字符串）该实体撞击时的音效。
- `homing`：（布尔）可选。如果为 `true`，该实体会追踪最近的实体。默认为 `false`。
- `inertia`：（数字）可选。该实体每在空中一刻所剩下的速度的比值。默认为 `0.99`。
- `isdangerous`：（布尔）可选。如果为 `true`，该实体会被视作对玩家有威胁。默认为 `false`。
- `knockback`：（布尔）可选。如果为 `true`，该实体会击退它击中的实体。默认为 `true`。
- `lightning`：（布尔）可选。如果为 `true`，被击中的实体会被雷劈。默认为 `false`。
- `liquid_inertia`：（数字）可选。该实体每在水中一刻所剩下的速度的比值。默认为 `0.6`。
- `multipletargets`：（布尔）可选。如果为 `true`，该实体一次飞行可以击中多个实体。默认为 `false`。
- `offset`：（数组）可选。该实体生成时的偏移位置。默认 `[0.0, 0.5, 0.0]`。
- `on_fire_time`：（数字）可选。被击中的实体着火的秒数。默认为 `5.0`。
- `particle`：（字符串）可选。击中时的粒子效果。默认 `iconcrack`。
- `potion_effect`：（数字）可选。被击中的实体被附加的状态效果。默认为 `-1`。
- `power`：（数字）可选。该实体的速度。默认为 `1.3`。
- `semirandomdiffdamage`：（布尔）可选。如果为 `true`，伤害量会基于伤害和速度随机施加。默认为 `false`。
- `shoot_sound`：（字符串）该实体被射出时的音效。
- `shoottarget`：（布尔）可选。如果为 `true`，该弹射物会射向攻击实体的实体。默认为 `true`。
- `shouldbounce`：（布尔）可选。如果为 `true`，该实体会在撞击时弹跳。默认为 `false`。
- `splash_potion`：（布尔）可选。如果为 `true`，该实体会被认为是喷溅药水。默认为 `false`。
- `splash_range`：（数字）可选。喷溅效果的范围。默认为 `4.0`。
- `uncertainty_base`：（数字）可选。基准精度。精度由公式 `uniqueBase - <游戏难度> * uncertaintyMultiplier` 计算。默认为 `0.0`。
- `uncertainty_multiplier`：（数字）可选。决定游戏难度对精度的影响。精度由公式 `uniqueBase - <游戏难度> * uncertaintyMultiplier` 计算。默认为 `0.0`。

示例（箭 `entities/arrow.json`）：
```json
{
    "minecraft:projectile": {
        "on_hit": {
            "definition_event": {
                "affect_projectile": true,
                "event_trigger": {
                    "event": "minecraft:explode",
                    "target": "self"
                }
            }
        },
        "power": 1.6,
        "gravity": 0.00,
        "inertia": 1,
        "liquid_inertia": 1,
        "uncertainty_base": 0,
        "uncertainty_multiplier": 0,
        "anchor": 1,
        "offset": [ 0, -0.1, 0 ],
        "reflect_on_hurt": true,
        "catch_fire": true
    }
}
```

## minecraft:rail_movement

定义该实体在铁轨上的移动。拥有该组件的实体将只能在铁轨上移动。

- `max_speed`：（数字）可选。该实体在铁轨上移动时的最大速度。默认为 `0.4`。

## minecraft:rail_sensor

定义当铁轨被激活/取消激活时该实体的行为。

- `check_block_types`：（布尔）可选。如果为 `true`，该实体会在每一刻触发 `on_deactivate` 事件。默认为 `false`。
- `eject_on_activate`：（布尔）可选。如果为 `true`，该实体会在铁轨激活时将所有乘客丢出。默认为 `true`。
- `eject_on_deactivate`：（布尔）可选。如果为 `true`，该实体会在铁轨取消激活时将所有乘客丢出。默认为 `false`。
- `on_activate`：（对象，格式同触发器）当铁轨激活时触发。
- `on_deactivate`：（对象，格式同触发器）当铁轨取消激活时触发。
- `tick_command_block_on_activate`：（布尔）可选。如果为 `true`，该实体会在通过激活铁轨时每刻执行命令方块。默认为 `true`。
- `tick_command_block_on_deactivate`：（布尔）可选。如果为 `true`，该实体会在通过非激活的铁轨时每刻执行命令方块。默认为 `false`。

示例（命令方块矿车 `entities/command_block_minecart.json`）：
```json
{
    "minecraft:rail_sensor": {
        "check_block_types": true,
        "eject_on_activate": false,
        "eject_on_deactivate": false,
        "tick_command_block_on_activate": true,
        "tick_command_block_on_deactivate": false,
        "on_deactivate": {
            "event": "minecraft:command_block_deactivate"
        }
    }
}
```
## minecraft:rideable

允许该实体被骑乘。可以定义多个不同位置的座位。

- `seat_count`：（数字）可选。能同时骑乘该实体的最大实体数目。默认为 `1`。
- `controlling_seat`：（数字）可选。能够控制该实体移动方向的座位在 `seats` 数组中的索引。默认为 `0`。
- `crouching_skip_interact`：（布尔）可选。如果为 `true`，潜行时不能与该实体交互。默认为 `true`。
- `family_types`：（数组）能够骑乘该实体的实体。
    - *（字符串）一个实体 ID*
- `pull_in_entities`：（布尔）可选。如果为 `true`，该实体会将符合条件的实体拉入任一可用的座位。默认为 `false`。
- `interact_text`：（字符串）使用触摸屏与该实体交互时显示的文字。
- `rider_can_interact`：（布尔）可选。如果为 `true`，乘客可以通过看向该实体与之交互。默认为 `false`。
- `seats`：（数组）座位列表。
    - *（对象）一个座位*
        - `lock_rider_rotation`：（数字）可选。当骑乘该实体时乘客所能转向的角度限制。如不指定则无限制。默认为 `181.0`。
        - `max_rider_count`：（数字）可选。当骑乘该实体的实体数量小于此值时，该座位才可用。默认为 `0`。
        - `min_rider_count`：（数字）可选。当骑乘该实体的实体数量大于此值时，该座位才可用。默认为 `0`。
        - `position`：（数组）可选。座位相对于该实体位置的位置。默认 `[0.0, 0.0, 0.0]`。
        - `rotate_rider_by`：（数字）可选。乘客旋转的偏移角度。默认为 `0.0`。

示例（船 `entities/boat.json`）：
```json
{
    "minecraft:rideable": {
        "seat_count": 2,
        "interact_text": "action.interact.ride.boat",
        "pull_in_entities": true,
        "seats": [
            {
                "position": [ 0.0, -0.2, 0.0 ],
                "min_rider_count": 0,
                "max_rider_count": 1,
                "rotate_rider_by": -90,
                "lock_rider_rotation": 90
            },
            {
                "position": [ 0.2, -0.2, 0.0 ],
                "min_rider_count": 2,
                "max_rider_count": 2,
                "rotate_rider_by": -90,
                "lock_rider_rotation": 90
            },
            {
                "position": [ -0.6, -0.2, 0.0 ],
                "min_rider_count": 2,
                "max_rider_count": 2,
                "lock_rideer_rotation": 90
            }
        ]
    }
}
```

## minecraft:scaffolding_climber

允许在脚手架上爬行。只能被玩家使用。

## minecraft:scale_by_age

定义基于年龄的该实体大小的缩放。

- `start_scale`：（数字）可选。新生实体的起始缩放倍数。默认为 `1.0`。
- `end_scale`：（数字）可选。实体完全成熟时的终止缩放倍数。默认为 `1.0`。

示例（海豚 `entities/dolphin.json`）：
```json
{
    "minecraft:scale_by_age": {
        "start_scale": 0.5,
        "end_scale": 1.0
    }
}
```
## minecraft:shareables

定义该实体想要交换的物品。

- `items`：（数组）想要交换的物品列表。
    - *（对象）一个物品*
        - `item`：（字符串）该实体想要的物品。
        - `craft_into`：（字符串）该实体能给出的物品。
        - `want_amount`：（数字）该实体能给出的物品的数量（存疑。Wiki 上是这么写的）。
        - `surplus_amount`：（数字）该实体能给出的额外物品的数量。

示例（新版村民 `entities/villager_v2.json`）：
```json
{
    "minecraft:shareables": {
        "items": [
            {
                "item": "minecraft:bread",
                "want_amount": 3,
                "surplus_amount": 6
            },
            {
                "item": "minecraft:carrot",
                "want_amount": 60,
                "surplus_amount": 4
            },
            {
                "item": "minecraft:potato",
                "want_amount": 60,
                "surplus_amount": 24
            },
            {
                "item": "minecraft:beetroot",
                "want_amount": 60,
                "surplus_amount": 24
            },
            {
                "item": "minecraft:wheat_seeds",
                "want_amount": 64,
                "surplus_amount": 64
            },
            {
                "item": "minecraft:beetroot_seeds",
                "want_amount": 64,
                "surplus_amount": 64
            },
            {
                "item": "minecraft:wheat",
                "want_amount": 45,
                "surplus_amount": 18,
                "craft_into": "minecraft:bread"
            }
        ]
    }
}
```

## minecraft:shooter

定义该实体的远程攻击表现。

- `type`：（字符串）不明。
- `def`：（字符串）作为远程攻击的弹射物的实体 ID。指定的实体必须有 `minecraft:projectile` 组件。

示例（烈焰人 `entities/blaze.json`）：
```json
{
    "minecraft:shooter": {
        "type": "smallfireball",
        "def": "minecraft:small_fireball"
    }
}
```

## minecraft:sittable

定义该实体的「坐姿」状态。

- `sit_event`：（对象，格式同触发器）当该实体坐下时触发。
- `stand_event`：（对象，格式同触发器）当该实体站起时触发。

## minecraft:spawn_entity

该实体在一段时间后会生成另一个实体。

- `min_wait_time`：（数字）可选。随机生成另一个实体的最短秒数。默认为 `300`。
- `max_wait_time`：（数字）可选。随机生成另一个实体的最长秒数。默认为 `600`。
- `spawn_sound`：（字符串）可选。实体生成后播放的音效。默认为 `plop`。
- `spawn_entity`：（字符串）可选。生成实体的 ID。如果留空则会依据 `spawn_item` 生成物品。
- `spawn_item`：（字符串）可选。生成的物品的名字。仅在 `spawn_entity` 未被指定时有效。默认为 `egg`（鸡蛋）。
- `spawn_event`：（字符串）可选。当该实体被生成时其触发的事件。默认为 `minecraft:entity_born`。
- `spawn_method`：（字符串）可选。生成该实体的方式。默认为 `born`。
- `filters`：（对象，格式同滤器）可选。需要满足该滤器才能生成出指定实体。

示例（鸡 `entities/chicken.json`）：
```json
{
    "minecraft:spawn_entity": {
        "min_wait_time": 300,
        "max_wait_time": 600,
        "spawn_sound": "plop",
        "spawn_item": "egg",
        "filters": {
            "test": "rider_count", "subject": "self", "operator": "==", "value": 0
        }
    }
}
```

## minecraft:tameable

允许该实体被玩家驯服。

- `probability`：（数字）可选。每使用一个物品使该实体被驯服的可能性。取值应在 `[0.0, 1.0]` 之中。默认为 `1.0`。
- `tame_items`：（数组）能够驯服该实体的物品列表。
    - *（字符串）一个物品 ID*
- `tame_event`：（对象，格式同触发器）当该实体被驯服时触发。

示例（ `entities/.json`）：
```json
{
    "minecraft:tameable": {
        "probability": 0.33,
        "tame_items": [ "fish", "salmon" ],
        "tame_event": {
            "event": "minecraft:on_tame",
            "target": "self"
        }
    }
}
```
## minecraft:tamemount

允许该实体能通过骑乘被驯服。

- `min_temper`：（数字）可选。实体 temper 的随机初始值的最小值。默认为 `0`。
- `max_temper`：（数字）可选。实体 temper 的随机初始值的最大值。当 temper 到达此值时即可被驯服。默认为 `100`。
- `attempt_temper_mod`：（数字）可选。每次尝试骑乘时增加的 temper。默认为 `5`。
- `auto_reject_items`：（对象）该实体不喜欢的物品。当携带这些物品与实体交互时会使其愤怒。
    - `item`：（字符串）一个物品 ID。
- `feed_items`：（数组）该实体喜欢的物品。可以喂食这些物品提升实体的 temper，加速驯化过程。
    - *（对象）一个物品*
        - `item`：（字符串）物品 ID。
        - `temper_mod`：（数字）可选。当喂食此物品时增加的 temper。默认为 `0`。
- `feed_text`：（字符串）显示在喂食交互按钮上的文本。
- `ride_text`：（字符串）显示在骑乘交互按钮上的文本。
- `tame_event`：（对象，格式同触发器）当成功被驯服时触发。

示例（驴 `entities/donkey.json`）：
```json
{
    "minecraft:tamemount": {
        "min_temper": 0,
        "max_temper": 100,
        "feed_text": "action.interact.feed",
        "ride_text": "action.interact.mount",
        "feed_items": [
            {
                "item": "wheat",
                "temper_mod": 3
            },
            {
                "item": "sugar",
                "temper_mod": 3
            },
            {
                "item": "apple",
                "temper_mod": 3
            },
            {
                "item": "golden_carrot",
                "temper_mod": 5
            },
            {
                "item": "golden_apple",
                "temper_mod": 10
            },
            {
                "item": "appleEnchanted",
                "temper_mod": 10
            }
        ],
        "auto_reject_items": [
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
}
```

## minecraft:target_nearby_sensor

定义实体能将其他实体视为目标的范围。

- `inside_range`：（数字）可选。目标实体能被列入「内部」范围的最远距离，单位为方块。默认为 `1.0`。
- `outside_range`：（数字）可选。目标实体能被列入「外部」范围的最远距离，单位为方块。默认为 `5.0`。
- `must_see`：（布尔）是否要必须能看到该目标实体。
- `on_inside_range`：（对象，格式同触发器）当目标实体被列入「内部」范围时触发。
- `on_outside_range`：（对象，格式同触发器）当目标实体被列入「外部」范围范围时触发。
- `on_vision_lost_inside_range`：（对象，格式同触发器）当目标实体仍在「内部」范围内，但是该实体看不到目标时触发。

示例（苦力怕 `entities/creeper.json`）：
```json
{
    "minecraft:target_nearby_sensor": {
        "inside_range": 2.5,
        "outside_range": 6.0,
        "must_see": true,
        "on_inside_range": {
            "event": "minecraft:start_exploding",
            "target": "self"
        },
        "on_outside_range": {
            "event": "minecraft:stop_exploding",
            "target": "self"
        },
        "on_vision_lost_inside_range": {
            "event": "minecraft:stop_exploding",
            "target": "self"
        }
    }
}
```
## minecraft:teleport

定义一个实体的传送行为。

- `min_random_teleport_time`：（数字）可选。定义实体两次随机传送间隔的最小秒数。默认为 `0.0`。
- `max_random_teleport_time`：（数字）可选。定义实体两次随机传送间隔的最大秒数。默认为 `20.0`。
- `random_teleportCube`：（数组，`[x, y, z]`）可选。实体会在该大小的区域内随机传送。默认 `[32.0, 16.0, 32.0]`。
- `random_teleports`：（布尔）可选。如果为 `true`，该实体会随机传送。默认为 `true`。
- `target_distance`：（数字）可选。定义实体在追踪目标时最远传送距离。默认为 `16.0`。
- `dark_teleport_chance`：（数字）可选。定义实体在黑暗中传送的可能性。取值应在 `[0.0, 1.0]` 之中。默认为 `0.01`。
- `light_teleport_chance`：（数字）可选。定义实体在光亮中传送的可能性。取值应在 `[0.0, 1.0]` 之中。默认为 `0.01`。
- `target_teleport_chance`：（数字）可选。定义实体在追踪目标时传送的可能性。取值应在 `[0.0, 1.0]` 之中。默认为 `1.0`。

示例（末影人 `entities/enderman.json`）：
```json
{
    "minecraft:teleport": {
        "random_teleports": true,
        "max_random_teleport_time": 30,
        "random_teleport_cube": [ 32, 32, 32 ],
        "target_distance": 16,
        "target_teleport_chance": 0.05,
        "light_teleport_chance": 0.05
    }
}
```
## minecraft:tick_world

定义该实体是否能加载世界。

- `never_despawn`：（布尔）可选。如果为 `true`，该实体即使距离玩家很远也不会被移除。默认为 `false`。
- `distance_to_players`：（数字）可选。该实体被移除时距离玩家的最短距离。如果 `never_despawn` 设置为 `true` 该选项将被忽略。默认为 `128.0`。最小值 `128.0`。
- `radius`：（数字）可选。该实体加载的区块半径。默认为 `2`，可用范围 `2-6`。

## minecraft:timer

定义一个一段时间后自动触发触发器的组件。

- `looping`：（布尔）可选。如果为 `true`，该实体会在每次触发以后重新计时。默认为 `true`。
- `time`：（数字或数组 `[a, b]`）可选。触发指定触发器的秒数。默认为 `0.0`。
- `randomInterval`：（布尔）可选。如果为 `true`，该计时器每次的时间会在 `time` 指定的范围内随机生成。默认为 `true`。
- `time_down_event`：（对象，格式同触发器）当时间到达时触发的触发器。

示例（蜜蜂 `entities/bee.json`）：
```json
{
    "minecraft:timer": {
        "looping": false,
        "time": [10, 60],
        "randomInterval": true,
        "time_down_event": {
            "event": "perish_event",
            "target": "self"
        }
    }
}
```

## minecraft:trade_table

定义该实体与玩家交易的能力。似乎只被旧版村民使用。另见新版村民使用的 `minecraft:economy_trade_table` 组件。

- `display_name`：（字符串）与此实体交易时显示的名称。
- `table`：（字符串）从行为包根目录起始的交易表的相对路径，包含后缀 `.json`。使用左斜杠 `/` 分割路径。
- `convert_trades_economy`：（布尔）不明。

示例（旧版村民 `entities/villager.json`）：
```json
{
    "minecraft:trade_table": {
        "display_name": "entity.villager.farmer",
        "table": "trading/farmer_trades.json",
        "convert_trades_economy": true
    }
}
```

## minecraft:transformation

定义实体从当前实体转变为另一个实体的行为。

- `add`：（对象）转变后该实体启用的组件组。
    - `component_groups`：（数组）要启用的组件组。
        - *（字符串）一个组件组的名字*
- `remove`：（对象）转变后该实体禁用的组件组。
    - `component_groups`：（数组）要禁用的组件组。
        - *（字符串）一个组件组的名字*
- `begin_transform_sound`：（字符串）开始转变时的音效。
- `delay`：（对象）定义该实体转变的延迟。
    - `block_assist_chance`：（数字）可选。实体寻找附近的能够提升转变速度的方块的几率。取值应在 `[0.0, 1.0]` 之中。默认为 `0.0`。
    - `block_chance`：（数字）可选。实体找到此方块后，该方块能够提升转变速度的几率。取值应在 `[0.0, 1.0]` 之中。默认为 `0.0`。
    - `block_radius`：（数字）可选。实体会寻找辅助转变的方块的半径。默认为 `0`。
    - `block_max`：（数字）可选。实体会寻找的最大的辅助转变的方块数量。如果设置为 0 或不设置，将采用 `block_radius` 的值。默认为 `0`。
    - `trust_items`：（数组）能辅助转变的方块列表。
        - *（字符串）一个方块*
    - `keep_owner`：（布尔）如果为 `true`，该实体转变后会保留原有的主人。
    - `value`：（数字）可选。实体完成转变所消耗的秒数。默认为 `0.0`。
- `drop_equipment`：（布尔）如果为 `true`，该实体转变后会掉落所有装备。
- `into`：（字符串）该实体会转变成的实体的 ID。可以在 ID 后用 `<>` 包裹要触发的事件。
- `transformation_sound`：（字符串）完成转变时播放的音效。

示例（尸壳 `entities/husk.json`）：
```json
{
    "minecraft:transformation": {
        "into": "minecraft:zombie<minecraft:as_adult>",
        "transformation_sound": "convert_to_drowned",
        "drop_equipment": true,
        "delay": {
            "value": 15
        }
    }
}
```

## minecraft:trusting

定义该实体信任玩家的条件。

- `probability`：（数字）可选。每使用一个物品使该实体信任玩家的可能性。取值应在 `[0.0, 1.0]` 之中。默认为 `1.0`。
- `trust_items`：（数组）能使该实体信任玩家的物品列表。
    - *（字符串）一个物品 ID*
- `trust_event`：（对象，格式同触发器）当该实体信任玩家时触发。

示例（豹猫 `entities/ocelot.json`）：
```json
{
    "minecraft:trusting": {
        "probability": 0.33,
        "trust_items": [ "fish", "salmon" ],
        "trust_event": {
            "event": "minecraft:on_trust",
            "target": "self"
        }
    }
}
```
