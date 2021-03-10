---
title: ICPC Asia Urumqi 2017 B.The Difference
date: 2019-08-09 17:21:45

tags:
- ACM

categories :
- 算法
---

Alice was always good at math. Her only weak points were multiplication and subtraction. To help her with that, Bob presented her with the following problem.

He gave her four positive integers. Alice can change their order optionally. Her task is to find an order, denoted by `A1,A2,A3` and `A4`, with the maximum value of `A1×A2 − A3×A4`.

### Input

The input contains several test cases and the first line provides an integer `t(1 ≤ t ≤ 100)`indicating the number of cases.

Each of the following `t` lines contains four space-separated integers.

All integers are positive and not greater than `100`.

### Output

For each test case, output a line with a single integer, the maximum value of `A1×A2 − A3×A4`.

#### 样例输入

```
5
1 2 3 4
2 2 2 2
7 4 3 8
100 99 98 97
100 100 1 2
```

#### 样例输出

```
10
0
44
394
9998
```

### 题解：

给出n个测试样例，每个样例有4个100以内的正整数，求`A1×A2 − A3×A4`的最大值。由于只有4个数，所以可以直接求出所有数两两相乘的所有组合，只有3!个，再将这些数排序用最大的减去最小的即是解。

### 代码：

```java
import java.util.Scanner;
import java.util.Arrays;
public class TheDif {
  public static void main(String[] args) {
    Scanner input = new Scanner(System.in);
    int n = input.nextInt();
    input.nextLine();
    while(n-- > 0) {
      int[] s = new int[4];
      int[] m = new int[6];
      for(int i = 0; i < 4; i++) {
        s[i] = input.nextInt();
      }
      m[0] = s[0] * s[1];
      m[1] = s[0] * s[2];
      m[2] = s[0] * s[3];
      m[3] = s[1] * s[2];
      m[4] = s[1] * s[3];
      m[5] = s[2] * s[3];
      Arrays.sort(m);
      System.out.println(m[5] - m [0]);
    }
  }
}
```



