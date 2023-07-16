author: Early0v0, frank-xjh, Great-designer, ksyx, qiqistyle, Tiphereth-A , Saisyc, shuzhouliu, Xeonacid, xyf007

本页面将简要介绍枚举算法。

## 简介

枚举（英文名：Enumerate），顾名思义，就是粗暴的枚举出所有可能的答案，然后判断答案是否符合条件，统计或者输出。

枚举也被称为暴力，这种方法对于一些简单的问题可以得到正解，对于一些复杂的问题也可以得到部分分数，而枚举也是 `DFS` 等算法的基础。

## 要点

### 给出解空间

建立简洁的数学模型。

枚举的时候要想清楚：可能的情况是什么？要枚举哪些要素？

### 减少枚举的空间

枚举的中心思想虽然是「粗暴的枚举出所有可能的答案」，但是枚举时大部分答案都是「无意义」的，对于这些答案，我们要尽可能规避，以此减少枚举的时间代价。

因此，枚举时要尽可能缩小枚举的范围，减少枚举的要素。

例如问题「给定一个正整数 $C$，输出满足 $A \times B = C$ 的 $A$ 和 $B$」。

对于这个问题，我们不需要用双重循环分别枚举 $A$ 和 $B$，可以只用一层循环枚举 $A$，此时 $B = C \div A$ ，然后输出即可，而枚举 $A$ 的范围则是 $[1,\sqrt C]$[^1]。 

### 选择合适的枚举顺序

根据题目判断。比如例题中要求的是最大的符合条件的素数，那自然是从大到小枚举比较合适。

## 例题

以下是一个使用枚举解题与优化枚举范围的例子。

??? 例题
    一个数组中的数互不相同，求其中和为 $0$ 的数对的个数。

??? note "解题思路"
    枚举两个数的代码很容易就可以写出来。
    
    === "C++"
    
        ```cpp
        for (int i = 0; i < n; ++i)
            for (int j = 0; j < n; ++j)
                if (a[i] + a[j] == 0) ++ans;
        ```
    
    === "Python"
    
        ```python
        for i in range(n):
            for j in range(n):
                if a[i] + a[j] == 0:
                    ans += 1
        ```
    
    来看看枚举的范围如何优化。由于题中没要求数对是有序的，答案就是有序的情况的两倍（考虑如果 `(a, b)` 是答案，那么 `(b, a)` 也是答案）。对于这种情况，只需统计人为要求有顺序之后的答案，最后再乘上 $2$ 就好了。
    
    不妨要求第一个数要出现在靠前的位置。代码如下：
    
    === "C++"
    
        ```cpp
        for (int i = 0; i < n; ++i)
            for (int j = 0; j < i; ++j)
                if (a[i] + a[j] == 0) ++ans;
        ```
    
    === "Python"
    
        ```python
        for i in range(n):
            for j in range(i):
                if a[i] + a[j] == 0:
                    ans += 1
        ```
    
    不难发现这里已经减少了 $j$ 的枚举范围，减少了这段代码的时间开销。
    
    我们可以在此之上进一步优化。
    
    两个数是否都一定要枚举出来呢？枚举其中一个数之后，题目的条件已经确定了其他的要素（另一个数）的条件，如果能找到一种方法直接判断题目要求的那个数是否存在，就可以省掉枚举后一个数的时间了。较为进阶地，在数据范围允许的情况下，我们可以使用桶[^2]记录遍历过的数。
    
    === "C++"
    
        ```cpp
        bool met[MAXN * 2];
        memset(met, 0, sizeof(met));
        for (int i = 0; i < n; ++i) {
            if (met[MAXN - a[i]]) ++ans;
            met[MAXN + a[i]] = true;
        }
        ```
    
    === "Python"
    
        ```python
        met = [False] * MAXN * 2
        for i in range(n):
            if met[MAXN - a[i]]:
                ans += 1
            met[a[i] + MAXN] = True
        ```

### 复杂度分析

-   时间复杂度分析：对 $a$ 数组遍历了一遍就能完成题目要求，当 $n$ 足够大的时候时间复杂度为 $O(n)$。
-   空间复杂度分析：$O(n+\max\{|x|:x\in a\})$。

## 习题

-   [2811: 熄灯问题 - OpenJudge](http://bailian.openjudge.cn/practice/2811/)

## 脚注

[^1]: 之所以只需判断 $A|C$ 是否成立，是因为如果 $C = A \times B$，那么 $B = C \div A$ 也应成立，但

[^2]: [桶排序](../basic/bucket-sort.md) 以及 [主元素问题](../misc/main-element.md#桶计数做法) 以及 [Stack Overflow 上对桶数据结构的讲解](https://stackoverflow.com/questions/42399355/what-is-a-bucket-or-double-bucket-data-structure)（英文）
