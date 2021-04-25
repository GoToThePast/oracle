

# 实验5：PL/SQL编程

## 实验目的

 了解PL/SQL语言结构
		了解PL/SQL变量和常量的声明和使用方法
		学习条件语句的使用方法
		学习分支语句的使用方法
		学习循环语句的使用方法
		学习常用的PL/SQL函数
		学习包，过程，函数的用法。

## 〇、

本次实验所用用户为**wangwei_user**

## 一、建包

1. 用户**wangwei_user**登录WEI数据库

   ![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test5/image/%E7%99%BB%E5%BD%95.png)

2. 创建一个包(Package)，包名是MyPack

   ```sql
   create or replace PACKAGE MyPack IS
     FUNCTION Get_SaleAmount(V_DEPARTMENT_ID NUMBER) RETURN NUMBER;
     PROCEDURE Get_Employees(V_EMPLOYEE_ID NUMBER);
   END MyPack;
   ```

   ![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test5/image/createPack1.png)

3. 创建FUNCTION Get_SaleAmount和PROCEDURE GET_EMPLOYEES

   - 在MyPack中创建一个函数SaleAmount ，查询部门表，统计每个部门的销售总金额，每个部门的销售额是由该部门的员工(ORDERS.EMPLOYEE_ID)完成的销售额之和。函数SaleAmount要求输入的参数是部门号，输出部门的销售金额
   - 在MyPack中创建一个过程，在过程中使用游标，递归查询某个员工及其所有下属，子下属员工。过程的输入参数是员工号，输出员工的ID,姓名，销售总金额。信息用dbms_output包中的put或者put_line函数。输出的员工信息用左添加空格的多少表示员工的层次（LEVEL）。

   ```sql
   create or replace PACKAGE BODY MyPack IS
     FUNCTION Get_SaleAmount(V_DEPARTMENT_ID NUMBER) RETURN NUMBER
     AS
       N NUMBER(20,2); --注意，订单ORDERS.TRADE_RECEIVABLE的类型是NUMBER(8,2),汇总之后，数据要大得多。
       BEGIN
         SELECT SUM(O.TRADE_RECEIVABLE) into N  FROM ORDERS O,EMPLOYEES E
         WHERE O.EMPLOYEE_ID=E.EMPLOYEE_ID AND E.DEPARTMENT_ID =V_DEPARTMENT_ID;
         RETURN N;
       END;
   
     PROCEDURE GET_EMPLOYEES(V_EMPLOYEE_ID NUMBER)
     AS
       LEFTSPACE VARCHAR(2000);
       begin
         --通过LEVEL判断递归的级别
         LEFTSPACE:=' ';
         --使用游标
         for v in
         (SELECT LEVEL,EMPLOYEE_ID,NAME,MANAGER_ID FROM employees
         START WITH EMPLOYEE_ID = V_EMPLOYEE_ID
         CONNECT BY PRIOR EMPLOYEE_ID = MANAGER_ID)
         LOOP
           DBMS_OUTPUT.PUT_LINE(LPAD(LEFTSPACE,(V.LEVEL-1)*4,' ')||
                                V.EMPLOYEE_ID||' '||v.NAME);
         END LOOP;
       END;
   END MyPack;
   /
   ```

   ![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test5/image/createPack2.png)

   >由于订单只是按日期分区的，上述统计是全表搜索，因此统计速度会比较慢，如何提高统计的速度呢？
   >
   >答：建立索引或者使用分区查询统计

   

## 二、测试

1. 测试函数Get_SaleAmount()

   ```sql
   select count(*) from orders;
   select MyPack.Get_SaleAmount(11) AS 部门11应收金额,MyPack.Get_SaleAmount(12) AS 部门12应收金额 from dual;
   ```

   ![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test5/image/测试1.png)

   ![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test5/image/测试2.png)

   

2. 测试过程Get_Employees()

   ```sql
   set serveroutput on
   DECLARE
     V_EMPLOYEE_ID NUMBER;    
   BEGIN
     V_EMPLOYEE_ID := 1;
     MYPACK.Get_Employees (  V_EMPLOYEE_ID => V_EMPLOYEE_ID) ;  
     V_EMPLOYEE_ID := 11;
     MYPACK.Get_Employees (  V_EMPLOYEE_ID => V_EMPLOYEE_ID) ;    
   END;
   /
   ```

   ![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test5/image/测试3.png)

## 三、总结

通过通过本次实验，让我更加的了解PL/SQL语言有了更加深刻的认识了，PL /SQL是一种高级数据库程序设计语言，该语言专门用于在各种环境下对ORACLE数据库进行访问。由于该语言集成于数据库服务器中，所以PL/SQL代码可以对数据进行快速高效的处理。

本次实验后我学会了条件语句、分支语句、循环语句的使用方法和常用的PL/SQL函数，通过本次实验，让我对oracl更加的感兴趣了。