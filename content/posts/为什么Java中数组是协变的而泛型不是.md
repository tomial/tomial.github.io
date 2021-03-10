---
title: 为什么Java中数组是协变的而泛型不是?
date: 2019-08-10 18:15:14

tags :
- Java

categories: 
- Java
---

### 什么是协变?

Effective Java中写到：

1.数组和泛型有两个主要的不同点。第一个是数组是协变的(covariant)，而泛型是不变的(invariant)。

2.协变简单来说就是如果`X`是`Y`的子类型则`X[]`也是 `Y[]`的子类型。因为数组是协变的，`String`是`Object`的子类型，故：

`String[] 是Object[]的子类型`

不变的(invariant)简单来说就是无论`X`是不是`Y`的子类型，

`List<X> 都不会是 List<Y>的子类型`

### 原因：

> 早期的Java和C#都没有泛型的特性(参数多态)
>
> 这种情况下，假设要写一个打乱数组功能的函数，或者一个测试数组元素是否相等的`Object.equals`方法，实现就不能只针对单一的类型，所以如果要写一个通用的函数，可以用以下的写法：
>
> `boolean equalArrays(Object[] a1, Object[] a2);`
>
> `void shuffleArray(Object[] a);`
>
> 上面提到，假设数组是不变的(invariant)，那么`String[]`不是`Object[]`的子类型，那么只能给这两个函数传`Object[]`类型的数组参数，所以设计者们将数组设计为协变的(covariant)，便可以将`String[]`、`Integer[]`等类型传进去，这两个函数就可以通用于各种类型。

上面就是“为什么Java中数组是协变的”的原因。

当泛型出现后，设计者有目的性地将泛型设计为不变的(invariant)

Jon Skeet:

> `List<Dog>`跟`List<Animal>`不是一个东西。想想你会怎么使用`List<Animal>`...你可以向里面添加各种动物，包括一只猫。逻辑上你没法往一群小狗里面加一只猫：

```java
 // Illegal code - because otherwise life would be Bad
 List<Dog> dogs = new List<Dog>();
 List<Animal> animals = dogs; // Awooga awooga
 animals.add(new Cat());
 Dog dog = dogs.get(0); // This should be safe, right?
```



`之所以没有像数组一样将泛型设计为协变的是因为**通配符(wildcards)**让协变表达式变得可能

```java
boolean equalLists(List<?> l1, List<?> l2);
void shuffleList(List<?> l);
```

参考资料：

[Why are arrays covariant but generics are invariant?--StackOverflow](https://stackoverflow.com/questions/18666710/why-are-arrays-covariant-but-generics-are-invariant)