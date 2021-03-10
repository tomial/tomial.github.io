---
title: Spring Overview
date: 2020-05-16 10:51:43

tags: 
- Spring文档阅读

categories : 
- Spring
---

# Spring Overview

## Spring这个词指的是什么？

Spring一词既可以指`Spring Framework`这个Spring系列的起源框架，也可以指Spring这整个体系。`Spring Framework` 提供了许多模块供不同的应用选择，比如基于`Servlet`的`Spring MVC`，又或是与之平行关系的`Spring WebFlux`



## Spring的历史

```
Spring came into being in 2003 as a response to the complexity of the early J2EE specifications.
```

很多人认为Spring是J2EE的竞争对手，但Spring其实是对J2EE的补充，Spring整合了许多经过仔细挑选的J2EE规范：

- Servlet API ([JSR 340](https://jcp.org/en/jsr/detail?id=340))
- WebSocket API ([JSR 356](https://www.jcp.org/en/jsr/detail?id=356))
- Concurrency Utilities ([JSR 236](https://www.jcp.org/en/jsr/detail?id=236))
- JSON Binding API ([JSR 367](https://jcp.org/en/jsr/detail?id=367))
- Bean Validation ([JSR 303](https://jcp.org/en/jsr/detail?id=303))
- JPA ([JSR 338](https://jcp.org/en/jsr/detail?id=338))
- JMS ([JSR 914](https://jcp.org/en/jsr/detail?id=914))
- as well as JTA/JCA setups for transaction coordination, if necessary.

Spring系列框架一直在不断进化中，在Spring Framework之上还有很多别的项目：Spring Boot, Spring Security, Spring Data, Spring Cloud, Spring Batch等等。



## 设计哲学

学习一个框架很重要的一点是不仅要知道它做的什么还需要知道它遵循的设计原则。Spring遵循以下原则：

- 每一层都提供许多选择。Spring让你尽可能地推迟设计决策，解决同一个问题时可以很容易地在不同的组件中灵活的切换，直到找到满意的那一个。
- 容纳不同的观点。Spring在同一个问题上提供多种选择，拥抱灵活性而不是"opinionated"
- 强大的向后兼容性。
- 注重API的设计。Spring尽力设计符合直觉的API
- 对代码质量的高要求