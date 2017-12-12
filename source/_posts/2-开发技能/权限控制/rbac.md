---
title: RBAC
date: '2016-04-10 14:36'
layout: page
tag:
  - rbac
toc: true
---

# 前言
在权限设计中，若权限比较复杂，使用简单的权限判断，则会使代码比较冗长，逻辑比较复杂，不利用维护和扩展。这时可以考虑使用 RBAC。

# RBAC 介绍

RBAC（Role-Based Access Control ）基于角色的访问控制。抽象出来就是用户，角色和权利三个方面。在权利和用户之间，增加一个角色，使其不再耦合在一起，方便维护和扩展。

一个角色拥有多个权利，一个用户想拥有某些权利时，只需要附给用户相应的角色即可。若一些用户需要相同的角色，则建立起用户组即可。

# RBAC 分类

## RBAC0
>RBAC核心,其他分类都是建立在此基础上

主要有四部分组成：

1. 用户
2. 角色
3. 权限
4. 会话

用户和角色是多对多关系，角色和权限是多对多关系。 **一个会话只能由一个用户创建**，会话和角色是多对多关系。
![rbac0_1](/images/2017/11/rbac0-1.png)
一般会有8张表来设计(若不需要用户组，则是5张表)：  
![rbac0_2](/images/2017/11/rbac0-2.png)

## RBAC1
> 基于 RBAC0 的角色分级；角色有了继承和包含的层级

这种模型使用 RBAC0 也是可以实现的，但是会造成数据冗余

## RBAC2
> 基于 RBAC0 的角色限制（互斥，数量限制等）

## RBAC3
> 组合 RBAC1 和 RBAC2

# RBAC 总结

通常使用 RBAC0 就可以解决大部分权限问题，其他的细节在具体的设计中具体分析。RBAC 只是一种设计思想，我们在设计的过程中要具体问题具体分析，灵活的使用，不要照本宣科。

# 参考资料

1. [权限管理——RBAC模型总结](http://blog.csdn.net/liujiahan629629/article/details/23128651)
2. [RBAC](http://baike.baidu.com/view/73432.htm)
3. [RBAC权限管理](http://blog.csdn.net/painsonline/article/details/7183613/)
4. [基于角色的访问控制](https://zh.wikipedia.org/wiki/以角色為基礎的存取控制)
