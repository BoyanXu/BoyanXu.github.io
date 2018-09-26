---
layout:     post
title:      C++ Struct (archieved)
subtitle:   
date:       2018-07-13
author:     Boyan
header-img: img/post-bg-math.jpg
catalog: true
tags:
    - C++
---
# Struct
结构体数据是构造数据类型：利用 struct 关键字
``` 
struct TypeName
{
int Name1;
int Name2;
float Name3;
}
```
 
## 利用结构体定义变量：
 
方法一：先构造，再定义
```
struct Tpyename
{
int Name1;
int Name2;
char Name3[3];
}
``` 
```
TypeName VariableName = {1，2，{'a','b','c'}}
``` 
(同定义内置类型变量的语法相同)
 
方法二：构造时定义
```
struct Tpyename
{
…
} VariableName1，VariableName2；
``` 
 
## 结构体的存储：
与数组存储类似，利用一片连续的存储空间
 
## 引用结构体包含的变量：
使用运算符 structName.PropertyName 
 
## 结构体变量赋值：
struct1 =  struct2 复制！创造副本!
 
## 结构体变量与函数：
  结构体作参数传递给函数：复制一份给函数的形式参数，创造副本
  结构体作函数返回值：复制一份副本，把副本返回到主函数里
 
## 结构体与指针：
 
指向结构体的指针 student *studentOne，要访问其所指向的结构体所包含的变量，
写 `(*studentOne).Name / studentOne -> Name`
注：-> 为指向运算符
 
## 指向结构体指针 作参数 传递给函数：
在被调函数内用 -> 访问结构体包含的变量
 
结构体数组，数组名为指向第一个结构体的指针
 
对结构体数组名++运算时，会跨过一整个结构体
 

 

