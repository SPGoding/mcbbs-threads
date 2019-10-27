# 属性

定义实体的固有属性。

## minecraft:attack

定义实体的近战攻击及其附加效果。

- `damage`：（数组）近战伤害的范围。格式为 `[最低伤害, 最高伤害]`。
- `effect_name`：（字符串）可选。一个状态效果的 ID。如果填写，将会对被打的实体施加指定的状态效果。
- `effect_duration`：（数字）可选。对被打的实体施加的状态效果的持续时间，以秒为单位。

示例（蜜蜂 `entities/bee.json`）：
```json
{
    "minecraft:attack": {
        "damage": 2,
        "effect_name": "poison",
        "effect_duration": 10
    }
}
```

## minecraft:ambient_sound_interval

设置该实体的音效。

- `event_name`：（字符串）要播放的音效的名称。这和在资源包里定义的音效名一当一致。资源包的具体内容不在本教程范畴内。
- `range`：（数字）可选。实体再次播放该音效的时间间隔最大值，单位为秒。默认为 `16.0`。
- `value`：（数字）可选。实体再次播放该音效的时间间隔最小值，单位为秒。默认为 `8.0`。

示例（蜜蜂 `entities/bee.json`）：
```json
{
    "minecraft:ambient_sound_interval" :{
        "event_name" : "ambient.pollinate",
        "range" : 3.0,
        "value" : 2.0
    }
}
```

## minecraft:burns_in_daylight

使该实体能够在日光下燃烧。

示例（溺尸 `entities/dronwed.json`）：
```json
{
    "minecraft:burns_in_daylight": {}
}
```

## minecraft:can_climb

允许实体爬楼梯（存疑）。

示例（烈焰人 `entities/blaze.json`）：
```json
{
    "minecraft:can_climb": {}
}
```

## minecraft:can_fly

允许实体飞行。寻路系统将不再限制实体脚下的方块必须为固体方块。

示例（烈焰人 `entities/blaze.json`）：
```json
{
    "minecraft:can_fly": {}
}
```

## minecraft:can_power_jump

允许实体蓄力跳。（类似原版中马、驴等的表现。）

示例（驴 `entities/donkey.json`）：
```json
{
    "minecraft:can_power_jump": {}
}
```

## minecraft:collision_box

设置该实体碰撞箱的宽和高。

- `height`：（数字）可选。碰撞箱的高度，单位为格。默认为 `1.0`。
- `width`：（数字）可选。碰撞箱的宽度，单位为格。默认为 `1.0`。

示例（盔甲架 `entities/arcmor_stand.json`）：
```json
{
    "minecraft:collision_box": {
        "width": 0.5,
        "height": 1.975
    }
}
```
## minecraft:color

定义该实体的颜色。只对原版的有颜色预制的生物有效（例如羊、潜影贝）。

- `value`：（数字）可选。颜色值。默认为 `0`。

示例（猫 `entities/cat.json`）：
```json
{
    "minecraft:color": {
        "value": 14
    }
}
```
## minecraft:color2

定义实体的第二个颜色。只对原版的有第二个颜色预制的生物有效（热带鱼）。

- `value`：（数字）可选。颜色值。默认为 `0`。

示例（热带鱼 `entities/tropical_fish.json`）：
```json
{
    "minecraft:color2": {
        "value": 1
    }
}
```
## minecraft:default_look_angle

设置该实体默认状态下头的角度。

- `value`：（数字）可选。角度，单位为度。默认为 `0.0`。

## minecraft:equipment

使用指定的战利品表来设置该实体的装备。

- `table`：（字符串）战利品表的相对路径。从行为包的根目录起始，包含 `.json` 后缀，使用左斜杠 `/` 分割路径。这个战利品表中返回的第一项（`entry`）会被认为是手中的物品，你可以利用在 `entry` 里嵌套 `pools` 来设置主手和副手。这个战利品表中返回的第二项会被认为是装备栏的物品，你可以利用在 `entry` 里嵌套 `pools` 来设置头盔、胸甲、护腿、靴子。由于这里的路径是从根目录开始写的，所以你完全可以不把战利品表放到 `loot_tables` 文件夹里，并且把自己的诡异的路径填到这里面。我个人不推荐这么做就是了，因为会造成他人理解你的行为包的困难。共享技术才能形成社区，独占技术是无法帮助人类进步的，这都是题外话，不说了。
- `slot_drop_chance`：（数组）可选。储存可能掉落物品的装备栏位，以及这个栏位上掉落的几率。
    - *（对象）一个栏位以及它的掉落几率*
        - `slot`：（字符串）栏位名。可选值和 `replaceitem` 命令里的一样。即：
            - `slot.weapon.offhand` 代表副手。
            - `slot.weapon.mainhand` 代表主手。
            - `slot.armor.head` 代表头部。
            - `slot.armor.chest` 代表胸部。
            - `slot.armor.legs` 代表腿部。
            - `slot.armor.feet` 代表脚部。
        - `drop_chance`：（数组）掉落几率。取值应在 `[0.0, 1.0]` 当中。

