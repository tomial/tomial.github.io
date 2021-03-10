---
title: ICPC Asia Urumqi 2017 G.The Mountain
date: 2019-08-09 16:58:08

tags:
- ACM

categories :
- 算法
---

All as we know, a mountain is a large landform that stretches above the surrounding land in a limited area. If we as the tourists take a picture of a distant mountain and print it out, the image on the surface of paper will be in the shape of a particular polygon.

From mathematics angle we can describe the range of the mountain in the picture as a list of distinct points, denoted by` (x1,y1)` to `(xn,yn)`. The first point is at the original point of the coordinate system and the last point is lying on the `x-axis`. All points else have positive `y` coordinates and incremental `x `coordinates. Specifically, all `x` coordinates satisfy `0=x1<x2<x3<...<xn`. All `y` coordinates are positive except the first and the last points whose `y` coordinates are zeroes.

The range of the mountain is the polygon whose boundary passes through points `(x1,y1)` to `(xn,yn)` in turn and goes back to the first point. In this problem, your task is to calculate the area of the range of a mountain in the picture.

### Input

The input has several test cases and the first line describes an integer `t (1 ≤ t ≤ 20)` which is the total number of cases.

In each case, the first line provides the integer `n (1 ≤ n ≤100)` which is the number of points used to describe the range of a mountain. Following *n* lines describe all points and the `i-th` line contains two integers `xi` and `yi yi (0 ≤ xi,yi ≤1000)` indicating the coordinate of the` i-th` point.

### Output

For each test case, output the area in a line with the precision of 66 digits.

#### 样例输入

```
3
3
0 0
1 1
2 0
4
0 0
5 10
10 15
15 0
5
0 0
3 7
7 2
9 10
13 0
```



#### 样例输出

```
1.000000
125.000000
60.500000
```

### 题解

给出n的值和n个点的坐标，求这几个点组成的多边形的面积。其中所有的点横纵坐标都大于0，且最后一个点在y轴上，故这个多边形在第一象限上。

公式：`area = |(x1y2 - y1x2) + (x2y3 - y2x3) + ... (xn-1yn - yn-1xn)/2|`

### 代码：

```java
import java.util.Scanner;
public class Polygon {
  public static void main(String[] args) {
    Scanner input = new Scanner(System.in);
    int c = input.nextInt();
    while(c-- > 0) {
      int pn = input.nextInt();
      int[][] p = new int[pn][2];
      for(int i = 0; i < pn; i ++) {
        p[i][0] = input.nextInt();
        p[i][1] = input.nextInt();
      }
      double sum = 0;
      for(int i = 0; i < p.length - 1; i++) {
        sum += p[i][0] * p[i+1][1] - p[i][1] * p[i+1][0];
      }
      sum /= 2.00;
      System.out.printf("%.6f\n",Math.abs(sum));
    }
  }
}
```

