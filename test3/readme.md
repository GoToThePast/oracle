# 实验3：创建分区表

## 实验目的

掌握分区表的创建方法，掌握各种分区方式的使用场景。

## 实验内容

- 本实验使用3个表空间：USERS,USERS02,USERS03。在表空间中创建两张表：订单表(orders)与订单详表(order_details)。
- 使用**你自己的账号创建本实验的表**，表创建在上述3个分区，自定义分区策略。
- 你需要使用system用户给你自己的账号分配上述分区的使用权限。你需要使用system用户给你的用户分配可以查询执行计划的权限。
- 表创建成功后，插入数据，数据能并平均分布到各个分区。每个表的数据都应该大于1万行，对表进行联合查询。
- 写出插入数据的语句和查询数据的语句，并分析语句的执行计划。
- 进行分区与不分区的对比实验。



## 实验步骤

### 1. 登录

1. 连接ssh

![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test3/ssh.jpg)

       2. sys账号登录WEI

![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test3/login.jpg)

### 2. 创建表空间

1. 查看当前表空间

   ![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test3/tablespace_before.jpg)

2. 创建表空间users02、users03

   ![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test3/create.jpg)

3. 查看当前表空间

   ![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test3/tablespace_after.jpg)

### 3. 授权

1. 为wangwei_user 授权表空间权限

![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test3/grant.jpg)

### 4. 创建表

1. 登录到**wangwei_user**用户

   ![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test3/login_wangwei.jpg)

2. 创建orders表，并设定分区策略

![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test3/create_table_orders.jpg)

代码如下:

```sql
-- 创建 ORDERS表
CREATE TABLE ORDERS
(
  ORDER_ID NUMBER(10, 0) NOT NULL
, CUSTOMER_NAME VARCHAR2(40 BYTE) NOT NULL
, CUSTOMER_TEL VARCHAR2(40 BYTE) NOT NULL
, ORDER_DATE DATE NOT NULL
, EMPLOYEE_ID NUMBER(6, 0) NOT NULL
, DISCOUNT NUMBER(8, 2) DEFAULT 0
, TRADE_RECEIVABLE NUMBER(8, 2) DEFAULT 0
, CONSTRAINT ORDERS_PK PRIMARY KEY
  (
    ORDER_ID
  )
  USING INDEX
  (
      CREATE UNIQUE INDEX ORDERS_PK ON ORDERS (ORDER_ID ASC)
      LOGGING
      TABLESPACE USERS
      PCTFREE 10
      INITRANS 2
      STORAGE
      (
        BUFFER_POOL DEFAULT
      )
      NOPARALLEL
  )
  ENABLE
)
TABLESPACE USERS
PCTFREE 10
INITRANS 1
STORAGE
(
  BUFFER_POOL DEFAULT
)
NOCOMPRESS
NOPARALLEL
PARTITION BY RANGE (ORDER_DATE)
(
  PARTITION PARTITION_2015 VALUES LESS THAN (TO_DATE(' 2016-01-01 00:00:00', 'SYYYY-MM-DD HH24:MI:SS', 'NLS_CALENDAR=GREGORIAN'))
  NOLOGGING
  TABLESPACE USERS
  PCTFREE 10
  INITRANS 1
  STORAGE
  (
    INITIAL 8388608
    NEXT 1048576
    MINEXTENTS 1
    MAXEXTENTS UNLIMITED
    BUFFER_POOL DEFAULT
  )
  NOCOMPRESS NO INMEMORY
, PARTITION PARTITION_2016 VALUES LESS THAN (TO_DATE(' 2017-01-01 00:00:00', 'SYYYY-MM-DD HH24:MI:SS', 'NLS_CALENDAR=GREGORIAN'))
  NOLOGGING
  TABLESPACE USERS
  PCTFREE 10
  INITRANS 1
  STORAGE
  (
    BUFFER_POOL DEFAULT
  )
  NOCOMPRESS NO INMEMORY
, PARTITION PARTITION_2017 VALUES LESS THAN (TO_DATE(' 2018-01-01 00:00:00', 'SYYYY-MM-DD HH24:MI:SS', 'NLS_CALENDAR=GREGORIAN'))
  NOLOGGING
  TABLESPACE USERS
  PCTFREE 10
  INITRANS 1
  STORAGE
  (
    BUFFER_POOL DEFAULT
  )
  NOCOMPRESS NO INMEMORY
, PARTITION PARTITION_2018 VALUES LESS THAN (TO_DATE(' 2019-01-01 00:00:00', 'SYYYY-MM-DD HH24:MI:SS', 'NLS_CALENDAR=GREGORIAN'))
  NOLOGGING
  TABLESPACE USERS02
  PCTFREE 10
  INITRANS 1
  STORAGE
  (
    BUFFER_POOL DEFAULT
  )
  NOCOMPRESS NO INMEMORY
, PARTITION PARTITION_2019 VALUES LESS THAN (TO_DATE(' 2020-01-01 00:00:00', 'SYYYY-MM-DD HH24:MI:SS', 'NLS_CALENDAR=GREGORIAN'))
  NOLOGGING
  TABLESPACE USERS02
  PCTFREE 10
  INITRANS 1
  STORAGE
  (
    BUFFER_POOL DEFAULT
  )
  NOCOMPRESS NO INMEMORY
, PARTITION PARTITION_2020 VALUES LESS THAN (TO_DATE(' 2021-01-01 00:00:00', 'SYYYY-MM-DD HH24:MI:SS', 'NLS_CALENDAR=GREGORIAN'))
  NOLOGGING
  TABLESPACE USERS02
  PCTFREE 10
  INITRANS 1
  STORAGE
  (
    BUFFER_POOL DEFAULT
  )
  NOCOMPRESS NO INMEMORY
, PARTITION PARTITION_2021 VALUES LESS THAN (TO_DATE(' 2022-01-01 00:00:00', 'SYYYY-MM-DD HH24:MI:SS', 'NLS_CALENDAR=GREGORIAN'))
  NOLOGGING
  TABLESPACE USERS03
  PCTFREE 10
  INITRANS 1
  STORAGE
  (
    BUFFER_POOL DEFAULT
  )
  NOCOMPRESS NO INMEMORY
);

```



