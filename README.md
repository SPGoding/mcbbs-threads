# mcbbs-threads

@SPGoding 在 MCBBS 发表的有价值帖子的 BBCode 代码

## 协议

![知识共享许可协议](https://i.creativecommons.org/l/by-nc-sa/4.0/88x31.png)

本作品采用[知识共享署名-非商业性使用-相同方式共享 4.0 国际许可协议](http://creativecommons.org/licenses/by-nc-sa/4.0/)进行许可。

## 快速替换 regex

```
`(.*?)`
[ic]$1[/ic]

\[ic\](.*?)\[\/ic\]
[backcolor=white][font=Monaco,Consolas,'Lucida Console','Courier New',serif]$1[/font][/backcolor]
```

致 MCBBS：请尽快支持原生的行内代码样式。