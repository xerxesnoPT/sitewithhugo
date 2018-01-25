---
title: "python-source-one"
date: 2018-01-22T13:06:01+08:00
draft: false
categories: ['c','python']
tags: ['python']
---

## Python所有的对象PyObject

[object.h]
```c
typedef struct _object {
  PyObject_HEAD
} PyObject;
```

`PyObject_HEAD`是一个宏


[object.h]
```c
type def struct _object {
  int ob_refcnt;
  struct _typeobject *ob_type;
} PyObject;
```

python 对象机制核心非常简单

- 一个是引用计数
- 一个是类型信息

[intobject.h]
```c
typedef struct {
  PyObject_HEAD
  long ob_ival;
} PyIntObject;
```
python的整数对象中，除了PyObject。还有一个long变量，整数的值就保存在`ob_ival`中。

- 说明实际的Python对象中。不是仅仅只有一个PyObject。还占有一些额外内存。PyObject是必须的。

## 定长对象和变长对象(可变，不可变)
整数对象的特殊信息是一个C中的整形变量，始终可以保存在`ob_ival`中.但是,对于字符串.C中没有此类型,而是n个char型变量.
对于类似'n个...' python中有这么一个结构体- `PyVarObject`.

[object.h]
```c
    PyObject_HEAD
    int ob_size;

typedef struct {
  PyObject_VAR_HEAD
} PyVarObject;
```

- 定长（不可变）对象的不同对象占用的内存大小是一样的。整数 1 跟100 占用的内存大小都是sizeof（PyIntObject）。
- 变长 （可变) 对象占用的内存大小不同。例如字符串“Python”跟“Ruby”占用的内存大小不同。变长对象通常都是容器。`ob_size`指明了所容纳的元素的个数。

可以从定义中看到，`PyVarObejct`实际上只是对`PyObject`的一个扩展。所以，在Python内部，每一个对象都拥有相同的对象头部。所以在python中对对象的引用非常统一，只需要一个`PyObject*`指针即可.

## 类型对象

python对象创建时,一定知道需要申请多大的空间.这个对象的元信息存放在`_typeObject`.

[object.h]
```c
typedef struct _typeobject {
  char *tp_name; /* for printing */
  int tp_basicsize, tp_itemsize; /* For allocation */

  /* methods to implement standard operations */
  destructor tp_deadlloc;
  printfunc tp_print;
  ...
   /* More standard operations*/ 
  hashfunc tp_hash;
  ternaryfunc tp_call;
  ...
} PyTypeObject;
```

1. 类型名，tp_name,主要是python内部以及调试的时候使用;
2. 创建该类型对象时分配内存空间大小的信息 `tp_basicsize` and `tp_itemsize`;
3. 与该类型相关联的操作信息（就是诸如`tp_print`这样的函数指针）；
4. 类型的类型信息
    
### 对象的创建
python 中的c api分为2类
- 范型API。格式如`PyObject_***`的形式。创建一个整数对象`PyObject* intObj = PyObject_New(PyObject,&PyInt_Type)`
- 类型API，通常只能作用在某一种类型的对象上。创建整数对象 `Pyobject *intObj = PyInt_FromLong(10)`

### 对象的行为

在PyTypeObject中定义的大量函数指针可以视为类型对象中所定义的操作。直接决定着一个对象在运行时所表现出的行为

举例PyTypeObject中的`tp_hash`指明了该类型的对象如何生成hash值。
3组非常重要的操作族。
`tp_as_number` :-----> `PyNumberMethods` 整数
`tp_as_sequence`:-----> `PySequenceMethods` 序列（list）
`tp_as_mapping`:-----> `PyMappingMethods` 映射（dict）
[object.h]
```c
typedef PyObject * (*binaryfunc) (PyObject *, Pyobject *);

typedef struct{
  binaryfunc nb_add;
  binaryfunc nb_subtract;
  ...
} PyNumberMethods;
```
- 可以看到，在以上的代码中.定义了作为一个数值对象应该支持的操作。
- PyTypeObject 中允许一种类型同时指定三种不同对象的行为特征。

### 类型的类型

- 类型对象的类型是`PyType_Type`.

### 多态性


### 引用计数

是python垃圾收集机制的一部分。
- 类型对象是超越引用计数规则的。每一个对象中指向类型对象的指针不被视为对类型对象的引用。


















  
