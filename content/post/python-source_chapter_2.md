---
title: "python-source-two"
date: 2018-01-22T13:06:01+08:00
draft: false
categories: ['c','python']
tags: ['python']
---

## Python中的整数对象

 python对象根据对象维护数据的可变现将对象分为“可变（mutable）”对象跟“不可变（immutable）”对象

`PyIntObject` 对象就是一个不可变对象
创建了一个`PyIntObject`后，再也不能改变该对象的值了。（字符串对象也如此）

Python中为高效使用整数设置了整数对象池。
<!--more-->

Python中的整数对象实际上就是对c中原生类型long的一个简单包装。

```c
[intobject.c]
PyTypeObject PyInt_Type = {
  PyObject_HEAD_INIT(&PyType_Type)
    0,
  "int",
  sizeof (PyIntObject),
  0,
  (destructor) int_dealloc,    /* tp_dealloc */
  (printfunc) int_print,        /* tp_print */
  0,                            /* tp_getattr */
  0,                            /* tp_setattr */
  (cmpfunc) int_compare,        /* tp_compare */
  (reprfunc) int_repr,          /* tp_repr */
  & int_as_number,          /* tp_as_number */
  0,                          /* tp_as_sequence */
  0,                             /* tp_as_mapping */
  (hashfunc) int_hash ,                        /* tp_hash */
  0,                            /* tp_call */
  (reprfunc) int_repr,          /* tp_str */
  PyObject_GenericGetAttr,      /* tp_getattro */
  0,                            /* tp_setattro */
  0,                            /* tp_as_buffer */
  Py_TPFLAGS_DEFAULT | Py_TPFLAGS_CHECKTYPES | Py_TPFLAGS_BASETYPES,  /* tp_flags */
  int_doc,                  /* tp_doc */
  0,                        /* tp_traverse */
  0,                        /* tp_clear */
  0,                        /* tp_richcompare */
  0,                        /* tp_weaklistoffset */
  0,                        /* tp_iter */
  0,                        /* tp_iternext */
  int_methods,              /* tp_methods */
  0,                        /* tp_members */
  0,                        /* tp_getset */
  0,                        /* tp_base */
  0,                        /* tp_dict */
  0,                        /* tp_descr_get */
  0,                        /* tp_descr_set */
  0,                        /* tp_dictoffset */
  0,                        /* tp_init */
  0,                        /* tp_alloc */
  int_new,                  /* tp_new */
  (freefunc) int_free       /* tp_free */
};
```

从上面的代码中可以看到`PyInt_Type`中保存了关于`PyIntObject`对象的丰富元信息

exp:如何比较2个整数对象的大小

```c
[intobject.c]
static int int_compare(PyIntObject *v, PyIntObject *w)
{
  register long i = v ->ob_ival;
  register long j = w ->ob_ival;
  retrun (i < j) ? -1 : (i > j) ? 1: 0;
}
```

- 实际上就是比较2个整数对象所维护的long值。

exp：来看看`int_as_number`这个域：

```c
[intobject.c]
static PyNumberMethods int_as_number = {
  (binaryfunc) int_add,     /* nb_add */
  (binaryfunc) int_sub,   /* nb_subtract */
  .....
  (binaryfunc) int_div,     
  int_true_divide,
  0,
  0,
};
```
- `PyNumberMethods` 中定义了一个对象作为数值对象时的所有可选操作信息,一共有39个函数指针
- `int_as_number` 中确定了对于一个整数对象这些数值操作应该怎么进行。当然,并非所有函数都要被实现。
- 查看`PyIntObject`中加法的实现

