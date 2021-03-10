---
title: 如何理解REST/RESTful API
date: 2019-10-07 22:37:50

tags:
- Web

categories:
- 原理
---



## 什么是REST/RESTful?

REST起源于Roy Fielding在2000年发表的[博士论文](https://www.ics.uci.edu/~fielding/pubs/dissertation/fielding_dissertation.pdf)

REST的意思是**RE**presentational **S**tate **T**ransfer，中文意思为**表现层状态转移**。

首先要明白两个概念：

**客户端**：此时你在读这篇文章，你就请求获取了服务器上的这篇文章（资源），此时你的浏览器就是一个客户端

**资源**：资源可以是任何能够提供信息的东西：图片/文字等等等等，每个资源都有一个特定的**标识符**，通过这个标识符可以唯一指定一个资源

想象一下，将网页视为一个状态机，不同的网页状态视为状态机里的不同状态，通过RESTful服务就可以使你的网页对资源进行不同的操作：注册账号（新建资源）、请求页面（请求资源）、删除（删除资源）

![](/images/REST.gif)

REST不是一种协议，它是一种**架构/规范**，它通过约束你的API设计来使你的API变成**RESTful API**，RESTful，即按"REST"架构设计的服务

开发者们可以根据REST的约束来设计自己的API，来提供RESTful服务。

值得注意的是，**并不只有**HTTP协议才能构建RESTful服务，只是HTTP协议与REST架构能很好地协作，很适合将HTTP服务设计为RESTful服务，而**非HTTP协议**也可以用来构建RESTful服务。

调用RESTful API时，有两个东西决定了它的作用：

**标识符**：指定了操作的资源

**表示操作的HTTP方法/动作**：指定了对资源进行的操作

比如请求我的GITHUB主页：https://github.com/tomial，就使用了我的用户名作为标识符，GET方法作为对应的操作

## 设计RESTful API

REST规定了6种约束：

- Uniform interface
- Client — server separation
- Stateless
- Layered system
- Cacheable
- Code-on-demand

### Uniform interface



参考：

[**What is a RESTful API? --lazlojuly**](https://medium.com/@lazlojuly/what-is-a-restful-api-fabb8dc2afeb)

[**What is REST — A Simple Explanation for Beginners, Part 1: Introduction -- Shif Ben Avraham**](https://medium.com/extend/what-is-rest-a-simple-explanation-for-beginners-part-1-introduction-b4a072f8740f)



