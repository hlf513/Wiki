---
title: UML
date: '2016-02-28 23:22'
tag:
  - uml
toc: true
---

# UML 类图中的关系
## 关联关系
用于表示一类对象与另一类对象之间有联系
程序中，通常将一个类的对象作为另一个类的成员变量

### 双向关联
![shuangxiang](/images/2017/11/shuangxiang.jpg)
### 单向关联
![danxiang](/images/2017/11/danxiang.jpg)
### 自关联
>在系统中可能会存在一些类的属性对象类型为该类本身，这种特殊的关联关系称为自关联

![ziguanlian](/images/2017/11/ziguanlian.jpg)

### 多重性关联
>表示两个关联对象在数量上的对应关系

![duochongguanlian](/images/2017/11/duochongguanlian.jpg)

| 表示方式 | 多重性说明                                                 |
| -------- | ---------------------------------------------------------- |
| 1..1     | 表示另一个类的一个对象只与该类的一个对象有关系             |
| 0..\*    | 表示另一个类的一个对象与该类的零个或多个对象有关系         |
| 1..\*    | 表示另一个类的一个对象与该类的一个或多个对象有关系         |
| 0..1     | 表示另一个类的一个对象没有或只与该类的一个对象有关系       |
| m..n     | 表示另一个类的一个对象与该类最少m，最多n个对象有关系 (m≤n) |

## 聚合关系

>整体（整体对象）和部分（成员对象）可以分开独立存在

![juhe](/images/2017/11/juhe.jpg)

这里表示的是：汽车和发动机  
还有个更形象的比喻：雁群和雁子

>程序中，成员对象通常作为构造方法、Setter方法或业务方法的参数注入到整体对象中

## 组合关系
> 整体和部分不可分开，有相同的生命周期

![zuhe](/images/2017/11/zuhe.jpg)

这里表示的是：头和嘴  
还有个更形象的比喻：雁子和翅膀

>程序中，通常在整体类的构造方法中直接实例化成员类

## 依赖关系
>表示一个事物「使用」另一个事物时使用依赖关系

![yilai](/images/2017/11/yilai.jpg)

>程序中，依赖关系体现在某个类的方法使用另一个类的对象作为参数  
>  1. 将一个类的对象作为另一个类中方法的参数  
>  2. 在一个类的方法中将另一个类的对象作为其局部变量  
>  3. 在一个类的方法中调用另一个类的静态方法

## 泛化关系
>泛化关系就是继承关系

![fanhua](/images/2017/11/fanhua.jpg)

## 接口与实现关系
![jiekouyushixian](/images/2017/11/jiekouyushixian.jpg)

# 总结
各种关系的强弱顺序：
>泛化= 实现> 组合> 聚合> 关联> 依赖

![uml](/images/2017/11/uml.png)

# 参考资料
1. [深入浅出UML类图（二）](http://blog.csdn.net/lovelion/article/details/7842898)
2. [深入浅出UML类图（三）](http://blog.csdn.net/lovelion/article/details/7843308)
3. [UML类图几种关系的总结](http://blog.csdn.net/tianhai110/article/details/6339565)
