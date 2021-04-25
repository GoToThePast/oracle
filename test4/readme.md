

## 一、准备工作

- sys用户登录WEI数据库
- 查看所有表空间
- 授予用户**wangwei_user**的相关权限
- 授予用户**wangwei_user**创建视图的权限

## 二、建表

1. 删除表和序列，防止以前可能存在相同的表影响本次实验
2. 创建Departments表
3. 创建Employees表
4. 为Employees表创建索引
5. 为Employees表增加相关约束
6. 创建PRODUCTS表
7. 为PRODUCTS增加约束
8. 创建临时表
9. 创建ORDERS表
10. 创建索引
11. 为ORDERS表增加相关约束
12. 创建ORDER_DETAILS表
13. 创建索引并增加约束
14. 创建3个触发器
15. 创建序列
16. 创建视图

## 三、插入数据

1. 插入DEPARTMENTS，EMPLOYEES数据
2. 插入products数据
3. 批量插入1万订单数据
4. 启用触发器
5. 动态增加一个PARTITION_BEFORE_2018分区并修改索引

## 四、查询

1. 查询某个员工的信息

2. 递归查询某个员工及其所有下属，子下属员工。

   

3. 查询订单表，并且包括订单的订单应收货款: Trade_Receivable= sum(订单详表.ProductNum*订单详单表.ProductPrice)- Discount。

4. 查询订单详表，要求显示订单的客户名称和客户电话，产品类型用汉字描述。

5. 查询出所有空订单，即没有订单详单的订单。

6. 查询部门表，同时显示部门的负责人姓名。

7. 查询部门表，统计每个部门的销售总金额。