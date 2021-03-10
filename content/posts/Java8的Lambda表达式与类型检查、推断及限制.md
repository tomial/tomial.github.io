---
title: Java8的Lambda表达式与类型检查、推断及限制
date: 2019-04-23 22:12:15

tags: 
- Java

categories : 
- Java
---

为了全面了解Lambda表达式，我们应该知道Lambda的实际类型是什么。

## 类型检查

 	Lambda的类型**是从使用Lambda的上下文推断出来的**。上下文(即`接受它传递的方法参数`或`变量`)所需要的类型称为`目标类型`。下面的例子说明了Lambda表达式工作时**类型检查**的过程：

```java
List<Apple> heavierThan150g =
    	filter(inventory, (Apple a) -> a.getWeight() > 150);
```

1. 使用Lambda的**上下文所需的目标类型**是什么？

2. 可以看看filter的定义：`filter(List<Apple> inventory, Predicate<Apple> p)`

3. 可以知道**目标类型**为`Predicate<Apple>(T绑定到Apple类)`

4. `Predicate`接口定义的**抽象方法返回类型**是什么？

->`boolean test(Apple apple)`得到抽象方法为test，接收一个`Apple类型参数`并返回一个`boolean`值

->`(Apple a) -> a.getWeight() > 150`**函数描述符**`(Apple) -> boolean`匹配Lambda表达式，表示接收了一个`Apple参数`并返回一个`boolean`值。**类型检查无误；**

> 注：这里的函数描述符指的是抽象方法描述的Lambda表达式，包含了方法需要的参数列表和返回类型。如：(type 1, type 2, ...) -> return type，在上文中则为(Apple) -> boolean

当Lambda表达式抛出一个异常时，抽象方法所声明的throws语句也需要与之相匹配。

### 将相同的Lambda表达式作用于不同的函数式接口

由上文可以知道，不同的参数或变量只要有兼容的**目标类型，**Lambda表达式就可以作用于他们。比如

`Callable`接口和`PrivilegedAction`接口都拥有一个不接受参数而返回泛型T的抽象方法。所以下面的赋值就是有效的：

`Callable<Integer> c = () -> 42;`

`PrivilegedAction<Integer> p = () -> 42;`

第一个赋值语句的目标类型是`Callable<Integer>`，第二个是`PrivilegedAction<Integer>`

对void抽象方法的特殊兼容：

> 当Lambda的主体是一个语句，它就和一个返回void的函数描述符兼容。
>
> 如list.add(a)返回值一个boolean值
>
> 下面的语句是可行的：
>
> `Predicate<String> p = s -> list.add(s);`	//Predicate的抽象方法返回一个boolean值
>
> 下面的语句也是兼容的：
>
> Consumer<String> c = s -> list.add(s);				//Consumer的抽象方法返回一个void值

### 类型推断

Java编译器**可以推断适合的Lambda的签名**，因为Lambda的签名是由函数描述符得到的(即函数式接口的抽象方法)。比如下面的例子：

`List<Apple> greenApples = filter(inventory, a -> “green”.equals(a.getColor()));

例子里面的**参数a并没有指定它的类型**，Java编译器可以**自动推断出它的类型**，Lambda只有一个参数时可以省略括号。

相似地

`Comparator<Apple> c = (Apple a1, Apple a2) -> a1.getWeight().compareTo(a2.getWeight());`

可以写成

`Comparator<Apple> c = (a1, a2) -> a1.getWeight().compareTo(a2.getWeight());`

可以看到a1和a2的类型并没有给出，这两种写法都是可以的，选择哪种则看个人喜好。
