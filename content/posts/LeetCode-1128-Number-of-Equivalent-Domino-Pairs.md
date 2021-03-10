---
title: LeetCode-1128-Number of Equivalent Domino Pairs(等价多米诺骨牌对的数量)
date: 2019-08-01 23:35:00

tags:
- LeetCode

categories :
- 算法
---

## 题目描述：

> Given a list of `dominoes`, `dominoes[i] = [a, b]` is equivalent to `dominoes[j] = [c, d]` if and only if either` (a==c and b==d)`, or `(a==d and b==c)` - that is, one domino can be rotated to be equal to another domino.
>
> Return the number of pairs `(i, j) `for which `0 <= i < j < dominoes.length`, and `dominoes[i]` is equivalent to `dominoes[j]`.

## **Example 1:**

```
Input: dominoes = [[1,2],[2,1],[3,4],[5,6]]
Output: 1
```

**Constraints:**

- `1 <= dominoes.length <= 40000`
- `1 <= dominoes[i][j] <= 9`

## 中文题目：

给你一个由一些多米诺骨牌组成的列表 `dominoes`。

如果其中某一张多米诺骨牌可以通过旋转 0 度或 180 度得到另一张多米诺骨牌，我们就认为这两张牌是等价的。

形式上，`dominoes[i] = [a, b]` 和 `dominoes[j] = [c, d]` 等价的前提是 `a==c` 且 `b==d`，或是` a==d` 且` b==c`。

在 `0 <= i < j < dominoes.length `的前提下，找出满足 `dominoes[i]` 和 `dominoes[j] `等价的骨牌对` (i, j)` 的数量。



## 题目解释：

找出类似`[1,2]`和`[2,1]`这种形式对称的骨牌对数，即满足`[a,b]`和`[c,d]`中，`a==c`和 `b==d`

## 思路：

提示指出`dominoes.length`在1到4w间，且骨牌的点数在1到9之间。故所有骨牌的种类只有`81`种，可以建立一个10*10(忽略下标为0)的二维数组，暴力遍历统计所有骨牌种类的数量，每种骨牌中**相互组合的对数即为该种骨牌满足条件的对数**。

## 代码：

```java
class Solution {
  public int numEquivDominoPairs(int[][] dominoes) {
    int num = 0;
    int[] m = new int[100];	//储存骨牌个数的数组
    for(int i : m) i = 0;
    
    for(int i = 0; i < dominoes.length; i++) {
      int a = 0;
      int b = 0;
      if(dominoes[i][0] > dominoes[i][1]) {	//将[a,b]和[b,a]视为同一种骨牌。
        a = dominoes[i][0];	
        b = dominoes[i][1];
      }
      else {
        b = dominoes[i][0];
        a = dominoes[i][1];
      }
      m[a*10+b] += 1;	//该种骨牌数量加一
    }
    for(int i : m) {
        num += (i*(i-1))/2;	//获得每种骨牌中互相组合个数之和
    }
    return num;
  }
}
```



总结：

- 多看提示，提示中若数量较小可以考虑使用暴力和数组
- 找相同数量/对数的问题考虑使用二维数组计数