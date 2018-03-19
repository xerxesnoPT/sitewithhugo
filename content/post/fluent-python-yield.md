---
title: "python yield协程"
date: 2017-12-20T13:06:01+08:00
draft: false
categories: ['python']
tags: ['fluent python','yield']
---

从语法上看，协程与生成器类似。但是协程中，yield通常出现在表达式的右边（例如， datum = yield）。协程可以从调用方接受数据，如**.send(datum)**方法。要理解协程，需要从根本上把yield视作控制流程的方式。

## 生成器如何进化为协程
- 添加了**.send(...)**, **.throw(...)**,**.close()**方法。
<!--more-->

### 用作协程的生成器的基本行为

协程可以处于四个状态中的一个，当前状态可以使用**inspect.getgeneratorstat(...)**函数确定

- **GEN_CREATED**:等待开始执行
- **GEN_RUNNING**:解释器正在执行
- **GEN_SUSPENDED**: 在yield表达式处暂停
- **GEN_CLOSED**: 执行结束
只有当处于暂停状态时才能调用send方法。

### 预激协程的装饰器
如果不预激(使用next首次启动协程，到达yield处停止),协程没什么用.我们可以使用一个预激装饰器 
```python
from functools import wraps
def coroutine(func):
    @wraps(func)
    def primer(*args,**kwargs):
    gen = func(*args, **kwargs)
    next(gen)
    return gen
return primer
```

### 终止协程和异常处理
协程中未出率的异常会向上抛出，传给next函数或者send方法的调用方（即触发协程的对象）。

- generator.throw(): 让生成器在暂停的yield表达式处抛出指定的异常，如果生成器处理了异常，代码继续向前执行到下一个yield表达式。产出的值会成为该调用方法的返回值。如果生成器中没有处理异常，向上抛出异常，传到调用方的上下文中。
- generator.close()：致使生成器在暂停的yield表达式处抛出GeneratorExit异常。

### 让协程返回值
协程的返回值return表达式的值会偷偷的传给调用方，赋值给StopIteration异常的一个属性。获取该属性可以从异常的value中拿到。
其实，yield from 结构跟for循环处理StopIteration异常的方式一样。解释器不仅会捕获StopIteration异常，还会把value属性的值变为yield from表达式的值。
