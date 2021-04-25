

## 一、准备工作

- sys用户登录WEI数据库

![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test4/%E7%99%BB%E5%BD%95sys.png)

- 查看所有表空间

![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test4/查看表空间.png)

- 授予用户**wangwei_user**的相关权限

![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test4/授权.png)

- 授予用户**wangwei_user**创建视图的权限

![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test4/创建表空间权限.png)

## 二、建表

1. **wangwei_user**用户登录WEI数据库

   ![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test4/登录2.png)

2. 删除表和序列，防止以前可能存在相同的表影响本次实验

   ![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test4/删除表和序列.png)

3. 创建Departments表

   ![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test4/创建departments表.png)

4. 创建Employees表

   ![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test4/创建Employees表.png)

5. 为Employees表创建索引

   ![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test4/创建索引.png)

6. 为Employees表增加相关约束

   ![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test4/增加约束.png)

7. 创建PRODUCTS表

   ![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test4/创建PRODUCTS表.png)

8. 为PRODUCTS增加约束

   ![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test4/为PRODUCTS增加约束.png)

9. 创建临时表

   ![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test4/创建临时表.png)

10. 创建ORDERS表

    ![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test4/创建ORDERS表.png)

11. 创建索引

    ![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test4/创建索引2.png)

12. 为ORDERS表增加相关约束

    ![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test4/增加约束2.png)

13. 创建ORDER_DETAILS表

    ![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test4/创建ORDER_DETAILS表.png)

14. 创建索引并增加约束

    ![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test4/创建索引并增加约束.png)

15. 创建3个触发器

    ![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test4/创建3个触发器.png)

16. 创建序列

    ![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test4/创建序列.png)

17. 创建视图

    ![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test4/创建视图.png)

## 三、插入数据

1. 插入DEPARTMENTS，EMPLOYEES数据

   ![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test4/插入DEPARTMENTS，EMPLOYEES数据.png)

2. 插入products数据

   ![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test4/插入products数据.png)

3. 批量插入1万订单数据

   ![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test4/批量插入1万订单数据.png)

4. 启用触发器

   ![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test4/启用触发器.png)

5. 动态增加一个PARTITION_BEFORE_2018分区并修改索引

   ![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test4/动态增加分区.png)

## 四、查询

1. 查询某个员工的信息

2. 递归查询某个员工及其所有下属，子下属员工。

   ![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test4/查询2.png)

3. 查询订单表，并且包括订单的订单应收货款: Trade_Receivable= sum(订单详表.ProductNum*订单详单表.ProductPrice)- Discount。

4. 查询订单详表，要求显示订单的客户名称和客户电话，产品类型用汉字描述。

5. 查询出所有空订单，即没有订单详单的订单。

6. 查询部门表，同时显示部门的负责人姓名。

7. 查询部门表，统计每个部门的销售总金额。