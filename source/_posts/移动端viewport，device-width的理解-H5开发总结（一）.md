---
title: 移动端viewport，device-width的理解 - H5开发总结（一）
date: 2016-12-28 20:44:06
tags: [移动端,H5,viewport,device-width]
categories: 前端
---
## 前言

进入新公司一周多时间，主要负责hybrid app的开发，写一些H5的web页面。之前一直搞PC端页面，对移动端页面了解甚少。通过这几天的学习与google。对目前的移动端开发事项加以总结。

## 一些基本概念

进入主题之前，首先要理清一些基本概念。像素，分辨率，屏幕尺寸。

### 屏幕尺寸

平常所说的手机的尺寸，比如5.5英寸iPhone6s(1英寸=2.54厘米)，这里的屏幕尺寸是指的屏幕对角线的绝对长度。

### 像素

像素单位是px(pixel)，表示的是成像的基本单元。前端一般会遇到css pixels与device pixels两种像素概念。

- CSS pixels : css像素，是浏览器使用的抽象单位，为web开发者创造的。就是我们平常在css写的样式，如（width:200px）。
- device pixels：设备屏幕的物理像素。

css像素与设备像素没有关系。也就是说1个CSS像素占多少个物理像素是不确定的。这个问题通过页面的放缩比较容易理解。比如一个页面，有一个css像素为300px的div；如果我们放大页面，这个元素会占据更多的设备像素（即device pixels），但它的CSS pixels不变，依然是300px；缩小页面也是同样的道理。所以，元素占据了多少device pixels，是由当前页面的放缩比例（scale）而定的。



## 分辨率
分辨率表示的是单位长度包含的像素数，分辨率的基本单位是像素，准确的说应该是每英寸像素（密度）。这个跟我们关系不大，暂且不讨论。

# DevicePixelRadio（设备像素比）

DevicePixelRadio 是设备的物理像素和设备独立像素（device-independent pixels，即dips）的比例。公式表示就是：window.devicePixelRatio = 物理像素/dips（有个前提是缩放比例为1）。


下面进入今天的正题。

搞移动端开发的前端都知道我们经常在头部加上这样一行
```
<meta name="viewport" content="width=device-width, initial-scale=1.0, minimum-scale=1.0, maximum-scale=1.0" />
```
这个`viewport`神马鬼，`device-width`又是神马鬼呢。不要懵逼，待我慢慢道来...

### Viewport

Viewport，是关于设备窗口表示，分为visual viewport 和  layout viewport。

- visual viewport ：即视觉窗口，可以理解为设备自己的宽度内的区域窗口，对应的是device pixels。
- layout viewport： 虚拟视觉窗口。为了解决移动端正常显示而产生的。


```
<meta name="viewport" content="width=device-width" />
```
这句话的意思将视图窗口（visual viewport）的宽度设为虚拟视图的宽度。

例如：iPhone 4的分辨率为640px * 1136px,设备宽度却只有为320px，DevicePixelRadio（设备像素比）为2
width 表示设备宽度（320px），device-width表示虚拟视图的宽度（640px）。上面的意思是告诉浏览器将视觉窗口的宽度设为虚拟视图的宽度。这样宽度只有320px的屏幕能正常显示宽度640px的页面内容了。

一般的常用的meta标签实例：
```
<meta name="viewport" content="width=device-width,initial-scale=1, maximum-scale=1, minimum-scale=1, user-scalable=no">
```
- init-scale：设置页面的初始缩放程度
- maximum-scale：设置了页面最大缩放程度
- minimum-scale：设置了页面最小缩放程度
- user-scalable：是否允许用户对页面进行缩放操作

上面的意思是设备视图宽度设为虚拟视图的宽度，页面的初始缩放比例以及最大缩放比例都为1，且不允许用户对页面进行缩放操作。

---------------

<center><img src="http://ohwhjizw4.bkt.clouddn.com/kick_push_dribbble.gif" width="400" ></center>