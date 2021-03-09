## 实验内容

#### 1. 对Oracle12c中的HR人力资源管理系统中的表进行查询与分析。

​		都是查询部门人数及其平均工资

1. 查询一：
   1. 直接使用 in 标签限制查询部门为（“IT”, "Sales"）
   2. 通过部门名称分组
2. 查询二
   1. 直接按部门名称分组
   2. having 语句判断以实现部门筛选（“IT”, "Sales"）

#### 2. 首先运行和分析教材中的样例：本训练任务目的是查询两个部门('IT'和'Sales')的部门总人数和平均工资，以下两个查询的结果是一样的。但效率不相同。

* 查询结果如下:

  * ​	查询一

  ![image-20210309191301463](https://raw.githubusercontent.com/GoToThePast/oracle/master/test1/%E6%9F%A5%E8%AF%A2%E4%B8%80%E7%BB%93%E6%9E%9C.png)

  			* ​	查询二

  ![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test1/%E6%9F%A5%E8%AF%A2%E4%BA%8C%E7%BB%93%E6%9E%9C.png)

  

* 查询统计如下：

  * ​	查询一:

  ![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test1/%E6%9F%A5%E8%AF%A2%E4%B8%80%E7%BB%9F%E8%AE%A1.png)

  		* ​	查询二

  ![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test1/%E6%9F%A5%E8%AF%A2%E4%BA%8C%E7%BB%9F%E8%AE%A1.png)

* 查询一语句相对于查询二要更好一些，但查询一会发生全表查询

* 查询一优化建议如下:

![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test1/优化建议.png)

* 建立索引:

  ![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test1/创建索引.png)

#### 3. 用户hr默认没有统计权限，运行上述命令时要报错

* 无法收集统计信息, 请确保用户具有正确的访问权限。

* 统计信息功能要求向用户授予 v_sesstat,v_statname 和 v_session 的选择权限。

* 解决：授权

  ![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test1/%E6%8E%88%E6%9D%83.png)



#### 4.  设计自己的查询语句

```sql
-- 查询每个不同国家员工的平均工资
SELECT
	all_info.country_name 国家, 
	AVG( all_info.salary ) 平均工资 
FROM
	(
        -- 查询员工表与部门表关联 后与 询LOCATIONS与country关联表 的联合表 中的工资和国家名 
	SELECT
		employee.salary,
		country.country_name 
	FROM
		( SELECT * FROM HR.employees e INNER JOIN HR.departments d ON e.department_id = d.department_id ) employee
		INNER JOIN (
            -- 查询LOCATIONS与country关联表中 国家名字和地区ID
		SELECT
			l.LOCATION_ID,
			c.COUNTRY_NAME 
		FROM
			HR.LOCATIONS l
			INNER JOIN HR.COUNTRIES c ON l.country_id = c.country_id 
		) country ON employee.LOCATION_ID = country.LOCATION_ID 
	) all_info 
GROUP BY
-- 设置分组为国家名字
	all_info.country_name
```

> **运行结果如下**: 
>
> ![image-20210309193127133](F:\mdPic\image-20210309193127133.png)