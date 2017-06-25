---
title: SSH keys与Deploy keys的区别
date: 2016-11-02 21:13:04
tags: Github
categories: Github
---
在 `Github` 上建了多个仓库，本人之前的一贯做法是，把`ssh`目录下的`id_rsa.pub`公钥copy至`repository`的`Deploy keys`，这样会导致另外一个`repository`无法增加`Deploy keys`，会提示`Key is already use`的错误。正确的做法是将`id_rsa.pub`添加至账户的`SSH keys`。

<!-- more -->

#### `SSH keys` 与 `Deploy keys` 的区别：
- `SSH keys` 是账户的最高的key，只要有账户的权限，其下的`repository`也是有权限访问的
- `repository` 的 `Deploy keys`， 是`repository` 的专有key
- 通俗的区别就是`SSH keys` 相当于 一个大房子的钥匙（这个钥匙可以打开屋里任意房间），而`Deploy keys`相当于房子里一个房间的钥匙

另外`id_rsa.pub`是公钥，可以添加至服务器段。本机的`id_rsa`是与之对应的私钥，密钥是成对的。

---------

<center><img src="http://ofstpx613.bkt.clouddn.com/cat-and-dog-drib-3.gif" width="400" ></center>