示例（溺尸 `entities/drowned.json`）：
```json
{
    "minecraft:equipment": {
        "table": "loot_tables/entities/drowned_ranged_equipment.json",
        "slot_drop_chance": [
            {
                "slot": "slot.weapon.offhand",
                "drop_chance": 1.0
            }
        ]
    }
}
```

## minecraft:fire_immune （布尔）

使实体不会受到火焰伤害。

示例（烈焰人 `entities/blaze.json`）：
```json
{
    "minecraft:fire_immune": true
}
```

## minecraft:floats_in_liquid

使实体能够在液体中悬浮。

## minecraft:flying_speed

设置实体的飞行速度。

- `value`：（数字）可选。每刻该实体最多能飞过的方块的格数。默认为 `0.02`。

示例（蜜蜂 `entities/bee.json`）：
```json
{
    "minecraft:flying_speed": {
        "value": 0.15
    }
}
```

## minecraft:foot_size

设置实体在非跳跃时的步行速度。

- `value`：（数字）可选。实体迈一步的大小。默认为 `0.5`。

## minecraft:friction_modifier

定义「摩擦力」对该实体的影响程度。

- `value`：（数字）可选。数字越大，摩擦力影响越大。`1.0` 表示正常的摩擦，`2.0` 表示正常摩擦的二倍，`0.5` 表示一半，诸如此类。默认为 `1.0`。

## minecraft:ground_offset

设置实体从地面的偏移。

- `value`：（数字）可选。实体从地面向上偏移的距离，单位为格。默认为 `0.0`。

## minecraft:input_ground_controlled

如果该实体可以被骑乘，添加该组件可以使玩家能够使用方向键（WASD）控制实体移动。

示例（驴 `entities/donkey.json`）：
```json
{
    "minecraft:input_ground_controlled": {}
}
```

## minecraft:is_baby

设置该实体为幼儿状态。

示例（蜜蜂 `entities/bee.json`）：
```json
{
    "minecraft:is_baby": {}
}
```

## minecraft:is_charged

设置该实体带电（存疑。不止闪电苦力怕使用了这个组件，带着花蜜的蜜蜂也用了）。

示例（蜜蜂 `entities/bee.json`）：
```json
{
    "minecraft:is_charged": {}
}
```

## minecraft:is_chested

设置该实体携带了一个箱子。

示例（骡子 `entities/mule.json`）：
```json
{
    "minecraft:is_chested": {}
}
```

## minecraft:is_dyeable

允许染料改变该实体的颜色。似乎只对狼、猫、羊有用。

- `interact_text`：（字符串）触屏模式下用染料与该实体交互时显示的文本。

示例（猫 `entities/cat.json`）
```json
{
    "minecraft:is_dyeable": {
        "interact_text": "action.interact.dye"
    }
}
```

## minecraft:is_hidden_when_invisible

该生物有隐身状态效果时是否不会被敌对生物攻击。

示例（玩家 `entities/player.json`）：
```json
{
    "minecraft:is_hidden_when_invisible": {}
}
```

## minecraft:is_ignited

该实体是否已被点燃。

示例（TNT矿车 `entities/tnt_minecart.json`）：
```json
{
    "minecraft:is_ignited": {}
}
```

## minecraft:is_illager_captain

该实体是否是灾厄村民首领。

示例（掠夺者 `entities/pillager.json`）：
```json
{
    "minecraft:is_illager_captain": {}
}
```

## minecraft:is_saddled

该实体是否正装备着马鞍。

示例（ `entities/.json`）：
```json
{
    "minecraft:is_saddled": {}
}
```

## minecraft:is_shaking

该实体是否正在抖动。

示例（马 `entities/horse.json`）：
```json
{
    "minecraft:is_shaking": {}
}
```

## minecraft:is_sheared

该实体是否被剪刀剪了。似乎只对羊和雪傀儡有用。

示例（雪傀儡 `entities/snow_golem.json`）：
```json
{
    "minecraft:is_sheared": {}
}
```

## minecraft:is_stackable

该实体是否可以堆叠起来。

示例（船 `entities/boat.json`）：
```json
{
    "minecraft:is_stackable": {}
}
```

## minecraft:is_stunned

似乎只对劫掠兽有效。

示例（劫掠兽 `entities/ravager.json`）：
```json
{
    "minecraft:is_stunned": {}
}
```

## minecraft:is_tamed

该实体是否已被驯服。

示例（猫 `entities/cat.json`）：
```json
{
    "minecraft:is_tamed": {}
}
```

## minecraft:item_controllable

设置玩家在骑乘时可以控制该实体的物品。

- `control_items`：（数组）所有可以控制该实体的物品。
    - *（字符串）一个物品 ID*

