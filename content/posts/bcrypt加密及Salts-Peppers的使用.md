---
title: 'bcrypt加密及Salts,Peppers的使用'
date: 2019-12-07 12:46:26

tags:
- 安全

categories : 
- 数据库相关
---
<!--more-->

## Hash Function

我们知道，一般不会将用户的密码等重要信息的明文存储在数据库中，所以人们常用**散列函数(Hash Functions)**来给密码做加密后再存到数据库中。用户登录时，服务器用该密码求散列值来和数据库存储的值对比验证密码是否正确：

![MD5](/images/MD5.png)



常见的散列函数有MD5、SHA-1、bcrypt等。

散列函数的一个共同特点是：类似`password`和`passWord`这样微小差别的明文生成的散列值都是完全不一样的：

password+md5:`5F4DCC3B5AA765D61D8327DEB882CF99`

passWord+md5:`9334CF859035E2F0318320CEB88C7E6D`

而用于加密的密码散列函数还有一个特点：即加密是一个**单向**操作，无法从散列值反推获得明文。

因为以上的特点，破解者要获取一串散列值的原文方法有几种：

1. 暴力求解：遍历所有的数字和字母的组合来求散列值作对比

2. 彩虹表：彩虹表就是事先求出大量明文的散列值将其存到一个数据库中，可以在里面**搜索要破解的散列值**来找到其对应的明文。比如[hashkiller]( https://hashkiller.co.uk/Cracker/MD5 )，里面存储了8000亿个散列值，只要输入你的散列值他就可以找到对应的原文。

   ![hashkiller](/images/hashkiller.png)

看来纯散列函数的加密也不是那么安全，有人就想出了**Salt**和**Peppers**

## Salt

Salt是一串随机生成的字符，将slat加到明文后面再求散列值，并将散列值和salt一起存到数据库中，这就是**加盐(slating)**。用户登录时，服务器将用户登录的密码加上slat求散列值与数据库中的散列值对比验证密码是否正确。

![salting](/images/salting.png)

这样做的好处是使彩虹表破解这种方法失效，使破解者不得不使用暴力求解的方法来破解。

## Peppers

Peppers的做法和加盐差不多，它是在原文后面加上一个随机生成的字母，既可以是大写也可以是小写：



![pepper](/images/pepper.png)

peppers和slat的区别是，**pepper不会被存储到数据库中**。用户登陆时，服务器只需要尝试将52种字母分别加到用户密码后面求散列值并验证，对用户来说，登录验证时间是以往的52倍，仍然是可以接受的。

而对于破解者来说，52倍的时间使其破解难度大大增加。



参考资料：

### [Password History - Spring Security Reference](https://docs.spring.io/spring-security/site/docs/5.2.1.RELEASE/reference/htmlsingle/#pe-history)

### [Password Hashing, Salts, Peppers | Explained! - YouTube](https://www.youtube.com/watch?v=--tnZMuoK3E)