3. 创建表orders_details

   ![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test3/create_table_orders_details.jpg)

   代码如下:

   ```sql
   
   -- 创建 order_details表
   CREATE TABLE order_details
   (
   id NUMBER(10, 0) NOT NULL
   , order_id NUMBER(10, 0) NOT NULL
   , product_name VARCHAR2(40 BYTE) NOT NULL
   , product_num NUMBER(8, 2) NOT NULL
   , product_price NUMBER(8, 2) NOT NULL
   , CONSTRAINT order_details_fk1 FOREIGN KEY  (order_id)
   REFERENCES orders  (  order_id   )
   ENABLE
   )
   TABLESPACE USERS
   PCTFREE 10 INITRANS 1
   STORAGE (BUFFER_POOL DEFAULT )
   NOCOMPRESS NOPARALLEL
   PARTITION BY REFERENCE (order_details_fk1);
   
   ```

   

4. 声明变量

   ```sql
   declare
     dt date;
     m number(8,2);
     V_EMPLOYEE_ID NUMBER(6);
     v_order_id number(10);
     v_name varchar2(100);
     v_tel varchar2(100);
     v number(10,2);
     v_order_detail_id number;
   begin
   ```

   

5. 插入数据  orders（一万行数据），order_details（三万行数据）

   ![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test3/insert_data.jpg)

   代码如下:

   ``` sql
   v_order_detail_id:=1;
     delete from order_details;
     delete from orders;
     for i in 1..10000
     loop
       if i mod 6 =0 then
         dt:=to_date('2015-3-2','yyyy-mm-dd')+(i mod 60); --PARTITION_2015
       elsif i mod 6 =1 then
         dt:=to_date('2016-3-2','yyyy-mm-dd')+(i mod 60); --PARTITION_2016
       elsif i mod 6 =2 then
         dt:=to_date('2017-3-2','yyyy-mm-dd')+(i mod 60); --PARTITION_2017
       elsif i mod 6 =3 then
         dt:=to_date('2018-3-2','yyyy-mm-dd')+(i mod 60); --PARTITION_2018
       elsif i mod 6 =4 then
         dt:=to_date('2019-3-2','yyyy-mm-dd')+(i mod 60); --PARTITION_2019
       else
         dt:=to_date('2020-3-2','yyyy-mm-dd')+(i mod 60); --PARTITION_2020
       end if;
       V_EMPLOYEE_ID:=CASE I MOD 6 WHEN 0 THEN 11 WHEN 1 THEN 111 WHEN 2 THEN 112
                                   WHEN 3 THEN 12 WHEN 4 THEN 121 ELSE 122 END;
       --插入订单
       v_order_id:=i;
       v_name := 'aa'|| 'aa';
       v_name := 'zhang' || i;
       v_tel := '139888883' || i;
       insert /*+append*/ into ORDERS (ORDER_ID,CUSTOMER_NAME,CUSTOMER_TEL,ORDER_DATE,EMPLOYEE_ID,DISCOUNT)
         values (v_order_id,v_name,v_tel,dt,V_EMPLOYEE_ID,dbms_random.value(100,0));
       --插入订单y一个订单包括3个产品
       v:=dbms_random.value(10000,4000);
       v_name:='computer'|| (i mod 3 + 1);
       insert /*+append*/ into ORDER_DETAILS(ID,ORDER_ID,PRODUCT_NAME,PRODUCT_NUM,PRODUCT_PRICE)
         values (v_order_detail_id,v_order_id,v_name,2,v);
       v:=dbms_random.value(1000,50);
       v_name:='paper'|| (i mod 3 + 1);
       v_order_detail_id:=v_order_detail_id+1;
       insert /*+append*/ into ORDER_DETAILS(ID,ORDER_ID,PRODUCT_NAME,PRODUCT_NUM,PRODUCT_PRICE)
         values (v_order_detail_id,v_order_id,v_name,3,v);
       v:=dbms_random.value(9000,2000);
       v_name:='phone'|| (i mod 3 + 1);
   
       v_order_detail_id:=v_order_detail_id+1;
       insert /*+append*/ into ORDER_DETAILS(ID,ORDER_ID,PRODUCT_NAME,PRODUCT_NUM,PRODUCT_PRICE)
         values (v_order_detail_id,v_order_id,v_name,1,v);
       --在触发器关闭的情况下，需要手工计算每个订单的应收金额：
       select sum(PRODUCT_NUM*PRODUCT_PRICE) into m from ORDER_DETAILS where ORDER_ID=v_order_id;
       if m is null then
        m:=0;
       end if;
       UPDATE ORDERS SET TRADE_RECEIVABLE = m - discount WHERE ORDER_ID=v_order_id;
       IF I MOD 1000 =0 THEN
         commit; --每次提交会加快插入数据的速度
       END IF;
     end loop;
   end;
   ```

