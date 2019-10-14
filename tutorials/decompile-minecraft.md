# 用 Enigma 和官方混淆表反混淆游戏

Mojang AB 自 [19w36a](https://www.mcbbs.net/thread-911386-1-1.html) 起开始随游戏一同发布混淆表，玩家可以通过该混淆表反混淆游戏代码。本文将讲述如何通过[Enigma](https://github.com/FabricMC/Enigma) 软件完成这一事项。

## 下载 Enigma

Enigma（E・nig・ma）是一个可用于 Java 程序的反混淆工具，这里我们使用 [FabricMC](https://github.com/FabricMC/Enigma) 团队[维护的版本](https://github.com/FabricMC/Enigma)。

你可以直接下载 [FabricMC 构建好的 Enigma](https://maven.fabricmc.net/cuchaz/enigma/)。把滚动条拉到最下面，点进最新版本的文件夹，下载 `enigma-0.xx.x.xx-all.jar` 即可。

你也可以利用 [Enigma 的源代码](https://github.com/FabricMC/Enigma) 通过 `gradlew build` 自行构建。

得到的 `jar` 文件可以直接右键选择 “Java(TM) Platform SE binary” 打开。

接下来你可以下载游戏文件和官方混淆表了。

![image.png](https://i.loli.net/2019/10/14/86au2kZAi4mUwGD.png)

（主题可以在 “View” -> “Themes” 里切换）

## 下载游戏文件和官方混淆表

### 从 Wiki 下载

打开 [Wiki 的某个版本的页面](https://minecraft.gamepedia.com/Java_Edition_19w36a)，右侧的 “Download” 为游戏的核心 jar 文件下载，其中 “Client” 是客户端，“Server” 是服务端，各取所需。“Download” 之下的 “Obfuscation maps” 则为官方混淆表，选择和你刚刚下载的核心文件对应的混淆表下载即可。

### 从最原始的地方下载

Wiki 是由玩家社区更新的，可能会比较慢，在新快照发布后等一阵子才会有新页面出现。既然我们选择了~~粗制滥造的~~官方混淆表，追求的就是一个速度，如果等不及 Wiki 更新，则可以用下述方法下载。

首先，打开 [version_manifest.json](https://launchermeta.mojang.com/mc/game/version_manifest.json)，你看到了一个巨长无比的 JSON 文件。

> 如果不知道怎么处理，你有两个选择：
> 1. 学习 JSON；
> 2. 等着 Wiki 更新，从 Wiki 下载。

看 `versions` 数组即可，里面的每个对象代表一个版本。`id` 是该版本的版本号，`url` 是该版本的客户端 JSON。找到你想要的版本，然后打开对应 `url` 里的链接。这里以 `19w36a` 为例，打开 https://launchermeta.mojang.com/v1/packages/cf1478395850173064e078bc7195314471634118/19w36a.json。

你看到了另外的一个巨长无比的 JSON 文件，这是该版本的客户端 JSON。按下 Ctrl + F 搜索 `mappings`，你看到了 `client_mappings` 和 `server_mappings`，它们分别代表客户端的混淆表和服务端的混淆表（如果你没有找到，说明这个版本没有官方的混淆表）。找到你想要的那个表，打开它对应的 `url` 内链接即可下载该混淆表（如果你的浏览器没有下载，而是打开了一个新的展现混淆表内容的网页，你可以按 Ctrl + S 把它保存下来）。

再在客户端 JSON 中搜索 `client.jar` 或 `server.jar`，下载你想要的客户端或服务端核心 jar 文件即可。

## 把游戏文件和混淆表导入 Enigma

选择 Enigma 左上角的 “File” -> “Open Jar...”，选择你下载的游戏核心 jar 文件，等待片刻。

选择 “File” -> “Open Mappings...” -> “Proguard”，选择你下载的混淆表文件（`.txt` 格式），等待片刻。

![image.png](https://i.loli.net/2019/10/14/8H2MSguzyUavtef.png)

现在你眼前的就是经过官方混淆表反混淆后的 Minecraft 源代码了。官方混淆表比较小气，函数内的变量、函数的参数都没有给，javadoc 可能官方自己压根就没写，我也就不说它没给了（跑）。

## 注意

官方虽然发布了混淆表，但是一同随着混淆表的协议中是这样声明的：

```
(c) 2019 Microsoft Corporation. All rights reserved. This information is provided “as-is” and you bear the risk of using it. This information does not provide you with any legal rights to any intellectual property in any Microsoft product. You may copy and use this information for your internal, reference purposes. Microsoft makes no warranties, express or implied, with respect to the information provided here.
```

这意味着 Microsoft 并没有改变对游戏代码和资源等的限制，**Minecraft 仍然是商业闭源软件**，任何散步、使用商业闭源软件源码的行为都是违法行为。

不只是 Minecraft 本身，该混淆表也有着极严格的限制，你不能拿它的命名去做一些你不能做的事情。

> 妈耶，命名质量那么差，竟然还这么多限制！
>
> **[FabricMC](https://github.com/FabricMC) 团队和一个优秀的开源社区正在维护 [yarn](https://github.com/FabricMC/yarn) 项目**，能够在新快照发布的**几个小时内**提供该快照的**极高质量**、供任何人**自由使用**的以 [CC0 1.0](https://github.com/FabricMC/yarn/blob/19w41a/LICENSE) 协议分发的混淆表。吹爆！开源万岁！  
