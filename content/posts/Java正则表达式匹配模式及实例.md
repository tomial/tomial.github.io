---
title: Java正则表达式匹配模式及实例
date: 2019-04-09 01:08:58

tags: 
- Java

categories : 
- Java
---

| 正则表达式 | 匹配 | 示例 |
|:------------:|:---------------:|:------:|
| `x`          |   单个字符    |  Java匹配`Java`                  |
| `.`          |  任意单个字符  |  Java匹配`J..a`                |
| (ab&#124;cd)    |     ab或cd    |  ten匹配t(en&#124;im)              |
| `[abc]`      |     a或b或c   |  Java匹配`J[abc]va`             |
| `[^abc]`     | abc外的字符    |  Java匹配`J[^efg]va`            |
| `[a-z]`      |  a-z的任意字符 | Java匹配`[A-Z]av[a-z]`   	   |
| `[^a-z]`     | 除a-z的任意字符 | Java匹配`Jav[^b-z]`            |
|`[a-e[m-p]]`  | a到e或m到p     | Java匹配 `[A-G[I-M]]av[a-d]`   |
|`[a-e&&[c-p]]`|a到e与c到p的交集 | Java匹配 `[A-P&&[I-M]]av[a-d]` |
| `\d`         |    一个数字     |    123 匹配`\\d`  |
| `\D`         |   一个非数字    |  $Java匹配` [\\D][\\D]ava `|
| `w`          |    单词字符     | Java1匹配 `[\\w]ava[\\w]` |
| `W`          |    非单词字符   |  $Java匹配`[\\W][\\w]ava`|
| `s`          |    空白字符    | Java 2匹配`Java\\s2`|
| `p*`         |p模式出现0次或任意多次(贪婪匹配)| aaaaab匹配`a*b`|
| `p+`         |p模式出现1次或任意多次| a匹配`a+b*`, able匹配`(ab)+.*`|
| `p?`         |p出现0或1次(非贪婪匹配，尽量少地匹配)|Java匹配`J?Java`和`J?ava`|
| `p{n}`       |p正好出现n次     |  aaaa匹配`a{4}`|
| `p{n,}`      |p至少出现n次     |  aaaa匹配`a{1,}`,a不匹配`a{2,}`|
| `p{n,m}`     |p出现次数在n到m次之间|  aaaa匹配`a{1,9}`,abb不匹配`a{2,9}bb`|

- `matches(regex:String) : boolean` : 如果字符串匹配模式，返回true

- `replaceAll(regex: String, replacement: String): String`： 将所有匹配模式的子串更换为replacement的字符串并返回新的字符串

- `replaceFirst(regex: String, replacement: String): String`： 将匹配模式的第一个子串更换为replacement的字符串并返回新的字符串

- `split(regex: String)：String[]`：通过将匹配模式的子串去除分隔字符串并将分割的几段字符串存入字符串数组并返回

- `split(regex: String, limit: int): String`：作用同上，limit参数控制匹配次数，传入小于等于0时功能等同于`split(String)`，大于等于0时匹配分隔`limit-1`次