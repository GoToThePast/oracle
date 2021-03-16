实验2：用户及权限管理

## 实验目的

掌握用户管理、角色管理、权根维护与分配的能力，掌握用户之间共享对象的操作技能。

## 实验内容

Oracle有一个开发者角色resource，可以创建表、过程、触发器等对象，但是不能创建视图。本训练要求：

- 在pdborcl插接式数据中创建一个新的本地角色con_res_view，该角色包含connect和resource角色，同时也包含CREATE VIEW权限，这样任何拥有con_res_view的用户就同时拥有这三种权限。
- 创建角色之后，再创建用户new_user，给用户分配表空间，设置限额为50M，授予con_res_view角色。
- 最后测试：用新用户new_user连接数据库、创建表，插入数据，创建视图，查询表和视图的数据。

# 实验步骤

## 创建用户

**创建的角色为wangwei_res_view,创建的用户为wangwei_user**

![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test2/1.jpg)

- 第1步：以system登录到pdborcl

  ```
  sqlplus system/123@202.115.82.8/pdborcl
  ```

- 第2步：创建角色wangwei_res_view

  ```sql
  CREATE ROLE wangwei_res_view;
  ```

- 第3步：赋予权限给wangwei_res_view

  ```sql
  GRANT connect,resource,CREATE VIEW TO wangwei_res_view;
  ```

- 第4步：创建用户wangwei_user

  ```sql
  CREATE USER new_user IDENTIFIED BY 123 DEFAULT TABLESPACE users TEMPORARY 
  ```

- 第5步：授权wangwei_user用户访问users表空间，空间限额是50M。

  ```sql
  ALTER USER wangwei_user QUOTA 50M ON users;
  ```

- 第6步：将授权角色wangwei_res_view给用户wangwei_user

  ```sql
  GRANT wangwei_res_view TO wangwei_user;
  ```




## 创建表

![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test2/2.jpg)

- 第1步：以wangwei_user登录到pdborcl

  ```
  sqlplus wangwei_user/123@202.115.82.8/pdborcl
  ```

- 第2步：显示当前用户

  ```sql
  show user;
  ```

- 第3步：创建表mytable

  ```sql
  CREATE TABLE mytable (id number,name varchar(50));
  ```

- 第4步：插入数据

  ```sql
  INSERT INTO mytable(id,name)VALUES(1,'wang');
  INSERT INTO mytable(id,name)VALUES (2,'wwei');
  ```

- 第5步：创建视图并测试

  ```sql
  CREATE VIEW myview AS SELECT name FROM mytable;
  SELECT * FROM myview;
  ```

- 第6步：将myview的SELECT对象权限授予hr用户。

  ```sql
  GRANT SELECT ON myview TO hr;
  ```


## 测试

![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test2/3.jpg)

- 第1步：以hr登录到pdborcl

  ```
  sqlplus hr/123@202.115.82.8/pdborcl
  ```

- 第2步：查询wangwei_user授予它的视图myview

  ```sql
  SELECT * FROM wangwei_user.myview;
  ```

- 第3步：测试其他用户之间的只读共享和读写共享共享。

  1. 测试用户new_zrf的select权限
  
     ![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test2/%E6%B5%8B%E8%AF%95%E5%85%B6%E4%BB%96%E7%94%A8%E6%88%B7select.jpg)
  
  2. 测试用户new_zrf的insert权限
  
     ![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test2/%E6%B5%8B%E8%AF%95%E5%85%B6%E4%BB%96%E7%94%A8%E6%88%B7_insert1.jpg)
  
  3. 用户new_zrf授权后再次测试的insert权限
  
     ![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test2/%E6%B5%8B%E8%AF%95%E5%85%B6%E4%BB%96%E7%94%A8%E6%88%B7_insert2.jpg)

## 查看数据库的使用情况

查看表空间的数据库文件，以及每个文件的磁盘占用情况。

![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test2/%E6%9F%A5%E7%9C%8B%E8%A1%A8%E7%A9%BA%E9%97%B41.jpg)

![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test2/%E6%9F%A5%E7%9C%8B%E8%A1%A8%E7%A9%BA%E9%97%B42.jpg)

![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test2/%E6%9F%A5%E7%9C%8B%E8%A1%A8%E7%A9%BA%E9%97%B43.jpg)

![](https://raw.githubusercontent.com/GoToThePast/oracle/master/test2/%E6%9F%A5%E7%9C%8B%E8%A1%A8%E7%A9%BA%E9%97%B44.jpg)