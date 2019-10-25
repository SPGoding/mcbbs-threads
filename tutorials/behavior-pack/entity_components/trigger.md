# 触发器

首先讲解触发器，是因为在后面有些组件的参数中，使用到了与触发器完全相同的 JSON 格式。

触发器，是一种能够根据指定滤器来执行事件的东西（有关滤器、事件的详细内容，可以通过上方目录切换到相关附录）。它的工作流程如下所示：

![触发器.png](https://i.loli.net/2019/10/25/GVYvFQqrd2ih3I4.png)

## 基础格式

- `event`：（字符串）该触发器被触发后要执行的事件。
- `filters`：（对象）可选。一个滤器。如果指定了，则只有在该滤器通过的情况下才会执行 `event` 中指定的事件。
- `target`：（字符串）可选。执行该事件的目标。默认为 `self`，代表该实体自身。

再次强调，触发器属于组件的一种，所以要定义在 `components` 或是 `component_groups` 中的一个组件组之下。

## minecraft:on_death

触发时机：该实体死亡时。**只能被末影龙使用**。

```
{
    "event": "minecraft:start_death",
    "target": "self"
}
```

## minecraft:on_friendly_anger

触发时机：该实体附近有其他相同种类的实体愤怒时。

## minecraft:on_hurt

触发时机：该实体受到伤害时。

```
{
    "event": "minecraft:crystal_explode",
    "target": "self"
}
```

## minecraft:on_hurt_by_player

触发时机：该实体被玩家攻击时。

## minecraft:on_ignite

触发时机：该实体着火时。

## minecraft:on_start_landing

触发时机：该实体着陆时。**只能被末影龙使用**。

```
{
    "event": "minecraft:start_land",
    "target": "self"
}
```

## minecraft:on_start_takeoff

触发时机：该实体开始飞行时。**只能被末影龙使用**。

```
{
    "event": "minecraft:start_fly",
    "target": "self"
}
```

## minecraft:on_target_acquired

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

## minecraft:on_target_escape

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

## minecraft:on_wake_with_owner

触发时机：该实体的主人在与该实体一起睡觉后醒来时。
