---
title: "定义类型，方法"
date: 2017-11-02T13:06:01+08:00
draft: false
categories: ['haskell']
tags: ['hl基础语法']
---
## 定义新的数据类型

使用`data`关键字可以定义新的数据类型

```haskell
data BookInfo = Book Int String [String]
                          deriving (Show)
```
<!--more-->

上面代码中

- 跟在`data`后的` BookInfo`就是新类型的名字,我们称`BookInfo`为类型构造器. 类型构造器指代(refer)类型.
注意,类型的首字符必须大写,所以,类型构造器的首字母也必须大写.
- 接下来的`Book`是值构造器(也称为数据构造器)的名字.类型的值就是由值构造器创建的.值构造器的首字母也必须大写
- `Book`之后的`Int , String 和 [String]`是类型的组成部分.通常称为域。
在上面的例子中，`Int`表示一本书的ID, `String`表示书名, `[String]`代表作者.
`BookInfo`类型的结构跟(Int, String, [String])的三元组结构一样,但是他们的类型不同.
可以将值构造器看作是一个函数 - 它创建并返回某个类型值.
在ghci里定义变量需要使用`let`定义:

```haskell
*Main> let cities = Book 173 "Use of Weapons" ["Iain M. Banks"]
```

## 类型构造器和值构造器的命名

在Haskell里,类型的名字(类型构造器)跟值构造器的名字是相互独立的.类型构造器只能出现在类型的定义,或者类型签名中.而值构造器只能出现在实际的代码中.

## 类型别名

`type`关键字用于设置类型别名,其中新的类型名字在`=`号左边,已有类型放在右边.
```haskell
type CustomerID = Int
type ReviewBody = String
data BetterReview = BetterReview Bookinfo CustomerID ReviewBody
```
## 代数数据类型
`Bool`类型是代数数据类型最常见的例子.一个代数类型可以有多于一个值构造器.
```haskell
data Bool = False | True
```
使用多个值构造器时要指明值构造器的名字,并且传入对应的参数(参数个数跟类型)
## 何时用元组,何时用代数数据类型.
代数数据类型使得我们可以自结构相同但类型不同的数据之间进行区分.然而,对应元组来说,只要元素的结构跟类型都一致,那么元组的类型就是相同的.
## 枚举
Haskell中的枚举。不可能使用枚举值来代替Int值
```haskell
data Roygbiv = Red
             | Orange
             | Yellow
             | Green
             | Blue
             | Indigo
             | Violet
               deriving (Eq, Show)
```

## 模式匹配

对于某个类型的值来说，应该可以做到以下两点：

- 如果这个类型的值构造器有多个，应该知道这个值是由哪个构造器创建的。
- 如果一个值构造器包含不同的成分，那么应该有办法提取这些成分。
模式匹配允许我们查看值的内部，并将值所包含的数据绑定到变量上。
```haskell
myNot True = False
myNot False = True
```

这个自定义的函数通过模式匹配对Bool类型值进行了类似not函数的操作。

## 组成和解构

模式匹配的过程就像是逆转一个值的构造过程，因此它有时候也被称为解构。

## 记录语法

可以看做跟python中的字典参数一样的存在。

## 参数化类型

`Maybe`类型:既可以有值,也可以空缺.

## 递归类型

列表这种常见的类型就是递归的:它用自己来定义自己.

## let 语法

let 语法用来引入局部变量。
```haskell
lend amount balance = let reserve   = 100
                          newBalance = balance - amount
                      in if balance < reserve
                         then Nothing
                         else Just newBalance
```
上面的代码中使用了`let`关键字标识一个变量声明区块的开始,用`in`关键字标识了这个区块的结束

