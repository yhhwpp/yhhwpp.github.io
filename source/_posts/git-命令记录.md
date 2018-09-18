---
title: git 命令记录
date: 2018-07-24 16:13:42
tags: Git
categories: Git
---

`git`一些常用命令，记不住，在此记录

<!-- more -->

## checkout 总结
```javascript
git checkout -b newBranch // 新建分支，并切换至新建的分支
git branch --set-upstream-to origin newBranch//设置该分支与远程分支的追踪关系
以上合并等于
 git checkout -b newBranch  origin/newBranch


git branch -D master-v1.0.1  // 删除本地分支
git push origin :master-v1.0.1  // 删除远程分支
```

## 本地连接建立与远程连接关联
```
git branch --set-upstream debug origin/debug
```


## tag
```
git tag v1.0 // 打标签
git tag -a v1.0 -m 'my version 1.4' // 含附注的标签
$ git push origin v1.0 // 推送


git tag -d v1.0.1 //删除本地tag：
git push origin :refs/tags/v1.0.1  删除远程tag

```

## git忽略已经被提交的文件办法：

`git rm --cached xxx`，删除`暂存区`与`分支`的文件，但本地不删除。然后更新 `.gitignore` 忽略掉目标文件，最后 git commit -m ""


---------

<center><img src="http://ohwhjizw4.bkt.clouddn.com/basketball-_400x300_.gif" width="400" ></center>