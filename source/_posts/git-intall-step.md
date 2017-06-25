---
title: Git 安装教程
date: 2016-10-29 16:40:36
tags: Git
categories: Git
---

`Git` 用了很久了，家里电脑每次重装系统后，`Git` 都必须重新装，由于本人英语能力有限，每次安装选项不知如何选择，只能每次都要 `google` 一番，为防止此弱智行为，特此记录。

<!-- more -->
### step1
![Git-step-1](http://ofstpx613.bkt.clouddn.com/3BB0.tmp.png)
第一步很简单，这里我只安装了 `Git Bash` ， `Git GUI` 页面简陋，用 `SourceTree` 代替。

### step2
![Git-step-2](http://ofstpx613.bkt.clouddn.com/525D.tmp.png)
第二步，设置环境变量，选择使用的命令行工具，这里我选择默认的 `Git` 自带的 `Git Bash`。

### step3
![Git-step-3](http://ofstpx613.bkt.clouddn.com/6B4C.tmp.png)
第三步，是设置处理 `line endings `的方式。
在文本处理中，`CR/LF` 是不同操作系统上使用的换行符：
- `Dos` 和`windows` 采用回车 + 换行 `CR/LF` 表示下一行
- `Unix` / `Linux` 采用换行符`LF`表示下一行
- `OS` 则采用`CR`表示下一行

#### `CR` 与 `LF` 区别：

- `CR` 用符号 `\r` 表示，十进制 `ASCII` 代码是13，十六进制代码为 `0x0D`
- `LF` 使用 `\n` 符号表示，ASCII代码是10，十六制为`0x0A`

这里我选择第一项，将提交时候 `windows` 的 `CRLF` 会自动转化转化为Unix的 `LF`，
也可以设置不转换，配置如下：
```
$ git config --global core.autocrlf true
```

### step4
![Git-step-5](http://ofstpx613.bkt.clouddn.com/7F98.tmp.png)
第四步，是配置终端模拟器，我这里选择默认的选项，`windows` 的 `console` 窗口太丑。

### step5
![Git-step-3](http://ofstpx613.bkt.clouddn.com/42BA.tmp.png)
第五步，配置额外的选项，选择默认配置。
