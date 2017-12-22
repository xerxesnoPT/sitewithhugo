---
title: "typeclasses"
date: 2017-11-02T13:06:01+08:00
draft: false
categories: ['haskell']
tags: ['hl基础语法']
---

typeclasses: haskell 中的相当于类型接口的基类。
在检查`==`方法时
返回
```haskell
:t (==)
(==) :: (Eq a) => a -> a -> Bool
```
上面的代码中`=>`表示class constrain. 
可以这么理解: `==`方法提供两个参数.这两个参数必须是`Eq`类的成员. haskell中除了function跟io相关类型,其余类型都是`Eq` typeclass的一部分.

一些常见的typeclasses:

- `Eq`:供类型使用,提供了相等判断方法. 使用`==`和`/=`来实现
- `Ord`: 排序 `Ordering` is a type that can be `GT`, `LT` or `EQ`. 必须加入`Eq`成员组.
- `Show`: 能够使用strings来表示。使用`show`方法使用
- `Read` : 与`Show`相反,通过strings可以使用`read`方法把字符串转换为相应的类型。
- `Enum` : 枚举类型。都是连续的。每个值都有后继子跟前置子。分别可以通过`succ`跟`pred`函数获得. Types in this
class: (), Bool, Char, Ordering, Int, Integer, Float, Double.
- `Bounded` ：都有一个上限跟下限  
```haskell
ghci> minBound :: Int  
-2147483648  
ghci> maxBound :: Char  
'\1114111'  
ghci> maxBound :: Bool  
True  
ghci> minBound :: Bool  
False 
```
`minBound` 和 `maxBound` 都是多态常量

- `Num` : 是数字类型typeclass. 它的成员有数字的特征。
想作为`Num`的成员,这个类型必须已经实现了`Show`跟`Eq`



