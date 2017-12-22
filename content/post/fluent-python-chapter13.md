---
title: "python运算符"
date: 2017-12-15T13:06:01+08:00
draft: false
categories: ['python']
tags: ['fluent python']
---

## python中的运算符重载

- 不能重载内置类型的运算符
- 不能新建运算符，只能重载现有的
- is, and, or 和not 不能重载(位运算符可以)

### 一元运算符
- **-** `(__neg__)`: 一元取符
- **+** `(__pos__)`: 一元取正
- **~** `(__invert__)`: 对整数按位取反, ~x == -(x+1). 
如果x是2,那么~x==-3。
- **abs()** `(__abs__)`: 绝对值
实现一元运算符只需要实现这些特殊方法。它们只有一个参数，self。但是，要始终返回一个新对象。多数时候 `+`最好返回self的副本。
###### 扩展
x != +x的案例。一个在decimal.Decimal.一个在collections.Counter

### 重载加法运算符 **+**

使用itertools.zip_longest()函数来对向量实例进行值的加法运算。
```python
def __add__(self,other):
    pairs = itertools.zip_longest(self, other. fillvalue=0.0)
    return Vector(a + b for a, b in pairs)
```
注意：实现一元运算符跟中缀运算符的特殊方法一定不能修改操作数。即不能修改self。只有增量赋值表达式可能会修改self。

还需要实现`__radd__(self,other)`方法.这个方法可以看做是`__add__`的反向.(右向操作).

原因: python执行a+b时.先在a对象中找相应的`__add__`方法. 抛出NotImplemented异常后在b中寻找`__radd__`方法.

### 重载标量乘法运算符 __*__

使用白鹅类型。检查numbers.Real抽象基类。

### python3.5 中新添加的**@**运算符。

**@**中缀运算符：`__matmul__`

### 众多比较运算符

处理与中缀运算符大体相似.在两方面有重大区别.

- 正向和反向调用使用的是同一系列方法.

  1. 对`==`来说:正向跟反向都调用`__eq__`方法
  2. 正向的`__gt__`方法调用的是反向的`__lt__`方法.
- 对于`==`跟`!=`来说.反向调用失败不会抛出TypeError.而是比较对象的ID.
![image.png](http://upload-images.jianshu.io/upload_images/6865906-894f94377866a034.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 增量赋值运算符

- 增量赋值不会修改不可变目标，而是新建实例，然后重新绑定
- 如果没有实现`__iadd__` 增量赋值运算只是个语法糖。a += b 就是 a = a + b。
- 然后，如果实现了就地运算符方法，计算a += b的结果就会调用就地运算符方法。会修改左操作数，而不是新建对象。