示例（猪 `entities/pig.json`）：
```json
{
    "minecraft:item_controllable": {
        "control_items": "carrotOnAStick"
    }
}
```

## minecraft:loot

设置该实体死亡时使用的战利品表。

- `table`：（字符串）战利品表的相对路径。从行为包的根目录起始，包含 `.json` 后缀，使用左斜杠 `/` 分割路径。当实体死亡后该战利品表返回的物品将会掉落出来。

示例（盔甲架 `entities/armor_stand.json`）：
```json
{
    "minecraft:loot": {
        "table": "loot_tables/entities/armor_stand.json"
    }
}
```

## minecraft:mark_variant

标记该实体附加的变种值。可用于进一步区分不同的变种。另见 `minecraft:variant` 组件。

- `value`：（数字）可选。变种值。按照惯例，0 是基础实体的 ID。默认为 `0`。

示例（马 `entities/horse.json`）：
```json
{
    "minecraft:mark_variant": {
        "value": 1
    }
}
```
## minecraft:push_through

设置该实体可以被推动通过的距离。

- `value`：（数字）可选。单位为方块。默认为 `0.0`。

## minecraft:scale

设置该实体的缩放倍数。通常被用于使生物的幼体比成年状态下小。

- `value`：（数字）可选。该实体被放大的倍数。`1.0` 表示与模型里面定义的大小一致，`2.0` 表示二倍，`0.5` 表示一半，诸如此类。该选项会改变碰撞箱的大小。默认为 `1.0`。

示例（蜜蜂 `entities/bee.json`）：
```json
{
    "minecraft:scale": {
        "value": 0.5
    }
}
```

## minecraft:skin_id

设置该实体皮肤的 ID。似乎只对新版村民和新版僵尸村民有用。

示例（新版村民 `entities/villager_v2.json`）：
```json
{
    "minecraft:skin_id": {
        "value": 0
    }
}
```

## minecraft:sound_volume

设置该实体音效的基础音量。

- `value`：（数字）可选。音量的大小。默认为 `1.0`。

## minecraft:spell_effects

当该组件被添加时，会对该实体添加/移除指定的状态效果。

- `add_effects`：（数组）要添加的状态效果。
    - *（对象）一个状态效果*
        - `ambient`：（布尔值）是否为信标给予的效果。
        - `amplifier`：（数字）等级。`0` 代表 Ⅰ 级，`1` 代表 Ⅱ 级，以此类推。
        - `duration`：（数字）持续时间，单位为秒。
        - `visible`：（布尔值）是否显示粒子效果。
        - `display_on_screen_animation`：（布尔值）是否显示屏幕动画（即该药水的图标在屏幕中间晃悠一下的动画，具体的可以杀一只掠夺者首领看看效果）。只有在该状态效果给予的是玩家时该设置才有用。
- `remove_effects`：（字符串）要移除的状态效果的名字。

示例（玩家 `entities/player.json`）：
```json
{
    "minecraft:spell_effects": {
        "add_effects": [
            {
                "effect": "bad_omen",
                "duration": 6000,
                "display_on_screen_animation": true
            }
        ]
    }
}
```

## minecraft:strength

定义该实体携带物品的「力量」。

- `max`：（数字）可选。实体的最大力量。默认为 `5`。
- `value`：（数字）可选。实体的初始力量。默认为 `1`。

示例（羊驼 `entities/llama.json`）：
```json
{
    "minecraft:strength": {
        "value": 1,
        "max": 5
    }
}
```

## minecraft:type_family

设置该实体所属于的分类。

- `family`：（数组）分类。
    - *（字符串）一个分类的名称*

示例（盔甲架 `entities/armor_stand.json`）：
```json
{
    "minecraft:type_family": {
        "family": [ "armor_stand", "inanimate", "mob" ]
    }
}
```

## minecraft:variant

标记当前实体的变种值。另见 `minecraft:mark_variant` 组件。

- `value`：（数字）可选。变种的 ID。按照惯例，0 是基础实体的 ID。默认为 `0`。

示例（猫 `entities/cat.json`）：
```json
// 这个示例的结构是从根对象开始的，为了更好地表现 `minecraft:variant` 的用途。
{
    "component_groups": {
        "minecraft:cat_white": {
            "minecraft:variant": {
                "value": 0
            }
        },
        "minecraft:cat_tuxedo": {
            "minecraft:variant": {
                "value": 1
            }
        },
        // ...
    }
}
```

## minecraft:walk_animation_speed

设置该实体行走动画的播放速度。

- `value`：（数字）可选。数字越大，动画播放越快。`1.0` 表示正常的播放速度，`2.0` 表示正常速度的二倍，`0.5` 表示一半，诸如此类。默认为 `1.0`。

## minecraft:wants_jockey

设置该实体想要成为骑在其他生物身上（存疑，原版没有实体使用该组件）。
