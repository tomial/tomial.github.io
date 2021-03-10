---
title: Java中的二进制
date: 2019-05-15 23:20:33

tags: 
- Java

categories : 
- Java
---

Java SE 7之后可以使用二进制来定义`byte,short,int,long`这些整型变量。要使用二进制来表示整形，需要在数字开头带上`0b`或者`0B`，如下所示：

```java
// 一个8位的 `byte` 变量:
byte aByte = (byte)0b00100001;

// 一个16位的 `short` 变量:
short aShort = (short)0b1010000101000101;

// 一些32位的 `int` 变量:
int anInt1 = 0b10100001010001011010000101000101;
int anInt2 = 0b101;
int anInt3 = 0B101; // B可以是大写也可以是小写

// 一个64位的 `long` 变量 注意后缀"L":
long aLong = 0b1010000101000101101000010100010110100001010001011010000101000101L;
```

使用二进制可以比十六进制和十进制更明显的观察到数据间的关系：

```java
public static final int[] phases = {
  0b00110001,
  0b01100010,
  0b11000100,
  0b10001001,
  0b00010011,
  0b00100110,
  0b01001100,
  0b10011000
}  //二进制

public static final int[] phases = {
    0x31, 0x62, 0xC4, 0x89, 0x13, 0x26, 0x4C, 0x98
}  //十六进制
```

还可以使用`Integer.toBinaryString(int x)`获得`x`转化为二进制格式后的`字符串`。

```java
String s = Integer.toBinaryString(4)	//s的内容为"100"
```

### Java中二进制的异或运算：

Java中可以通过`^`运算符进行两个数之间的异或运算，它会**先将数字自动转化为二进制格式**再进行异或运算，然后**将二进制数字转回十进制格式**。

`System.out.print(4 ^ 1); //返回5`，即`先将4和1转换为 0100 和 0001，异或得 0101，转换回十进制得到5`。

可以使用`Integer.bitCount(int x ^ int y)`来`统计 x 和 y 异或得到的二进制格式数字中1的数量`



参考：[Binary Literals - Java SE Documentation](https://docs.oracle.com/javase/7/docs/technotes/guides/language/binary-literals.html)

