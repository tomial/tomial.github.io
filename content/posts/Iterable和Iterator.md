---
title: Iterable和Iterator
date: 2019-04-09 22:27:12

tags: 
- Java

categories : 
- Java
---



## Iterator和Iterable的内部实现



`iterator`为Java中的迭代器对象，是**能够对List这样的集合进行迭代遍历的底层依赖**。而`iterable`接口里`定义了返回iterator的方法，相当于对iterator的封装，实现了iterable接口的类可以支持for each循环。`



`Iterator`接口主要方法如下：

```java
public interface Iterator<E> {
    boolean hasNext();
    E next();
}
```

`Iterator`接口定义了`boolean hasNExt()`和`E next()`两个方法，而迭代的方式则**取决于实现Iterator接口的方法中具体的实现**。

`Iterable`接口的定义如下：

```java
public interface Iterable<T> {
    Iterator<T> iterator();
}
```



`List`中**实现了Iterable**接口，所以可以调用`Iterator`中的`hasNext()`方法进行迭代

```java
Iterator it = list.iterator();
while (it.hasNext()) {
    System.out.print(it.next() + ",");
}
```

也可以使用foreach循环进行迭代**(使用foreach需要实现Iterable接口)**

```java
for (Element element : it) {
    System.out.print(element + " );
}
```

## foreach实现原理

java docs里面提到了foreach的实现，称之为The enhanced`for `statement(增强型for循环)

```java
for (I #i = Expression.iterator(); #i.hasNext(); ) {
    
	{VariableModifier} TargetType Identifier = (TargetType) #i.next();
    
	Statement
}
```

 - `#i` is an automatically generated identifier that is distinct from any other identifiers (automatically generated or otherwise) that are in scope ([§6.3](https://docs.oracle.com/javase/specs/jls/se8/html/jls-6.html#jls-6.3)) at the point where the enhanced `for` statement occurs.



`#i`是 一个自动生成的标识符，它和其他任何(自动生成之类的)标识符都不一样，`#i`会在foreach循环调用时在特定的作用域生成。



 - If the declared type of the local variable in the header of the enhanced `for` statement is a reference type, then *TargetType* is that declared type; otherwise, *TargetType* is the upper bound of the capture conversion ([§5.1.10](https://docs.oracle.com/javase/specs/jls/se8/html/jls-5.html#jls-5.1.10)) of the type argument of I, or `Object` if I is raw.



如果在foreach循环条件里传入了已经声明了类型的的本地变量，而且这个变量的类型是一个引用类型，那么`TargetType`就会变成这个变量的类型；否则，`TargetType`是`I`的一个上界，或者当`I`是原始类型`比如(T A)`时，这个变量的类型就为`Object`.



比如下面的例子：

 ```java
 List<? extends Integer> l = ...
 for (float i : l) ...
 ```

这段代码会被翻译为

```java
for (Iterator<Integer> #i = l.iterator(); #i.hasNext(); )  {
float #i0 = (Integer)#i.next();

  ...

```

可见Java的foreach循环是依赖于`Iterator`迭代器接口实现的，foreach循环的实现调用了其中的`iterator()`、`hasNext()`和`next()`方法



## Iterator和Iterable的关系

 之所以要将`Iterator`再封装一层形成`Iterable`接口，是因为这样做可以使实现了`Iterable`的类内部再定义多个`Iterator`内部类，从而达到不同遍历方法的目的。例如`LinkedList`中的`ListIterator`和`DescendingIterator`两个内部类，就分别实现了双向遍历和逆序遍历。通过返回不同的`Iterator`实现不同的遍历方式，这样更加灵活。如果把两个接口合并，就没法返回不同的`Iterator`实现类了。

 

 `ListIterator`的实现如下：

```java
public ListIterator<E> listIterator(int index) {
	checkPositionIndex(index);
	return new ListItr(index);
}
 
 private class ListItr implements ListIterator<E> {
 ...
     
 ListItr(int index) {
 	// assert isPositionIndex(index);
 	next = (index == size) ? null : node(index);
 	nextIndex = index;
 }
     
 public boolean hasNext() { 
 	return (nextIndex < size);
 }
     
 ...
```


 `DescendingIterator`源码如下：

 ```java
     public Iterator<E> descendingIterator() {
         return new DescendingIterator();
     }
 
     private class DescendingIterator implements Iterator<E>     {
         
         private final ListItr itr = new ListItr(size());
         
         public boolean hasNext() {
             return itr.hasPrevious();
         }
         
         public E next() {
             return itr.previous();
         }
         
         public void remove() {
             itr.remove();
         }
     }
 ```

 

可见，`ListIterator()`返回的是`ListItr`对象，而`descendingIterator()`返回的是`DescendingIterator`对象，这两个对象都实现了`Iterator`接口，从而实现了顺序不同的遍历。



参考资料：



[Java中的Iterable与Iterator详解](https://www.cnblogs.com/litexy/p/9744241.html)

[Java docs 14.14.2. The enhanced `for` statement](https://docs.oracle.com/javase/specs/jls/se8/html/jls-14.html#jls-14.14.2)