6. 检验数据是否插入成功

   ![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test3/check.jpg)

### 5. 查看磁盘占用情况

1. 情况一

![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test3/space_use_info.jpg)

2. 情况二

![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test3/space_use_info2.png)

### 6. 执行计划分析

1. 以sys用户登录WEI

   ![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test3/login.jpg)

2. 执行sql

   ```sql
   set autotrace on
   
   select * from your_user.orders where order_date
   between to_date('2017-1-1','yyyy-mm-dd') and to_date('2018-6-1','yyyy-mm-dd');
   
   select a.ORDER_ID,a.CUSTOMER_NAME,
   b.product_name,b.product_num,b.product_price
   from your_user.orders a,your_user.order_details b where
   a.ORDER_ID=b.order_id and
   a.order_date between to_date('2017-1-1','yyyy-mm-dd') and to_date('2018-6-1','yyyy-mm-dd');
   ```

   结果如下:

   ![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test3/result.jpg)

查询结果一:

![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test3/result_1.jpg)

查询结果二:

![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test3/result_2.jpg)





### 7. 实验总结

1. 通过本次实验，让我对对以前就比较迷惑的数据库、实例、表空间、用户、角色之间的关系豁然开朗：

- 数据库是物理数据、内存、操作系统进程的组合体
- 实例是用来访问和使用数据库的一块进程，它只存在于内存中
- Oracle数据库是通过表空间来存储物理表的，一个数据库实例可以有N个表空间，一个表空间下可以有N张表

2. 本次实验让我对创建表空间的语法掌握的更加熟练了，同时对表外键链接，表的自动化分区策略也有了深刻的了解，对各种分区方式的使用场景有了掌握
3. 本次实验让我收获很大，对以后Oracle的学习也更加的充满激情了





