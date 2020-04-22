![](https://i.loli.net/2020/04/23/KQDdTakOx4FWfGC.gif)

通过活用 JSON 文本的 `translate` 与 `with` 属性，我们可以做到在启用与禁用自制资源包时显示不同的文字。

# 原理 —— `translate`

我们了解一下 `translate` 的原理。首先，游戏会寻找语言文件（位于 `assets/minecraft/lang` 文件夹下）中是否有 `translate` 中所指定的键，如果有的话就显示该键对应的值，否则就直接把这个键显示出来。

## 例子一
```
/tellraw @a {"translate": "block.minecraft.grass_block"}
```

结果：`草方块`

解释：在简体中文语言文件中有这么一行：

```
"block.minecraft.grass_block": "草方块"
```

因此，我们在 `translate` 中写的 `block.minecraft.grass_block` 能够被游戏正确翻译为 `草方块`。 

## 例子二
```
/tellraw @a {"translate": "block.minecraft.啊啊啊啊啊"}
```

结果：`block.minecraft.啊啊啊啊啊`

解释：在默认的语言文件中并没有 `block.minecraft.啊啊啊啊啊` 这个键，所以游戏原样把它显示了出来。

# 小试牛刀

掌握了 `translate` 的知识，我们现在可以做出最简单的、让自制的资源包启用与禁用时显示不同文字的效果了。

目标：
- 没有加载资源包时，显示 `请安装资源包`
- 已经加载资源包时，显示 `你已成功安装了资源包`

利用 `translate` 的绝佳特性，我们可以轻松地写出：

```
/tellraw @a {"translate": "请安装资源包"}
```

然后，在自制资源包的语言文件（`assets/minecraft/lang/zh_cn.json`）中写入：

```
{
    "请安装资源包": "你已成功安装了资源包"
}
```

搞定。

# 要 来 了

现在，我们**稍微**地改进**一小点儿**需求：
- 没有加载资源包时，显示 `请安装资源包`，颜色为浅蓝色，添加下划线，点击该文字时打开网址 `https://www.mcbbs.net`
- 已经加载资源包时，显示 `你已成功安装了资源包`，颜色为金色，加粗，鼠标悬浮在上面时显示文字 `作者：SPGoding`

然后我再告诉大家一个事实：截止 20w17a，语言文件中不能使用 JSON 文本。

好，相信这个问题是无解的了，本帖终结，MJSB。

# 原理 —— `with`

我们了解一下 `with` 的原理。在前面讲述 `translate` 时，我们说过

> 首先，游戏会寻找语言文件（位于 `assets/minecraft/lang` 文件夹下）中是否有 `translate` 中所指定的键，如果有的话就显示该键对应的值，否则就直接把这个键显示出来。

这段话里的「显示」其实是在骗你的。在真正显示之前，游戏还会先把那段字符串里面的**占位符**（placeholder）根据 `with`（一个数组，里面可以放 JSON 文本）中的内容进行替换。

## 例子三：一般通过占位符

`%s` 是一个很普通的占位符。

```
/tellraw @a {"translate": "这里有一个奇怪的占位符【%s】", "with": ["这里是往里面填充的内容"]}
```

结果：`这里有一个奇怪的占位符【这里是往里面填充的内容】`

## 例子四：二般通过占位符

多个 `%s` 会按顺序被 `with` 中的内容替换。

```
/tellraw @a {"translate": "这里有两个奇怪的占位符【%s】【%s】", "with": ["一", "二"]}
```

结果：`这里有两个奇怪的占位符【一】【二】`

## 例子五：带索引的占位符

你可以用类似 `%数字$s` 的结构来引用 `with` 中的第 `数字` 个内容。

```
/tellraw @a {"translate": "这几个占位符是带索引的【%1$s】【%2$s】【%2$s】【%1$s】", "with": ["一", "二"]}
```

结果：`这几个占位符是带索引的【一】【二】【二】【一】`

## 例子六：往 `with` 里面插奇怪的东西

`with` 块里面可以写复杂的 JSON 文本。

```
/tellraw @a {"translate": "%1$s", "with": [{"text": "复杂的", "color": "#66ccff", "bold": true}]}
```

结果：![复杂的.png](https://i.loli.net/2020/04/23/2QP1khy3eGibfEm.png)

## 例子七：不存在的索引

不存在的索引会被变成空字符串。

```
/tellraw @a {"translate": "【%1$s】【%114514$s】", "with": ["一", "二"]}
```

结果：`【一】【】`

**注意**：索引最大为 `2147483647`，再往上的不会被认为是索引，并会被原样显示出来。

# 大闹天宫

我们刚刚改进过后的需求是这样的：
> - 没有加载资源包时，显示 `请安装资源包`，颜色为浅蓝色，添加下划线，点击该文字时打开网址 `https://www.mcbbs.net`
> - 已经加载资源包时，显示 `你已成功安装了资源包`，颜色为金色，加粗，鼠标悬浮在上面时显示文字 `作者：SPGoding`

为了让后续命令看起来比较简单，我们先把这两个不同的文字用 JSON 文本表示出来，并在后文用 `"A"` 和 `"B"` 暂时替代：

- `A`：`{"text": "请安装资源包", "color": "aqua", "underlined": true, "clickEvent": {"action": "open_url", "value": "https://www.mcbbs.net"}}`
- `B`: `{"text": "你已成功安装了资源包", "color": "gold", "bold": true, "hoverEvent": {"action": "show_text", "contents": "作者：SPGoding"}}`

我们已知的特性是这样的：
- `translate` 可以用于让加载/不加载资源包时显示不同的内容。这些内容里的占位符会被 `with` 里的内容替代。
- `with` 中可以使用复杂的 JSON 文本格式。

可以简单地得到这样一条命令：

```
/tellraw @a {"translate": "%1$s", "with": ["A", "B"]}
```

然后，在自制资源包的语言文件（`assets/minecraft/lang/zh_cn.json`）中写入：

```
{
    "%1$s": "%2$s"
}
```

现在我们已经成功逆天改命了。

当没有安装资源包时，`translate` 的内容 `%1$s` 保持原样不动，因此调用 `with` 中的第一串文字 `"A"`；
当已经安装资源包时，`translate` 的内容 `%1$s` 翻译为 `$2$s`，因此调用 `with` 中的第二串文字 `"B"`。

# 多资源包兼容性

等一下，你自个儿玩儿得倒挺爽，别的资源包作者怎么活啊！

直接翻译 `%1$s` 确实过于简单粗暴，不过我们可以利用「例子七：不存在的索引」中提到的特性，在 `%1$s` 后（或前）加入一个（或多个）不存在的索引：

```
/tellraw @a {"translate": "%1$s%114514$s", "with": ["A", "B"]}
```

然后，在语言文件（`assets/minecraft/lang/zh_cn.json`）中把这整个怪物翻译为 `%2$s`：

```
{
    "%1$s%114514$s": "%2$s"
}
```

把 `A` 和 `B` 替换为真正的内容，我们的最终命令是这样的：

```
/tellraw @a {"translate": "%1$s%114514$s", "with": [{"text": "请安装资源包", "color": "aqua", "underlined": true, "clickEvent": {"action": "open_url", "value": "https://www.mcbbs.net"}}, {"text": "你已成功安装了资源包", "color": "gold", "bold": true, "hoverEvent": {"action": "show_text", "contents": "作者：SPGoding"}}]}
```

我们可以发现，启用资源包前后的文本都可以直接在这条命令中进行修改，并且不需要修改别的地方，十分方便。

# 作者 / Credits

相关思路非本人原创，系从 Minecraft Commands Discord Server 学习得到，最初作者暂不可考。

The idea wasn't created by me. I learnt it from the Minecraft Commands Discord Server. No idea who came up with it first.

本文发表于共有领域。
