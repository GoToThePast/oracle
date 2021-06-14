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

涉及表：User表、Hero表、Attribute表、Shop表、Game_Order表、Warehose表

## 2. 数据表说明

### 2.1 User表：存储用户数据

   - user_id：INT 用户id
   - name：VARCHAR 用户名
   - password：VARCHAR 密码
   - gold：INT 金币
### 2. 2 Hero表：游戏英雄表 
   - hero_Id：INT 英雄id
   - attribute_Id：INT 属性id
   - name：VARCHAR 用户名
### 2.3 Attribute表：英雄属性表
   - attribute_Id：INT 属性id
   - hp：INT 生命值
   - magic：INT 魔力值
   - attack：INT 攻击力
   - defense：INT 防御力
   - speed：INT 速度
### 2.4  Shop表：商店表
   - hero_id：INT 英雄id
   - price：INT 售卖价格
   - introduce：VARCHAR 介绍
### 2.5 Game_Order表：订单表
   - order_Id：INT 订单id
   - user_id：INT 用户id
   - hero_Id：INT 英雄id
   - create_date：DataTime 创建时间
   - status：VARCHAR 订单状态
### 2.6 Warehose表：用户拥有的英雄仓库表
   - user_id：INT 用户id
   - hero_Id：INT 英雄id



## 3. 数据库逻辑结构

![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test6/img/%E6%95%B0%E6%8D%AE%E5%BA%93%E9%80%BB%E8%BE%91%E7%BB%93%E6%9E%84.jpg)



## 4 设计简介

### 4.1 创建表空间

```sql
-- 创建表空间wang_users
CREATE TABLESPACE wang_users DATAFILE
'F:/Wangw/program/oradata/WANGW/wang_users_1.dbf'
SIZE 100M AUTOEXTEND ON NEXT 50M MAXSIZE UNLIMITED,
'F:/Wangw/program/oradata/WANGW/wang_users_2.dbf'
SIZE 100M AUTOEXTEND ON NEXT 50M MAXSIZE UNLIMITED
EXTENT MANAGEMENT LOCAL SEGMENT SPACE MANAGEMENT AUTO;

-- 创建表空间wang_users1
CREATE TABLESPACE wang_users1 DATAFILE
'F:/Wangw/program/oradata/WANGW/wang_users1_1.dbf'
SIZE 100M AUTOEXTEND ON NEXT 50M MAXSIZE UNLIMITED,
'F:/Wangw/program/oradata/WANGW/wang_users1_2.dbf'
SIZE 100M AUTOEXTEND ON NEXT 50M MAXSIZE UNLIMITED
EXTENT MANAGEMENT LOCAL SEGMENT SPACE MANAGEMENT AUTO;
```

![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test6/img/创建表空间.jpg)



### 4.2 创建数据表

#### 4.2.1 User表

```sql
CREATE TABLE GMAE_USER (
user_id int NOT NULL PRIMARY KEY,
name varchar(50),
password varchar(20),
gold int) 
partition by hash(user_id)(
partition part_01 tablespace wangw_users,
partition part_02 tablespace wangw_users1);
```

![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test6/img/创建User表.jpg)

#### 4.2. 2 Hero表

```sql
CREATE TABLE hero (
hero_Id int NOT NULL PRIMARY KEY,
attribute_Id int,
name varchar(255))
partition by hash(hero_Id)(
partition part_01 tablespace wangw_users,
partition part_02 tablespace wangw_users1);
```

![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test6/img/创建Hero表.jpg)

#### 4.2.3 Attribute表

```sql
CREATE TABLE attribute (
attribute_id int NOT NULL PRIMARY KEY,
hp int,
magic int,
attack int,
defense int,
speed int)
-- hash 分区
partition by hash(attribute_id)(
partition part_01 tablespace wangw_users,
partition part_02 tablespace wangw_users1);
```

![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test6/img/创建Attribute表.jpg)

#### 4.2.4  Shop表

```sql
CREATE TABLE shop (
hero_id int NOT NULL PRIMARY KEY,
price int,
introduce varchar(255)) 
partition by hash(hero_id)(
partition part_01 tablespace wangw_users,
partition part_02 tablespace wangw_users1);
```

![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test6/img/创建shop表.jpg)

#### 4.2.5 Game_Order表

```sql
CREATE TABLE game_order (
order_Id int NOT NULL PRIMARY KEY,
user_Id int,
hero_Id int,
create_date varchar(50),
status varchar(20))
partition by hash(order_Id)(
partition part_01 tablespace wangw_users,
partition part_02 tablespace wangw_users1);
```

