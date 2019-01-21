---
title: "Rust学习- Array,Vec,String"
date: 2019-01-21T13:06:01+08:00
draft: false
categories: ['Rust']
tags: ['Language']

---

## 数组 Array
- 类型全部是相同的,有个数限制
- [T; N]: T表示类型，N表示个数
- exp:

``` rust
fn main() {
	let mut array: [i32; 3] = [0;3];
	array[1] = 1;
	array[2] = 2;

	assert_eq! ([1,2], &array[1..]);
	// This loop prinsts :0 1 2
	for x in &array{
		println!("{} ",x);
    }
}

```

## 动态数组 Vec
- 基于堆内存申请的联系动态数据类型，O(1)时间复杂度的索引，push，pop。

``` rust
//创建空vec
let v: Vec<i32> = Vec::new();
//使用宏创建vec
let v: Vec<i32> = vec![];
//创建包含5个元素的Vec
let v = vec![1, 2, 3, 4, 5];
//创建10个零
let v = vec![0, 10];
//创建可变的Vec 压入元素3
let mut v = vec![1, 2];
v.push(3);
//可变Vec v pop一个值
let mut v = vec!["1", "2"];
let two = v.pop();
//索引使用，改变vec中的值
let mut v = vec![1, 2, 3];
let three = v[2];
v[1] = v[1] + 5;
```

## 字符串
字符串分为String和str

- &str: `[u8]`类型的切片形式&[u8]. 大小固定的字符串类型。
- exp :

``` rust
//字符串面量值
let hello = "Hello, world!";
//其实带类型标识就是
let hello: &'static str = "Hello, world!";
```

- String: vec:Vec<u8>的结构体。也有push 跟pop

``` rust
 //创建一个空的字符串
let mut s = String::new();
//从`&str` 类型转为`String`类型
let mut hello = String::from("Hello, ");
//压入字符和压入字符串切片
hello.push('w');
hello.push_str('orld!');
//弹出字符
let mut s = String::from("foo");
assert_eq!(s.pop(), Some('o'));
assert_eq!(s.pop(), Some('o'));
assert_eq!(s.pop(), Some('f'));
assert_eq!(s.pop(), None);
```
