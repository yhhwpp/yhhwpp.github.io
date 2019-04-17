---
title: Fisher–Yates shuffle 洗牌算法
date: 2018-03-27 12:25:00
tags:
---

Fisher–Yates shuffle 洗牌算法是一个非常高效又公平的目前最好的随机排序算法。

<!-- more -->

## Fisher and Yates 的原始版

Fisher–Yates shuffle 的原始版本，最初描述在 1938 年的 Ronald Fisher（上图） 和 Frank Yates 写的书中，书名为《Statistical tables for biological, agricultural and medical research》。他们使用纸和笔去描述了这个算法，并使用了一个随机数表来提供随机数。它给出了 1 到 N 的数字的的随机排列，具体步骤如下：

```
1. 写下从 1 到 N 的数字
2. 取一个从 1 到剩下的数字（包括这个数字）的随机数 k
3. 从低位开始，得到第 k 个数字（这个数字还没有被取出），把它写在独立的一个列表的最后一位
4. 重复第 2 步，直到所有的数字都被取出
5. 第 3 步写出的这个序列，现在就是原始数字的随机排列
```

## 现代方法

Fisher–Yates shuffle 算法的现代版本是为计算机设计的。由 Richard Durstenfeld 在1964年 描述。并且是被 Donald E. Knuth 在 《The Art of Computer Programming》 中推广。但是不管是 Durstenfeld 还是 Knuth，都没有在书的第一版中承认这个算法是 Fisher 和 Yates 的研究成果。也许他们并不知道。不过后来出版的 《The Art of Computer Programming》提到了 Fisher 和 Yates 贡献。

现代版本的描述与原始略有不同，因为如果按照原始方法，愚蠢的计算机会花很多无用的时间去计算上述第 3 步的剩余数字。这里的方法是在每次迭代时交换这个被取出的数字到原始列表的最后。这样就将时间复杂度从 O(n^2) 减小到了 O(n)。算法的伪代码如下：

```
-- To shuffle an array a of n elements (indices 0..n-1):
for i from n−1 downto 1 do
     j ← random integer such that 0 ≤ j ≤ i
     exchange a[j] and a[i]
```


## javascript 实现
```
function shuffle(arr) {
    let i = arr.length;
    while (i) {
        let j = Math.floor(Math.random() * i--);
        [arr[j], arr[i]] = [arr[i], arr[j]];
    }
}
```

-----------

<center><img src="https://subscription-1255463026.cos.ap-guangzhou.myqcloud.com/subscription.png" width="180" ></center>

