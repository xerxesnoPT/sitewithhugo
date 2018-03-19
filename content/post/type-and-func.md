---
title: "类型和函数"
date: 2017-11-03T13:06:01+08:00
draft: false
categories: ['haskell']
tags: ['hl基础语法']
---
## Haskell的类型系统

- 强类型：类型转换必须显示使用类型转换函数。
- 静态：不是python这种动态语言。java这样的编译时期就能发现类型不正确的表达式。Haskell提供的typeclass机制提供了大部分动态类型的特点。
- 可以通过自动推导得出
<!--more-->

## 一些常用的基本类型

- Char： 单个Unicode字符
- Bool：一个布尔逻辑值。只有2个值。 True和False
- Int：带符号的定长整数
- Integer: 不限长度的带符号整数。对Integer的计算不会造成溢出。因此使用Integer的计算结果更可靠。
- Double：浮点数，长度由机器决定，通常是64位。
`::` 这个符号用来标识类型.例如`:: T` 就是像Haskell表示,exp的类型是T. `:: T` 就是exp的类型签名。如果一个表达式没有显示地指名类型，它的类型就通过自动推导决定。

```Haskell
Prelude> :type 'a'
'a' :: Char

Prelude> 'a'            -- 自动推导
'a'

Prelude> 'a' :: Char    -- 显式签名
'a'
```
类型签名必须正确。
```Haskell
Prelude> 'a' :: Int     -- 试图将一个字符值标识为 Int 类型

<interactive>:7:1:
    Couldn't match expected type `Int' with actual type `Char'
    In the expression: 'a' :: Int
    In an equation for `it': it = 'a' :: Int
```
## 调用函数
要调用一个函数,首先写出它的名字.后面接函数的参数
```Haskell
Prelude> odd 3
True

Prelude> odd 6
False
```
注意.函数的参数不需要括号来包围,参数与参数直接直接通过空格来隔开.
```haskell
Prelude> compare 2 3
LT

Prelude> compare 3 3
EQ

Prelude> compare 3 2
GT
```
函数应用的优先级比操作符要高.

```haskell
Prelude> (compare 2 3) == LT
True

Prelude> compare 2 3 == LT
True
```

## 复合数据类型:列表和元组

复合类型通过其他类型构建得出.Haskell中最常用的是复合类型数据是列表跟元组.

- String 是[char]的别名,[Char]表示由Char类型组成的列表.
因为列表中的值可以是任意类型,所以列表是类型多态的.当编写带有多态类型的代码时,需要使用类型变量.这些类型变量用小写字母开头,作为一个占位符,最终被一个具体的类型替代(有点像java的泛型)
[a] 表示一个'类型为a的列表'
- 元组:跟列表的两个属性相反,元组长度固定,但可以包含不同类型的值.
Haskell 有一个特殊的类型(),这种类型只有一个值(),它的作用相当于包含零个元素的元组,类似于C语言中的void:

```haskell
Prelude> :t ()
() :: ()
```

元组通常用于以下两个地方:

- 如果一个函数需要返回多个值,可以将这些值包装到一个元组中,然后返回元组作为函数的值.
- 当需要使用定长容器,又没必要使用自定义的类型时,就可以使用元组来对值进行包装.
## 处理列表和元组的函数
- take和drop.接受两个参数,一个数字n和一个列表l.

```haskell
Prelude> take 2 [1, 2, 3, 4, 5]
[1,2]
Prelude> drop 2 [1, 2, 3, 4, 5]
[3,4,5]
```
- fst，snd接受一个二元组作为参数，返回该元组的第一个和第二个元素。

```haskell
Prelude> fst (1, 'a')
1

Prelude> snd (1, 'a')
'a'
```

## 将表达式传给函数

Haskell函数应用是左关联的。比如说，表达式a b c d 等同于(((a b) c) d)。要将一个表达式用作另一个表达式的参数。必须显式的使用括号来包围它。
```haskell
Prelude> head (drop 4 "azety")
'y'
```

## 函数类型

使用 :type命令可以查看函数的类型[缩写形式为：t]

```haskell
Prelude> :type lines
lines :: String -> [String]
```
通过类型签名显示，lines函数接受单个字符串，并返回包含字符串值的列表。
## 纯度
副作用：假设有某个函数，它读取并返回某个全局变量，如果程序中的其他代码可以修改这个全局变量，那么这个函数的返回值就取决于这个全局变量在某一时刻的值。
Haskell的函数在默认情况下都是无副作用的。函数的结果只取决于显式传入的参数。
带副作用的函数称为“不纯（impure）函数”。不带副作用的函数称为“pure函数“
从类型签名可以看出一个Haskell函数是否带有副作用 -不纯的函数类型签名都以IO开头:

```haskell
Prelude> :type readFile
readFile :: FilePath -> IO String
```
## Haskell源码，以及简单函数的定义

ghci只支持Haskell特性的一个非常受限的子集。因此，将代码写在源码文件里。
Haskell源码通常以.hs作为后缀.我们创建一个add.hs文件,并将以下定义添加到文件中:

```haskell
-- file: ch02/add.hs
add a b = a + b
```
`=`号左边的`add a b`是函数名和函数参数,而右边的`a+b`则是函数体,符号`=`表示将左边的名字（函数名和函数参数）定义为右边的表达式（函数体）。
将add.hs保存后，可以在ghci里通过: load （缩写为：l）载入。然后直接调用add函数即可。
注意，Haskell里一个函数就是一个单独的表达式，而不是一组陈述。所以不适用return关键字来返回函数值。
## 变量
- 在Haskell中，一旦变量绑定了某个表达式，name这个变量的值就不会改变，我们总能用这个变量来指代它所关联的表达式，而且每次都会得到同样的结果
- 在声明式语言中，变量的值可能无时无刻都处在改变当中。

## 条件求值
Haskell中的if表达式。
首先看自己实现的一个drop函数
```haskell
myDrop n xs = if n <= 0 || null xs
              then xs
              else myDrop (n - 1) (tail xs)
```

if 关键字引入了一个带有三个部分的表达式：

- 跟在if之后的是一个Bool类型表达式，是if的条件部分
- then 之后的是另一个表达式。这个表达式表示在if条件为
True时执行
- else之后的是另一个表达式，在条件部分是False时被执行。
- 跟在`then`跟`else`之后的表达式称为"分支".不同分支之间的类型必须相同.如果是`if True then 1 else "foo" `这样的表达式会产生错误，因为两个分支的类型并不相同。

## 通过示例了解求值

### 惰性求值
```haskell
isOdd n = mod n 2 == 1
```
一般而言,使用上面的isOdd函数计算isOdd (2+1),都是先计算2+1 = 3 然后在用isOdd的函数定义来进行判断.
但是在Haskell中.并不需要先计算出(2+1).只用把这个表达式存在块中.当这个子表达式的值真正被需要时才进行计算.

## Haskell里的多态.

观察last函数.这个函数返回列表中的最后一个元素,不管列表中元素的类型是int或者string.
last函数签名:
```haskell
last :: [a] -> a
```
a可以指代任意类型.

