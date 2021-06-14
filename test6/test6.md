# Oracle期末项目

(基于Oracle的Zero-征途游戏客户端系统数据库设计)

## 期末考核要求

- 自行设计一个信息系统的数据库项目，自拟系统名称。
- 设计项目涉及的表及表空间使用方案。至少5张表和5万条数据，两个表空间。
- 设计权限及用户分配方案。至少两类角色，两个用户。
- 在数据库中建立一个程序包，在包中用PL/SQL语言设计一些存储过程和函数，实现比较复杂的业务逻辑，用模拟数据进行执行计划分析。
- 设计自动备份方案或则手工备份方案。

## 1.项目简介

项目名称：Zero-征途游戏客户端系统

本项目是基于Oracle的Zero-征途游戏客户端系统数据库设计。

涉及角色/用户：

涉及表：User表、Hero表、Attribute表、Shop表、Order表、Warehose表

## 2. 数据表说明

1. User表：存储用户数据
   - user_id：INT 用户id
   - name：VARCHAR 用户名
   - password：VARCHAR 密码
   - gold：INT 金币
2. Hero表：游戏英雄表 
   - hero_Id：INT 英雄id
   - attribute_Id：INT 属性id
   - name：VARCHAR 用户名
3. Attribute表：英雄属性表
   - attribute_Id：INT 属性id
   - hp：INT 生命值
   - magic：INT 魔力值
   - attack：INT 攻击力
   - defense：INT 防御力
   - speed：INT 速度
4. Shop表：商店表
   - hero_id：INT 英雄id
   - price：INT 售卖价格
   - introduce：VARCHAR 介绍
5. Order表：订单表
   - order_Id：INT 订单id
   - user_id：INT 用户id
   - hero_Id：INT 英雄id
   - create_date：DataTime 创建时间
   - status：VARCHAR 订单状态
6. Warehose表：用户拥有的英雄仓库表
   - user_id：INT 用户id
   - hero_Id：INT 英雄id

## 3. 数据库逻辑结构

![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test6/img/%E6%95%B0%E6%8D%AE%E5%BA%93%E9%80%BB%E8%BE%91%E7%BB%93%E6%9E%84.png)















## 总结

通过本次期末项目，让我我对Oracle的基本概念如表和视图理解的更加深刻了，Oracle 数据库数据对象中最基本的是表和视图，其他还有约束、序列、函数、存储过程、包、触发器等。对数据库的操作可以基本归结为对数据对象的操作,理解和掌握 Oracle数据库对象是学习Oracle的捷径。同时让我对以前就比较迷惑的数据库、实例、表空间、用户、角色之间的关系豁然开朗：

- 数据库是物理数据、内存、操作系统进程的组合体
- 实例是用来访问和使用数据库的一块进程，它只存在于内存中
- Oracle数据库是通过表空间来存储物理表的，一个数据库实例可以有N个表空间，一个表空间下可以有N张表

本次实验让我对创建表空间的语法，条件语句、分支语句、循环语句的使用方法和常用的PL/SQL函数掌握的更加熟练了，掌握了使用SQL语句创建视图，并且学会了部分存储过程和触发器的使用。

通过本次期末项目，让我收获很大，对Oracle也更加的充满激情了



