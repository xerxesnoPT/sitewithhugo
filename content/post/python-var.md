---
title: "python变量"
date: 2018-01-15T13:06:01+08:00
draft: false
categories: ['python']
tags: ['python 小问题']
---

Python中，分为可变类型与不可变类型
这里有一个很简单的例子:

```python
a = 1
def funa(a):
    a = 2

print (a)


a = []

def funb(a):
    a.append(1)
print (a)
```

- 第一个print打印a为1,
- 第二个print打印a为[1].
解释是因为python中str,int,tuple为不可变类型,而list,dict,set是可变类型.

其实可以用汇编的角度来看:

- 第一个函数中, funa开始时,a作为参数首先入栈,此时已经把参数a指向对象1保留在栈中,调用funa时,a被赋予了新的指向对象2的引用.与外部无关.函数结束后,a出栈,重新指向对象1的引用.整个函数只是对内存的指向(类似c的指针)做了变化,内存中的内容并没发生变化.

- 第二个函数中,funb函数首先传入a的指向[]对象的引用.(看作内存地址)
然后使用了append函数对内存中的内容进行了改变.所以a=[1]. 如果如下

```python
def funb(a):
    a = [1,2,3,4]
print (a)
```
这样是无任何问题的.