![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test6/img/创建Order表.jpg)

#### 4.2.6 Warehose表

```sql
CREATE TABLE warehouse (
user_id int NOT NULL PRIMARY KEY,
hero_id int) 
partition by hash(user_id)(
partition part_01 tablespace wangw_users,
partition part_02 tablespace wangw_users1);
```

![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test6/img/创建Warehouse表.jpg)



## 4.3 向表中插入数据

#### 4.3.1 Attribute表

```sql
CREATE OR REPLACE PROCEDURE ADD_ATTRIBUTE AS 
BEGIN
    for i in 1..10000
    loop
        insert into attribute (ATTRIBUTE_ID,HP,MAGIC,ATTACK,DEFENSE,SPEED) 
        VALUES (i,100,100,15,25,25);
    end loop;
END ADD_ATTRIBUTE;
```

 ![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test6/img/插入Attribute表.jpg)

#### 4.3.2 Hero表

```sql
CREATE OR REPLACE PROCEDURE ADD_HERO AS 
v_name varchar(50); 
BEGIN 
  for i in 1..10000 
  loop  
    v_name := '张三'||i||'号'; 
    insert into hero (HERO_ID,ATTRIBUTE_ID,NAME) VALUES  
    (i,i,v_name); 
  end loop; 
END ADD_HERO; 
```

 ![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test6/img/插入Hero表.jpg)

#### 4.3.3 Game_order表

```sql
CREATE OR REPLACE PROCEDURE ADD_ORDER( 
    -- 键入USER 数量 
    in_user_count in number, 
    -- 键入英雄 数量 
    in_hero_count in number 
) AS  
    -- 显示声明 hero 游标 
    cursor c_hero is select HERO_ID from HERO; 
    -- 定义hero_id 
    v_hero_id HERO.HERO_ID%type; 
    -- 随机订单数 
    v_order_random_count number; 
    -- 随机用户ID 
    v_user_id game_order.user_id%type; 
    -- 日期 
    v_date date; 
    -- 全局订单index; 
    v_index game_order.order_id%type := 1; 
BEGIN 
   -- 打开游标 
   open c_hero; 
   -- 遍历游标 
   loop  
        FETCH c_hero into v_hero_id; 
        -- 退出条件 
        EXIT when c_hero%NOTFOUND; 
        -- 执行操作 
        -- 改用户随机订单数 
        -- 1. 获取随机数 
        SELECT trunc(dbms_random.value(1,5))into v_order_random_count FROM DUAL;  
        for k in 1..v_order_random_count 
        loop 
            -- 2.获得随机用户id 
            SELECT trunc(dbms_random.value(1,in_user_count))into v_user_id FROM DUAL; 
            -- 4.获取随机日期 
            v_date := TO_DATE(sysdate); 
--            DBMS_OUTPUT.PUT_LINE(v_hero_id||'&'||v_user_id||'&'||v_hero_id||'&'||v_date||'死'); 
            insert into GAME_ORDER (ORDER_ID,USER_ID,HERO_ID,CREATE_DATE,STATUS) 
            VALUES (v_index,v_user_id,v_hero_id,v_date,'死'); 
            v_index := v_index+1; 
        end loop; 
   end loop; 
   -- 关闭游标 
   close c_hero; 
END ADD_ORDER; 
```

 ![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test6/img/插入Game_order表1.jpg)

 ![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test6/img/插入Game_order表2.jpg)

#### 4.3.4 User表

```sql
-- user 表 
CREATE OR REPLACE PROCEDURE ADD_USER AS  
v_name varchar(50); 
v_password varchar(50); 
v_gole number; 
BEGIN 
    -- 20000名召唤师 
    for i in 1..20000 
    loop 
        v_name := '召唤师'||i||'号'; 
        v_password := 'password'||i||'.'; 
        -- 1. 获取随机金币 
        SELECT trunc(dbms_random.value(1,200000))into v_gole FROM DUAL;  
        insert into gmae_user (USER_ID,NAME,PASSWORD,GOLD) VALUES (i,v_name,v_password,v_gole); 
    end loop;
END ADD_USER; 
```

 ![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test6/img/插入User表.jpg)

#### 4.3.5 Shop表

