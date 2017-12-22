---
title: "柯里化"
date: 2017-11-01T13:06:01+08:00
draft: false
categories: ['haskell']
tags: ['hl基础语法']
---
## 柯里化
haskell 趣学指南中在高阶函数这一章中提出了柯里化概念。
举例`max`函数
首先看`max`函数的类型
```haskell
max :: Ord a => a -> a -> a
```
按照以前haskell中的方式来讲。
`max`这个函数需要2个a类型的参数.然后返回一个a类型的值.`max 4 5`中,4跟5就是参数.返回较大者5.
看下`max 4`的类型
```haskell
max 4 :: (Num a, Ord a) => a- >a
```
需要一个a类型（Ord跟Num的成员），返回一个a类型的值。

所有其实

max :: Ord a=> a -> a-> a 可以看做max :: Ord a=>a ->(a->a)

即`max 4`返回一个函数.该函数需要一个a类型的参数,返回a类型的值.

`max 4` 可以看做f (a, b)中a=4 .然后传入b的调用.

简而言之.

柯里化：在直观上。 f (x,y) = x+y.我们把x固定为4然后返回。就成了f (4,y) = 4+y .这个函数。此时只要考虑传入y即可。
python中的functools.partial（）函数就是这么用的。 廖雪峰教程中管它叫偏函数来着。
