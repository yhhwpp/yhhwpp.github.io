---
title: 实例解析防抖动（Debouncing）和节流阀（Throttling）
date: 2019-04-01 11:45:44
tags: javacript
categories: 前端
---

| 原文：[Debouncing and Throttling Explained Through Examples](https://css-tricks.com/debouncing-throttling-explained-examples/)


防抖（Debounce）和节流（throttle）都是用来控制某个函数在一定时间内执行多少次的技巧，两者相似而又不同。

当我们给 DOM 绑定事件的时候，加了防抖和节流的函数变得特别有用。为什么呢？因为我们在事件和函数执行之间加了一个控制层。记住，我们是无法控制 DOM 事件触发频率的。

<!-- more -->

看下滚动事件的例子：
<iframe height="265" style="width: 100%;" scrolling="no" title="Scroll events counter" src="//codepen.io/yhhwpp-the-typescripter/embed/YMzOPj/?height=265&theme-id=0&default-tab=result" frameborder="no" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/yhhwpp-the-typescripter/pen/YMzOPj/'>Scroll events counter</a> by hehe
  (<a href='https://codepen.io/yhhwpp-the-typescripter'>@yhhwpp-the-typescripter</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

当使用触控板，滚动滚轮，或者拖拽滚动条的时候，一秒可以轻松触发30次事件。经我的测试，在智能手机上，慢慢滚动一下，一秒可以触发事件100次之多。这么高的执行频率，你的滚动回调函数压力大吗？

早在2011年，Twitter 网站抛出了一个问题：向下滚动 Twitter 信息流的时候，变得很慢，很迟钝。John Resig 发表了[一篇博客](https://johnresig.com/blog/learning-from-twitter/)解释这个问题，文中解释到直接给 scroll 事件关联昂贵的函数，是多么糟糕的主意。

John（5年前）建议的解决方案是，在 onScroll 事件外部，每 250ms 循环执行一次。简单的技巧，避免了影响用户体验。

现如今，有一些稍微高端的方式处理事件。我来结合用例介绍下 `Debounce``，Throttle` 和 `requestAnimationFrame` 吧。


## 防抖动（Debounce）

防抖技术可以把多个顺序地调用合并成一次。

![debounce](https://user-images.githubusercontent.com/7929656/55302919-08155280-5476-11e9-8834-f62082da9f30.png)

假想一下，你在电梯中，门快要关了，突然有人准备上来。电梯并没有改变楼层，而是再次打开梯门。电梯延迟了改变楼层的功能，但是优化了资源。

在顶部按钮上点击或移动鼠标试一下：

<iframe height="265" style="width: 100%;" scrolling="no" title="Debounce. Trailing" src="//codepen.io/dcorb/embed/KVxGqN/?height=265&theme-id=0&default-tab=result" frameborder="no" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/dcorb/pen/KVxGqN/'>Debounce. Trailing</a> by Corbacho
  (<a href='https://codepen.io/dcorb'>@dcorb</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

你可以看到连续快速的事件是如何被一个 debounce 事件替代的。但是如果事件触发的时间间隔过长，debounce 则不会生效。

### 前缘（或者“immediate”）

你会发现，直到事件停止快速执行以后，debounce 事件才会触发相应功能。为何不立即触发呢？那样的话就跟原本的非 debounce 处理无异了。

直到两次快速调用之间的停顿结束，事件才会再次触发。

这是带 `leading` 标记的例子：

![p2](https://user-images.githubusercontent.com/7929656/55302993-5c203700-5476-11e9-80c2-8574fc5685fd.png)

前缘 `debounce` 的例子

在 `underscore.js` 中，选项叫 `immediate` ，而不是 `leading`：

<iframe height="265" style="width: 100%;" scrolling="no" title="Debounce. Leading" src="//codepen.io/yhhwpp-the-typescripter/embed/yrLxOJ/?height=265&theme-id=0&default-tab=result" frameborder="no" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/yhhwpp-the-typescripter/pen/yrLxOJ/'>Debounce. Leading</a> by hehe
  (<a href='https://codepen.io/yhhwpp-the-typescripter'>@yhhwpp-the-typescripter</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

### Debounce 实现

我首次看到 debounce 的 JavaScript 实现是在 2009 年的 John Hann 的博文。

不久后，Ben Alman 做了个 jQuery 插件（不再维护），一年后 Jeremy Ashkenas 把它加入了 underscore.js。而后加入了 Lodash 。

<iframe height="265" style="width: 100%;" scrolling="no" title="New example" src="//codepen.io/yhhwpp-the-typescripter/embed/eoYLZv/?height=265&theme-id=0&default-tab=result" frameborder="no" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/yhhwpp-the-typescripter/pen/eoYLZv/'>New example</a> by hehe
  (<a href='https://codepen.io/yhhwpp-the-typescripter'>@yhhwpp-the-typescripter</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>


`Lodash` 给 `_.debounce` 和 `_.throttle` 添加了不少特性。之前的 `immediate` 被 `leading（最前面）` 和 `trailing（最后面）` 选项取代。你可以选一种，或者都选，默认只有 trailing 启用。

新的 `maxWait` 选项（仅 `Lodash` `有）本文未提及，但是也很有用。事实上，throttle` 方法是用 `_.debounce` 加 `maxWait` 实现的，你可以看 `lodash` 源码。


### Debounce 实例

**调整大小的例子**

调整桌面浏览器窗口大小的时候，会触发很多次 resize 事件。

看下面 demo：

<iframe height="265" style="width: 100%;" scrolling="no" title="Debounce Resize Event Example" src="//codepen.io/yhhwpp-the-typescripter/embed/YMzOqJ/?height=265&theme-id=0&default-tab=result" frameborder="no" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/yhhwpp-the-typescripter/pen/YMzOqJ/'>Debounce Resize Event Example</a> by hehe
  (<a href='https://codepen.io/yhhwpp-the-typescripter'>@yhhwpp-the-typescripter</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

如你所见，我们为 resize 事件使用了默认的 trailing 选项，因为我们只关心用户停止调整大小后的最终值。

**基于 AJAX 请求的自动完成功能，通过 keypress 触发**

为什么用户还在输入的时候，每隔50ms就向服务器发送一次 AJAX 请求？_.debounce 可以帮忙，当用户停止输入的时候，再发送请求。

此处也不需要 `leading` 标记，我们想等最后一个字符输完。


<iframe height="265" style="width: 100%;" scrolling="no" title="Debouncing keystrokes Example" src="//codepen.io/yhhwpp-the-typescripter/embed/YMzOWJ/?height=265&theme-id=0&default-tab=result" frameborder="no" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/yhhwpp-the-typescripter/pen/YMzOWJ/'>Debouncing keystrokes Example</a> by hehe
  (<a href='https://codepen.io/yhhwpp-the-typescripter'>@yhhwpp-the-typescripter</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

相似的使用场景还有，直到用户输完，才验证输入的正确性，显示错误信息。

## 如何使用 debounce 和 throttle 以及常见的坑

自己造一个 debounce / throttle 的轮子看起来多么诱人，或者随便找个博文复制过来。我是建议直接使用 underscore 或 Lodash 。如果仅需要 _.debounce 和 _.throttle 方法，可以使用 Lodash 的自定义构建工具，生成一个 2KB 的压缩库。使用以下的简单命令即可：

```
npm i -g lodash-cli
lodash-cli include=debounce,throttle
```

常见的坑是，不止一次地调用 _.debounce 方法：

```
// 错误
$(window).on('scroll', function() {
   _.debounce(doSomething, 300); 
});
// 正确
$(window).on('scroll', _.debounce(doSomething, 200));
```


debounce 方法保存到一个变量以后，就可以用它的私有方法 debounced_version.cancel()，lodash 和 underscore.js 都有效。

```
var debounced_version = _.debounce(doSomething, 200);
$(window).on('scroll', debounced_version);
// 如果需要的话
debounced_version.cancel();
```



## Throttle（节流阀）

使用 _.throttle 的时候，只允许一个函数在 X 毫秒内执行一次。

跟 debounce 主要的不同在于，throttle 保证 X 毫秒内至少执行一次。

### 节流阀实例

**无限滚动**

用户向下滚动无限滚动页面，需要检查滚动位置距底部多远，如果邻近底部了，我们可以发 AJAX 请求获取更多的数据插入到页面中。

我们心爱的 `_.debounce` 就不适用了，只有当用户停止滚动的时候它才会触发。只要用户滚动至邻近底部时，我们就想获取内容。

使用 `_.throttle` 可以保证我们不断检查距离底部有多远

<iframe height="265" style="width: 100%;" scrolling="no" title="Infinite scrolling throttled" src="//codepen.io/yhhwpp-the-typescripter/embed/wZvEow/?height=265&theme-id=0&default-tab=result" frameborder="no" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/yhhwpp-the-typescripter/pen/wZvEow/'>Infinite scrolling throttled</a> by hehe
  (<a href='https://codepen.io/yhhwpp-the-typescripter'>@yhhwpp-the-typescripter</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>


#### requestAnimationFrame（rAF）

requestAnimationFrame 是另一种限速执行的方式。

跟 _.throttle(dosomething, 16) 等价。它是高保真的，如果追求更好的精确度的话，可以用浏览器原生的 API 。

可以使用 rAF API 替换 throttle 方法，考虑一下优缺点：

**优点**
- 动画保持 60fps（每一帧 16 ms），浏览器内部决定渲染的最佳时机
- 简洁标准的 API，后期维护成本低

**缺点**
- 动画的开始/取消需要开发者自己控制，不像 ‘.debounce’ 或 ‘.throttle’由函数内部处理。
- 浏览器标签未激活时，一切都不会执行。
- 尽管所有的现代浏览器都支持 rAF ，IE9，Opera Mini 和 老的 Android 还是需要打补丁。
- Node.js 不支持，无法在服务器端用于文件系统事件。
-  根据经验，如果 JavaScript 方法需要绘制或者直接改变属性，我会选择 requestAnimationFrame，只要涉及到重新计算元素位置，就可以使用它。

涉及到 AJAX 请求，添加/移除 class （可以触发 CSS 动画），我会选择 _.debounce 或者 _.throttle ，可以设置更低的执行频率（例子中的200ms 换成16ms）。

#### rAF 实例

灵感来自于 Paul Lewis 的文章，我将用 requestAnimationFrame 控制 scroll 。

16ms 的 _.throttle 拿来做对比，性能相仿，用于更复杂的场景时，rAF 可能效果更佳。

<iframe height="265" style="width: 100%;" scrolling="no" title="Scroll comparison requestAnimationFrame vs throttle" src="//codepen.io/yhhwpp-the-typescripter/embed/xexaRY/?height=265&theme-id=0&default-tab=result" frameborder="no" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/yhhwpp-the-typescripter/pen/xexaRY/'>Scroll comparison requestAnimationFrame vs throttle</a> by hehe
  (<a href='https://codepen.io/yhhwpp-the-typescripter'>@yhhwpp-the-typescripter</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>



## 结论

使用 debounce，throttle 和 requestAnimationFrame 都可以优化事件处理，三者各不相同，又相辅相成

总之：

- debounce：把触发非常频繁的事件（比如按键）合并成一次执行。

- throttle：保证每 X 毫秒恒定的执行次数，比如每200ms检查下滚动位置，并触发 CSS 动画。

- requestAnimationFrame：可替代 throttle ，函数需要重新计算和渲染屏幕上的元素时，想保证动画或变化的平滑性，可以用它。注意：IE9 不支持。


