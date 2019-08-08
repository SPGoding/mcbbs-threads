# [1.14.4] 追根溯「源」—— NBT 的相等与匹配

[![CC BY-NC-SA 4.0](https://i.creativecommons.org/l/by-nc-sa/4.0/88x31.png)](http://www.mcbbs.net/plugin.php?id=link_redirect&target=https%3A%2F%2Fcreativecommons.org%2Flicenses%2Fby-nc-sa%2F4.0%2Fdeed.zh)

[GitHub 原帖](https://github.com/SPGoding/mcbbs-threads/blob/master/tutorials/seek-the-source/nbt_match/nbt_match.md)

[MCBBS 原帖](https://www.mcbbs.net/thread-??????-1-1.html)

# 追根溯「源」

**追根溯「源」**，是从源代码角度对部分命令机制的漫谈系列。也许在骂着 Mojang SB 的间隙，偶尔看看他们是怎么犯下的蠢，能帮你更好地驾驭命令？谁知道呢。

本系列漫谈基于对 Minecraft Java Edition 1.14.4 的反编译与反混淆，十分感谢 [FabricMC](https://github.com/FabricMC) 提供的散发着自由气息的 [yarn](https://github.com/FabricMC/yarn) 项目，梓榆谨代表自己向所有该项目的贡献者致以最崇高的敬意。三鞠躬。由于散步商业闭源软件的源代码属于违法行为，本帖中只给出极少数代码片段，仅供学习交流使用。当然，因为名字都是人起的，为了方便，本人对部分映射名进行了修改。

阅读本系列可能需要具备一定的英语水平，或是具备查阅字典的能力，并不需要有多么高深的编程水平。毕竟，笔者压根就不会编程。

# NBT 的相等