```sql
CREATE OR REPLACE PROCEDURE ADD_SHOP AS  
    cursor c_hero is select HERO_ID from HERO; 
    v_hero_id HERO.HERO_ID%type; 
    -- shop介绍 
    v_shop_introduce varchar(80); 
    v_price shop.price%type; 
    v_index number := 1; 
BEGIN 
    open c_hero; 
    loop 
        fetch c_hero into v_hero_id; 
        exit when c_hero%NOTFOUND; 
        SELECT trunc(dbms_random.value(1,3200))into v_price FROM DUAL; 
        v_shop_introduce := '这是英雄'||v_hero_id||'号';
        insert into shop (HERO_ID,PRICE,INTRODUCE) 
        VALUES (v_hero_id,v_price,v_shop_introduce); 
    end loop; 
    close c_hero; 
END ADD_SHOP; 
```

 ![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test6/img/插入Shop表.jpg)

#### 4.3.6 Warehose表

```sql
CREATE OR REPLACE PROCEDURE ADD_WAREHOUSE( 
    in_hero_count in number 
) AS  
    cursor c_user is select USER_ID from GMAE_USER; 
    v_user_id warehouse.user_id%type; 
    v_hero_count number; 
    v_hero_id warehouse.hero_id%type; 
BEGIN 
    DBMS_OUTPUT.ENABLE(buffer_size => null); 
    open c_user; 
    loop 
        fetch c_user into v_user_id; 
        exit when c_user%NOTFOUND; 
        SELECT trunc(dbms_random.value(0,10))into v_hero_count FROM DUAL; 
        DBMS_OUTPUT.PUT_LINE('该轮循环走'||v_hero_count||'次'); 
        for i in 0..v_hero_count 
        loop  
            SELECT trunc(dbms_random.value(0,in_hero_count))into v_hero_id FROM DUAL; 
            insert into WAREHOUSE(USER_ID,HERO_ID)  
            VALUES (v_user_id,v_hero_id); 
            DBMS_OUTPUT.PUT_LINE('插入 v_user_id = '||v_user_id||' v_hero_id = '|| v_hero_id); 
        end loop; 
    end loop; 
    close c_user; 
END ADD_WAREHOUSE; 
```

 ![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test6/img/插入Warehose表.jpg)

## 5.设计权限及用户分配方案

### 5.1 创建角色

```sql
-- 角色1
CREATE ROLE wdb_manager IDENTIFIED BY 123456;

-- 角色2
CREATE ROLE wdb_reader IDENTIFIED BY 123456;
```

 ![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test6/img/q1.png)

### 5.2 角色授权

```sql
-- 角色授权
grant create session,create table,create sequence,create VIEW to wdb_manager;
grant create session to wdb_reader;
```

 ![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test6/img/q2.png)

### 5.3 查看结果

 ![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test6/img/q3.png)

 ![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test6/img/q4.png)

### 5.4 创建用户

```sql
-- USER SQL
CREATE USER "wangw" IDENTIFIED BY "123456"
DEFAULT TABLESPACE "WANGW_USERS"
TEMPORARY TABLESPACE "TEMP";
CREATE USER "wangw1" IDENTIFIED BY "123456"
DEFAULT TABLESPACE "WANGW_USERS1"
TEMPORARY TABLESPACE "TEMP";
-- QUOTAS
ALTER USER "wangw" QUOTA UNLIMITED ON "WANGW_USERS";
ALTER USER "wangw1" QUOTA UNLIMITED ON "WANGW_USERS1";
-- ROLES
GRANT "CONNECT" TO "wangw" ;
GRANT "CONNECT" TO "wangw1" ;
```



### 5.5 授权

 ![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test6/img/q5.png)

 ![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test6/img/q6.png)

## 6. 综合设计

**在数据库中建立一个程序包，在包中用PL/SQL语言设计一些存储过程和函数，实现比较复杂的业务逻辑，用模拟数据进行执行计划分析**

>设计简介
>
>包名：WW_WORK 
>
>函数名：get_user(user_id number)
>
>功能：输入用户id，根据用户仓库查询用户具有的英雄的属性
>
>过程名：Get_user_info(user_id number)

### 6.1 创建包 

```sql
-- 创建包 
CREATE OR REPLACE
PACKAGE WW_WORK AS
FUNCTION get_user(order_id_t NUMBER) RETURN VARCHAR2;
PROCEDURE get_user_info(train_id_t VARCHAR2);
END WW_WORK;
```

![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test6/img/res1.png)



### 6.2 创建函数和过程

