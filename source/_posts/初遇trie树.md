---
title: 初遇trie树
date: 2017-06-25 18:15:44
tags: trie树
categories: 算法
---
今天用Ant-Design的Cascader组件的时候，数据格式为
```javascript
var data = [{
    "value": "浙江",
    "children": [{
        "value": "杭州",
        "children": [{
            "value": "西湖"
        }]
    }]
}, {
    "value": "四川",
    "children": [{
        "value": "成都",
        "children": [{
            "value": "锦里"
        }, {
            "value": "方所"
        }]
    }, {
        "value": "阿坝",
        "children": [{
            "value": "九寨沟"
        }]
    }]
}]
```
燃鹅，后端返回的数据是这个鸟样子
```javascript
var data = [{
    "province": "浙江",
    "city": "杭州",
    "name": "西湖"
}, {
    "province": "四川",
    "city": "成都",
    "name": "锦里"
}, {
    "province": "四川",
    "city": "成都",
    "name": "方所"
}, {
    "province": "四川",
    "city": "阿坝",
    "name": "九寨沟"
}]
```
想想实现也不难。就写个类似数据去重的方式实现(有密集恐惧者的请绕过)
```javascript
'use strict'

/**
 * 将一个没有层级的扁平对象,转换为树形结构({value, children})结构的对象
 * @param {array} tableData - 一个由对象构成的数组,里面的对象都是扁平的
 * @param {array} route - 一个由字符串构成的数组,字符串为前一数组中对象的key,最终
 * 输出的对象层级顺序为keys中字符串key的顺序
 * @return {array} 保存具有树形结构的对象
 */

var transObject = function (tableData, keys) {
    let hashTable = {},
        res = []
    for (let i = 0; i < tableData.length; i++) {
        if (!hashTable[tableData[i][keys[0]]]) {
            let len = res.push({
                value: tableData[i][keys[0]],
                children: []
            })
            // 在这里要保存key对应的数组序号,不然还要涉及到查找
            hashTable[tableData[i][keys[0]]] = {
                $$pos: len - 1
            }
        }
        if (!hashTable[tableData[i][keys[0]]][tableData[i][keys[1]]]) {
            let len = res[hashTable[tableData[i][keys[0]]].$$pos].children.push({
                value: tableData[i][keys[1]],
                children: []
            })
            hashTable[tableData[i][keys[0]]][tableData[i][keys[1]]] = {
                $$pos: len - 1
            }
        }
        res[hashTable[tableData[i][keys[0]]].$$pos].children[hashTable[tableData[i][keys[0]]][tableData[i][keys[1]]].$$pos].children.push({
            value: tableData[i][keys[2]]
        })
    }
    return res
}

var data = [{
    "province": "浙江",
    "city": "杭州",
    "name": "西湖"
}, {
    "province": "四川",
    "city": "成都",
    "name": "锦里"
}, {
    "province": "四川",
    "city": "成都",
    "name": "方所"
}, {
    "province": "四川",
    "city": "阿坝",
    "name": "九寨沟"
}]

var keys = ['province', 'city', 'name']

console.log(transObject(data, keys))

```
虽然代码写的不忍直视，但毕竟是实现了功能，后来有一天，产品让改成四级的。瞬间懵逼。
求助之，后端让我用`trie树` 实现。

`trie树`貌似大学学数据结构的时候学过，完全忘记了，一番google之，又重新里两外一个版本。

```javacript
    let hashTable = {}, // 定义trie数结
        res = []; //最终返回的数据
    for (let i = 0; i < tableData.length; i++) {
        let arr = res,
            cur = hashTable; // 临时保存
        for (let j = 0; j < keys.length; j++) {
            // 循环层级
            let key = keys[j],
                filed = tableData[i][key];
            if (!cur[filed]) {
                // 如果节点没有则插入
                let pusher = {value: filed}, tmp; // 临时保存pusher的children
                if (j !== keys.length - 1) {
                    tmp = [];
                    pusher.children = tmp;
                }
                cur[filed] = { $$pos: arr.push(pusher) - 1 }; //
                cur = cur[filed];
                arr = tmp;
            } else { // 继续遍历
                cur = cur[filed];
                arr = arr[cur.$$pos].children;
            }
        }
    }
    console.log(res, hashTable);
    return res;
};

var data = [
    {
        province: '浙江',
        city: '杭州',
        name: '西湖'
    },
    {
        province: '四川',
        city: '成都',
        name: '锦里'
    },
    {
        province: '四川',
        city: '成都',
        name: '方所'
    },
    {
        province: '四川',
        city: '阿坝',
        name: '九寨沟'
    }
];

var keys = ['province', 'city', 'name'];

console.log(transObject(data, keys));
```
这代码清爽多了，本质还是利用javascript的引用类型，实现类似于指针的功能。
看来有时间得看看常用的算法了...