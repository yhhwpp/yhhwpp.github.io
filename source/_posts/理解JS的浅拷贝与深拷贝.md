---
title: 理解JS的浅拷贝与深拷贝
date: 2016-11-03 20:41:09
tags: js
categories: js
---

JS 中的浅拷贝与深拷贝，只是针对复杂数据类型（`Object`，`Array`）的复制问题。浅拷贝与深拷贝都可以实现在已有对象上再生出一份的作用。但是对象的实例是存储在堆内存中然后通过一个引用值去操作对象，由此拷贝的时候就存在两种情况了：拷贝引用和拷贝实例，这也是浅拷贝和深拷贝的区别。

<!-- more -->

- **浅拷贝**：浅拷贝是拷贝引用，拷贝后的引用都是指向同一个对象的实例，彼此之间的操作会互相影响
- **深拷贝**：在堆中重新分配内存，并且把源对象*所有属性*都进行新建拷贝，以保证深拷贝的对象的引用图不包含任何原有对象或对象图上的任何对象，拷贝后的对象与原来的对象是完全隔离，互不影响

### 浅拷贝

浅拷贝分两种情况，拷贝直接拷贝源对象的引用 和 源对象拷贝实例，但其属性（类型为`Object`，`Array`的属性）拷贝引用。

#### 拷贝原对象的引用

这是最简单的浅拷贝。例：

```
var a = {c:1};
var b = a;
console.log(a === b); // 输出true。
a.c = 2;
console.log(b.c); // 输出 2

```

#### 源对象拷贝实例，其属性对象拷贝引用。

这种情况，外层源对象是拷贝实例，如果其属性元素为复杂杂数据类型时，内层元素拷贝引用。
对源对象直接操作，不影响两外一个对象，但是对其属性操作时候，会改变两外一个对象的属性的只。
常用方法为：`Array.prototype.slice()`, `Array.prototype.concat()`, `jQury`的`$.extend({},obj)`，例：
```
var a = [{c:1}, {d:2}];
var b = a.slice();
console.log(a === b); // 输出false，说明外层数组拷贝的是实例
a[0].c = 3;
console.log(b[0].c); // 输出 3，说明其元素拷贝的是引用
```

### 深拷贝

深拷贝后，两个对象，包括其内部的元素互不干扰。常见方法有`JSON.parse()`,`JSON.stringify()`，`jQury`的`$.extend(true,{},obj)`，`lodash`的`_.cloneDeep`和`_.clone(value, true)`。例：

```
var a = {c: {d: 1}};
var b = $.extend(true, {}, a);
console.log(a === b); // 输出false
a.c.d = 3;
console.log(b.c.d); // 输出 1，没有改变。
```

---------------

<center><img src="https://subscription-1255463026.cos.ap-guangzhou.myqcloud.com/subscription.png" width="180" ></center>

