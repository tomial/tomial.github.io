---
title: 'JavaBean和Serializable,Mbeans'
date: 2019-11-25 23:15:45

tags: 
- Java

categories : 
- Java
---

## Java Bean

JavaBean简单来说是一种约定俗成的标准。

1. 所有的属性都是`private`的

2. 有一个`public`的无参构造函数

3. 实现了`Serializable`接口

可以看出，JavaBean就是一种惯例，按照要求实现的类就是一个JavaBean。

## Serializable

那么`Serializable`呢？从文档可以知道

> **可序列化**是一种实现了 `java.io.Serializable`接口的类的一种能力，没有实现这个接口的类则没有被序列化或反序列化的能力。而`Serializable`的子类自身也是可被序列化的。这个接口没有任何具体的方法，只有用来标记可序列化的语义。

可序列化的类（如JavaBean）可以被写入到Stream里面，这意味着它几乎可以被写入到任何地方。

之所以JavaBean有一个名字，大概是为了方便开发者称呼满足这类要求的类，比如传入一个方法的参数需要满足可被序列化的条件，那么它就可以要求这个参数是一个JavaBean。

## MBeans

从名字可以看出来，MBeans跟JavaBeans长得很像，同样，MBeans也是一种特定标准的名字。Java文档对MBeans的定义是：

>  An MBean is a managed Java object, similar to a JavaBeans component, that follows the design patterns set forth in the JMX specification. An MBean can represent a device, an application, or any resource that needs to be managed. MBeans expose a management interface that consists of the following: 
>
>  -   A set of readable or writable attributes, or both.
>  -   A set of invokable operations.
>  -   A self-description.

可以看出，一个MBean就是被一个被管理的资源：它可以是一个Java类、应用程序，甚至是一个设备，它需要满足一些要求：

- 一组可读/写的属性，或者二者兼顾
- 一组可以调用的操作
- 自我描述

MBeans自身遇到某类事件时也可以向管理者发出通知。

JMX定义了5种类型的MBeans：

-   Standard MBeans
-   Dynamic MBeans
-   Open MBeans
-   Model MBeans
-   MXBeans

这些类型的MBeans是一个个规范，按照要求实现接口即可。

各种MBeans可以在MBean Server上注册，之后就通过JConsole等工具管理这些资源。



参考链接：
[Lesson: Introducing MBeans -Java Documentation](https://docs.oracle.com/javase/tutorial/jmx/mbeans/index.html)

[What is a JavaBean exactly? -StackOverflow](https://stackoverflow.com/questions/3295496/what-is-a-javabean-exactly)