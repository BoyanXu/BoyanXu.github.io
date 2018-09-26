---
layout:     post
title:      C++ Reference (archieved)
subtitle:   
date:       2018-07-13
author:     Boyan
header-img: img/post-bg-math.jpg
catalog: true
tags:
    - C++
---
```
int a;
int & r1 = a;
```

- 引用相当于为变量起别名，
- 引用变量的类型为 int & （以r1 为例）
- 引用关系不可改变。
 
- 引用作函数的形参：可以构造希望修改主程序变量的值 的函数：
例子：swap(a,b)
 
## 利用指针的情况：
```
void swap(int *a, int *b)
{
int temp;
*a = temp; *a = *b; *b = temp； 
}
```
```
int n1, n2;
swap(&n1, &n2);
 ```
## 利用引用的情况：
```
void swap(int &a, int *b)
{
int temp;
a = temp; a = b; b = temp； 
}
```
``` 
int n1, n2;
swap(n1, n2)
```
## 引用作函数的返回值：
```
int n;
int& SetValue() {return n;}
SetValue() = 1
```
 
## 常引用： const int &r = n;
不能通过引用名修改引用值


