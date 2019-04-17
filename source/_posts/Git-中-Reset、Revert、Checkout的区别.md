---
title: Git 中 Reset、Revert、Checkout的区别
date: 2016-10-31 21:39:40
tags: Git
categories: Git
---

`git reset` 、 `git checkout` 和 `git revert` 是Git中常用命令。经常傻傻分不清他们之间的区别。最近工作不忙，抽出时间参考了其他文档，对其总结了下。

<!-- more -->

## 提交层面的操作

传给 `git reset` 和 `git checkout` 的参数决定了它们的作用域。如果其后参数不是 `filename`，这些操作对所有提交生效。注意，`git revert`没有文件层面的操作。

### Reset

在提交层面上，`reset` 将一个分支的末端指向另一个提交。这可以用来移除当前分支的一些提交。例：

```
git checkout hotfix
git reset HEAD~2
```

![把hotfix分支reset到HEAD~2](http://ofstpx613.bkt.clouddn.com/91DB.tmp.jpg)

hotfix分支末端的两个提交现在变成了悬挂提交。下次Git执行垃圾回收的时候，这两个提交会被删除。

如果你的更改还没有共享给别人，`git reset`是撤销这些更改的简单方法。

除了在当前分支上操作，你还可以通过传入这些标记来修改你的缓存区或工作目录：

- --soft – 缓存区和工作目录都不会被改变
- --mixed – 默认选项。缓存区和你指定的提交同步，但工作目录不受影响
- --hard – 缓存区和工作目录都同步到你指定的提交

![git rese的定义域](http://ofstpx613.bkt.clouddn.com/F85D.tmp.jpg)


### Checkout

常见于切换不同分支。

```
git checkout hotfix
```

这个命令做的是将`HEAD`移到一个新的分支，然后更新工作目录。因为这可能会覆盖本地的修改，Git强制你提交或者缓存工作目录中的所有更改，不然在`checkout`的时候这些更改都会丢失。和`git reset`不一样的是，`git checkout`没有移动这些分支。

![将 HEAD 从 master 移到 hotfix](http://ofstpx613.bkt.clouddn.com/E1F2.tmp.jpg)

除了分支之外，你还可以传入提交的引用来checkout到任意的提交。这和checkout到另一个分支是完全一样的：把HEAD移动到特定的提交。

```
git checkout HEAD~2
```

![将HEAD移动到任意commit](http://ofstpx613.bkt.clouddn.com/A16B.tmp.jpg)

这对于快速查看项目旧版本来说非常有用。但如果你当前的HEAD没有任何分支引用，那么这会造成HEAD分离。这是**非常危险**的，如果你接着添加新的提交，然后切换到别的分支之后就没办法回到之前添加的这些提交。因此，**在为分离的HEAD添加新的提交的时候你应该创建一个新的分支**。

### Revert

Revert撤销一个提交的同时会创建一个新的提交。这是一个安全的方法，因为它不会重写提交历史。比如，下面的命令会找出倒数第二个提交，然后创建一个新的提交来撤销这些更改，然后把这个提交加入项目中。

```
git checkout hotfix
git revert HEAD~2
```


![revert到倒数第二个commit](http://ofstpx613.bkt.clouddn.com/E7B0.tmp.jpg)

相比`git reset`，它不会改变现在的提交历史。因此，`git revert` 可以用在公共分支上，`git reset` 应该用在私有分支上。

你也可以把`git revert` 当作撤销已经提交的更改，而`git reset HEAD` 用来撤销没有提交的更改。

就像`git checkout` 一样，`git revert` 也有可能会重写文件。所以，Git会在你执行 `revert` 之前要求你提交或者缓存你工作目录中的更改。

## 文件层面的操作

`git reset`和`git checkout` 命令也接受文件路径作为参数。这时它的行为就大为不同了。

### Reset

当检测到文件路径时，`git reset` 将缓存区同步到你指定的那个提交。比如，下面这个命令会将倒数第二个提交中的foo.py加入到缓存区中，供下一个提交使用。

```
git reset HEAD~2 foo.py
```

和提交层面的`git reset`一样，通常我们使用HEAD而不是某个特定的提交。运行`git reset HEAD foo.py` 会将当前的foo.py从缓存区中移除出去，而不会影响工作目录中对foo.py的更改。

![将一个文件从commit历史中移动到stage缓存中](http://ofstpx613.bkt.clouddn.com/78F9.tmp.jpg)



--soft、--mixed和--hard对文件层面的`git reset`毫无作用，因为缓存区中的文件一定会变化，而工作目录中的文件一定不变。

### Checkout

Checkout一个文件和带文件路径`git reset` 非常像，除了它更改的是工作目录而不是缓存区。不像提交层面的checkout命令，它不会移动HEAD引用，也就是你不会切换到别的分支上去。

![将文件从提交历史移动到工作目录中](http://ofstpx613.bkt.clouddn.com/3B21.tmp.jpg)

比如，下面这个命令将工作目录中的foo.py同步到了倒数第二个提交中的foo.py。

```
git checkout HEAD~2 foo.py
```

和提交层面相同的是，它可以用来检查项目的旧版本，但作用域被限制到了特定文件。

如果你缓存并且提交了checkout的文件，它具备将某个文件回撤到之前版本的效果。注意它撤销了这个文件后面所有的更改，而`git revert` 命令只撤销某个特定提交的更改。

和`git reset` 一样，这个命令通常和HEAD一起使用。比如`git checkout HEAD foo.py`等同于舍弃foo.py没有缓存的更改。这个行为和`git reset HEAD --hard`很像，但只影响特定文件。

## 总结


|      命令      | 作用域  | 常用情景              |
| :----------: | :--: | :---------------- |
|  git reset   | 提交层面 | 在私有分支上舍弃一些没有提交的更改 |
|  git reset   | 文件层面 | 将文件从缓存区中移除        |
| git checkout | 提交层面 | 切换分支或查看旧版本        |
| git checkout | 文件层面 | 舍弃工作目录中的更改        |
|  git revert  | 提交层面 | 在公共分支上回滚更改        |
|  git revert  | 文件层面 | （**木有**）           |


---------

<center><img src="https://subscription-1255463026.cos.ap-guangzhou.myqcloud.com/subscription.png" width="180" ></center>
