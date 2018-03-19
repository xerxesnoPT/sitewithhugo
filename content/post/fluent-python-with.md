---
title: "python with管理器"
date: 2017-12-15T13:06:01+08:00
draft: false
categories: ['python']
tags: ['fluent python','with']
---

## 主要讨论：

- with 语句和上下文管理器
- for、while 和try语句的else子句
<!--more-->

## if语句之外的else块
else子句不仅能够在if语句中使用，还能在for, while, try语句中使用.

- for: 仅当for循环运行完毕时才执行else块.
- while: 仅当while循环因为条件为假值而退出时才运行else块.
- try: 仅当try块中没有异常抛出时才运行else块.

## 上下文管理器和with块

上下文管理器协议包含`__enter__`和`__exit__`两个方法.
with语句在开始运行时,会在上下文管理器对象上调用`__enter__`方法,结束后会调用`__exit__`方法. 用`__exit__`方法扮演finally子句的角色.

## contextlib模块中的实用工具

- closing：如果对象提供了close()方法，但没有实现`__enter__/__exit__`协议。可以使用这个函数构造上下文管理器。
- suppress：构建临时忽略指定异常的上下文管理器
- @contextmanager: 这个装饰器把简单的生成器函数变成了上下文管理器，这样就不用创建类去实现管理器协议了。
- ContextDecorator： 基类，用于定义基于类的上下文管理器。
- ExitStack：这个上下文管理器能够进入多个上下文管理器。with块结束时，ExitStack按照后进先出的顺序调用栈中各个上下文管理器的`__exit__`方法。

## 使用@contextmanager

在使用该装饰器的生成器中，yield语句的作用是把函数的定义体分为两部分： yield语句前面的所有代码在with块开始时执行（`__enter__`方法),yield后面的代码在with块结束时运行(`__exit__`方法).
