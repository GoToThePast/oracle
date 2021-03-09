实验内容

#### 1. 对Oracle12c中的HR人力资源管理系统中的表进行查询与分析。

​		都是查询部门人数及其平均工资

1. 查询语句一：
   1. 直接使用 in 标签限制查询部门为（“IT”, "Sales"）
   2. 通过部门名称分组
2. 查询二
   1. 直接按部门名称分组
   2. having 语句判断以实现部门筛选（“IT”, "Sales"）

#### 2. 首先运行和分析教材中的样例：本训练任务目的是查询两个部门('IT'和'Sales')的部门总人数和平均工资，以下两个查询的结果是一样的。但效率不相同。

* 查询结果如下:

  ![image-20210309191301463](https://raw.githubusercontent.com/GoToThePast/oracle/main/test1/%E6%9F%A5%E8%AF%A2%E4%B8%80%E7%BB%93%E6%9E%9C.png)

  ![](https://raw.githubusercontent.com/GoToThePast/oracle/main/test1/%E6%9F%A5%E8%AF%A2%E4%BA%8C%E7%BB%93%E6%9E%9C.png)

  

* 

* 查询二效率更低，因为

* 

#### 3. 设计自己的查询语句

```sql

-- 查询总的国家 及其平均工资
SELECT
	jimpie.country_name 国家, 
	AVG( jimpie.salary ) 平均工资 
FROM
	(
        -- 查询员工表与部门表关联 后与 询LOCATIONS与country关联表 的联合表 中的工资和国家名 
	SELECT
		jiam.salary,
		breeze.country_name 
	FROM
		( SELECT * FROM HR.employees e INNER JOIN HR.departments d ON e.department_id = d.department_id ) jiam
		INNER JOIN (
            -- 查询LOCATIONS与country关联表中 国家名字和地区ID
		SELECT
			l.LOCATION_ID,
			c.COUNTRY_NAME 
		FROM
			HR.LOCATIONS l
			INNER JOIN HR.COUNTRIES c ON l.country_id = c.country_id 
		) breeze ON jiam.LOCATION_ID = breeze.LOCATION_ID 
	) jimpie 
GROUP BY
-- 设置分组为国家名字
	jimpie.country_name
```