```c
[intobject.h]
// macro 牺牲类型安全，换取执行效率
#define PyInt_AS_LONG(op) (((PyIntObject *)(op)) -> ob_ival)

[intobject.c]
#define CONVERT_TO_LONG(obj, lng)  \
  if (PyInt_Check(obj)) {       \
    lng = PyInt_AS_LONG(obj);       \
  }             \
    else{       \
      Py_INCREF(Py_NotImplemented); \
      return Py_NotImplemented; \
    }       \

static PyObject* int_add(PyIntObject *v, PyIntObject *w)
{
  register long a, b, x;
  CONVERT_TO_LONG(v, a);
  CONVERT_TO_LONG(w, b);
  x = a + b;
  //检查加法结果是否溢出
  if ((x^a) >=0 || (x^b) >=0)
    return PyInt_FromLong(x);
  return PyLong_TYPE.tp_as_number -> nb_add ((Pyobject *)v, (PyObject *)w);
}
```


### PyIntObject对象的创建和维护

```c
PyObject *PyInt_FromLong(long ival)
```
可以直接调用c api 从一个long值生成PyIntObject对象。

## 小整数对象

- 使用了对象池技术。[-5, 127)

```c
#ifndef NSMALLPOSINTS
    #define NSMALLPOSINTS      257
#endif
#ifndef NSMALLNEGINTS
    #define NSMALLNEGINTS      5
#endif
#if NSMALLNEGINTS + NSMALLPOSINTS > 0
    static PyIntObject *small_ints[NSMALLNEGINTS + NSMALLPOSINTS];
#endif
```

## 大整数对象

- 对于小整数，使用小整数对象池
- 对于大整数，开辟一块内存空间轮流使用。

```c
#define BLOCK_SIZE 1000     /* 1k less tpyical malloc overhead */
#define BHEAD_SIZE 8        /* Enough for a 64-bit pointer */
#define N_INTOBJECTS        ((BLOCK_SIZE - BHEAD_SIZE) / sizeof (PyIntObject))

struct _intblock {
  struct _intblock *next;
  PyIntObject ojects[N_INTOBJECTS];
};

typedef struct _intblock PyIntBlock;

static PyIntBlock *block_list = NULL;
static PyIntObject *free_list = NULL;
```
- PyIntBlock : 结构中维护了一块内存，其中保存了一些`PyIntObject`对象。
- 每一个PyIntBlock中都维护了一个PyIntObject数组 --- obejcts. N_INTOBJECTS 简单计算是82个。即`object[92]`
- 使用`free_list`表示指向空闲内存的单向链表表头。

## 添加和删除

```c
PyObject* PyInt_FromLong(long ival)
{
  register PyIntObject *v;
#if NSMALLNEGINTS + NSMALLPOSINTS > 0
  if (-NSMALLNEGINTS <= ival && ival < NSMALLPOSINTS) {
    v = small_ints[ival + NSMALLNEGINTS];
    Py_INCREF(v);
    return (PyObject *) v;
  }
#endif
  if (free_list == NULL) {
    if ((free_list == fill_free_list()) == NULL)
      return NULL;
  }
  v = free_list;
  free_list = (PyIntObject *) v -> ob_type;
  PyObject_INIT(v, &PyInt_Type);
  v -> ob_ival = ival ;
  return (PyObject *) v;
}
```
PyIntObject 对象的创建通过两步完成：

- 如果小整数对象机制被激活，尝试使用小整数对象池；

NSMALLNEGINTS + NSMALLPOSINTS > 0 ,则小整数对象池机制被激活，对ival进行判断。是小整数则直接返回小整数对象池中的对应对象即可。

- 如果不在小整数范围，则使用通用的整数对象池。

只要`free_list`为`NULL`。就会调用`fill_free_list`.创建新的block。

```c
static PyIntObject* fill_free_list(void)
{
  PyIntObject *p, *q;
  // 申请大小为sizeof(PyIntBlock)的内存空间。链接到已有的block list 中
  p = (PyIntObject *) PyMem_MALLOC(sizeof(PyIntBlock));
  ((PyIntBlock *)p) -> next = block_list ;
  block_list = (PyIntBlock *)p;
  // 将PyIntBlock中的PyIntObject数组 --objects --转变为单向链表
  p = &((PyIntBlock *)p) -> objects[0];
  q = p + N_INTOBJECTS;
  while (--q >p)
    q -> ob_type = (struct _typeobject *)(q-1);
  q ->ob_type = NULL;
  return p + N_INTOBJECTS -1;
}
```





