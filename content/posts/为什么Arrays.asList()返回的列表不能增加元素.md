---
title: 为什么Arrays.asList()返回的列表不能增加元素 
date: 2020-04-10 17:49:32

tags:
- Java

categories: 
- Java
- 原理
---

```java
String[] arr = {"A", "B"};
List<String> list = Arrays.asList(arr);
list.add("C");	//java.lang.UnsupportedOperationException
```

如果调用`Arrays.asList()`方法返回列表的add方法会抛出异常，这是什么原因呢？



在`asList`方法源码可以看到

```java
public static <T> List<T> asList(T... a) {
    return new ArrayList<>(a);
}
```

返回的是一个`ArrayList`列表，然而这个`ArrayList`跟常用的`java.util.ArrayList`并不是同一个东西

![java.util.Arrays.ArrayList](/images/Arrays-ArrayList.png)

![java.util.ArrayList](/images/util-ArrayList.png)



也就是说`Arrays.asList()`返回的并不是可扩容的那个`ArrayList`，而是在`Arrays`类内部写的另一个`ArrayList`，可以看到这两个`ArrayList`都继承了`AbstractList<T>`接口。



跳转到`AbstractList<T>`接口的`add()`方法定义处可以看到

```java
 public void add(int index, E element) {
     throw new UnsupportedOperationException();
 }
```

默认就是不支持`add`操作的。



而上面的注释也写到：

```
Note that this implementation throws an UnsupportedOperationException unless {add(int, Object) add(int, E)} is overridden.
// 除非add操作被重写否则不支持add操作
```

而因为`java.util.ArrayList`重写了add方法，`java.util.Arrays.ArrayList`没有重写，所以`Arrays.asList()`返回的列表不支持该操作。



参考链接：

[Arrays.asList使用指南-简书](https://www.jianshu.com/p/2b113f487e5e)