```sql
create or replace PACKAGE BODY WW_WORK IS
FUNCTION get_user(user_id NUMBER) RETURN VARCHAR2
AS
M VARCHAR2(100);
BEGIN
select * into N from WAREHOUSE where USER_ID=user_id;
RETURN N;
END;
PROCEDURE get_user_info(user_id number)
AS
cursor c_hero is select HERO_ID from WAREHOUSE where USER_ID = in_user_id;
v_HERO_ID warehouse.hero_id%type;
v_ATTRIBUTE_ID hero.attribute_id%type;
v_HP attribute.hp%type;
v_MAGIC attribute.magic%type;
v_ATTACK attribute.attack%type;
v_DEFENSE attribute.defense%type;
v_SPEED attribute.speed%type;
BEGIN
DBMS_OUTPUT.ENABLE(buffer_size => null);
open c_hero;
loop
fetch c_hero into v_HERO_ID;
exit when c_hero%NOTFOUND;
select ATTRIBUTE_ID into v_ATTRIBUTE_ID from HERO where HERO_ID = v_HERO_ID;
select HP,MAGIC,ATTACK,DEFENSE,SPEED into
v_HP,v_MAGIC,v_ATTACK,v_DEFENSE,v_SPEED from ATTRIBUTE
where ATTRIBUTE_ID = v_ATTRIBUTE_ID;
DBMS_OUTPUT.PUT_LINE('英雄ID'||v_HERO_ID||'英雄血量'||v_HP||'英雄蓝量'||v_MAGIC||
'英雄攻击'||v_ATTACK||'英雄防御'||v_DEFENSE||'英雄移速'||v_SPEED);
end loop;
close c_hero;
end;
END WW_WORK;
```

![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test6/img/res2.png)

### 6.3 测试结果

![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test6/img/res3.png)



## 7.设计手动备份方案

>**手动备份方案——脱机备份**
>
>**自动备份方案——用户管理备份**

### 7.1 手动备份

1. 查询所有的数据文件、控制文件和联机重做日志文件

```sql
SELECT NAME FROM v$datafile
  UNION ALL
  SELECT MEMBER as NAME FROM v$logfile
  UNION ALL
SELECT NAME FROM v$controlfile;
```

![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test6/img/手动备份.jpg)

2. 手动备份这些文件 
3. 数据更改后只需将原数据复制到原来的文件即可 



### 7.2 自动备份

```sql
-- 让表空间进入备份模式 
ALTER TABLESPACE *** BEGIN BACKUP 
-- 复制表空间文件 
-- 让表空间结束备份模式 
ALTER TABLESPACE *** END BACKUP 
-- 复制控制文件和初始化文件 
-- 临时停止归档 
ALTER SYSTEM SWITCH LOGFILE; 
ALTER SYSTEM ARCHIVE LOG STOP; 
-- 复制归档日志文件 
-- 重新开启归档 
ALTER SYSTEM ARCHIVE LOG START; 
```



```sql
--1.查看归档模式 
archive log list; 
2.以archive模式启动数据库 
    2.2 shutdown immediate 
    2.3 startup mount 
    2.4 alter database archivelog; 
3.archive log list 
4.alter database open; 
5.ALTER tablespace users begin backup; 
```

![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test6/img/1.png)

![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test6/img/2.png)

![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test6/img/3.png)

![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test6/img/4.png)

![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test6/img/5.png)

![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test6/img/6.png)



复制数据库文件

 ![img](https://raw.githubusercontent.com/GoToThePast/oracle/master/test6/img/b1.png)

修改数据

 ![img](https://raw.githubusercontent.com/GoToThePast/oracle/master/test6/img/b2.png)

 ![img](https://raw.githubusercontent.com/GoToThePast/oracle/master/test6/img/b3.png) 

![img](https://raw.githubusercontent.com/GoToThePast/oracle/master/test6/img/b4.png)

结束备份

![img](https://raw.githubusercontent.com/GoToThePast/oracle/master/test6/img/b5.png)

## 8.总结

通过本次期末项目，让我对Oracle的基本概念如表和视图理解的更加深刻了，Oracle 数据库数据对象中最基本的是表和视图，其他还有约束、序列、函数、存储过程、包、触发器等。对数据库的操作可以基本归结为对数据对象的操作,理解和掌握 Oracle数据库对象是学习Oracle的捷径。同时让我对以前就比较迷惑的数据库、实例、表空间、用户、角色之间的关系豁然开朗：

- 数据库是物理数据、内存、操作系统进程的组合体
- 实例是用来访问和使用数据库的一块进程，它只存在于内存中
- Oracle数据库是通过表空间来存储物理表的，一个数据库实例可以有N个表空间，一个表空间下可以有N张表

本次实验让我对创建表空间的语法，条件语句、分支语句、循环语句的使用方法和常用的PL/SQL函数掌握的更加熟练了，掌握了使用SQL语句创建视图，并且学会了部分存储过程和触发器的使用。

通过本次期末项目，让我收获很大，对自己的能力与不足有了更加清晰的认识，并且对Oracle也更加的充满激情了



