# 起

JSON 文本中有一个属性叫做 `interpret`，翻译一下可以知道它的意思是「解析」。该属性必须和 `nbt` 同时使用，否则没有效果。

简单来讲，当 `interpret` 被设置为 `true` 时，实际显示出来的文本会是原先文本被按照 JSON 文本的格式解析后的文本。

举个例子吧。我们往 data storage 里先写这么一个标签：
```mcfunction
/data merge storage test {spg: '["测试", "啊啊"]'}
```

然后写这样的一个 JSON 文本：
```json
{"nbt": "spg", "storage": "test"}
```

把它放到 `tellraw` 或者什么地方里，稍有常识的人都可以预测出它的结果：
```json
["测试", "啊啊"]
```

那么，如果在 JSON 文本中加上 `interpret`：
```json
{"nbt": "spg", "storage": "test", "interpret": true}
```

结果变为了：
```
测试啊啊
```

可以看出，原先的 `["测试", "啊啊"]` 被按照 JSON 文本的格式解析为了 `测试啊啊`。

# 承

这个特性确实有用武之地。例如：
```mcfunction
/execute as @a run tellraw @s {"nbt": "SelectedItem.tag.display.Name", "entity": "@s", "interpret": true}
```

这条命令就可以在聊天栏输出玩家手中握着的武器的自定义名称了（如果有的话）

# 转

没错，没什么用。

# 合

然而，并不只有 NBT 字符串里能储存像 JSON 文本一样的东西。

我们往 data storage 里再写这么一个标签：
```mcfunction
/data merge storage test {spg: ["测试", "啊啊"]}
```

如果你没发现的话，这条命令的 `spg` 和刚才唯一的区别是删掉了最外层的单引号，使它从一个字符串标签变为了一个装有字符串的列表。

然后写这样的一个 JSON 文本：
```json
{"nbt": "spg", "storage": "test"}
```

它的结果和刚刚一模一样：
```json
["测试", "啊啊"]
```

聪明的人已经能够想到，如果加上 `interpret` 的话：
```json
{"nbt": "spg", "storage": "test", "interpret": true}
```

结果仍然是：
```
测试啊啊
```

哇！

这成功实现了将多个字符串拼接到一起。事实上，不止字符串能够被拼接，NBT 中的 Int 标签也能够被视作为合法的 JSON 格式。因此，我们还可以将由 Int 标签组成的列表标签拼接为一个字符串，这在原版模组[大数运算](https://www.mcbbs.net/thread-904284-1-1.html)中有所体现。

相信你已经发现了，这一技巧仍然没有什么用处。

TAG_End
