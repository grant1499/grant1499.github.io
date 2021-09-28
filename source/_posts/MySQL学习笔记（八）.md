---
title: MySQL学习笔记（八）
tags:
  - SQL
categories:
  - - SQL
mathjax: false
copyright: true
abbrlink: 9e8c515
date: 2021-09-24 09:46:37
---

## 1.索引原则

<!--more-->

- 索引不是越多越好
- 不要对经常变动的数据加索引
- 小数据量的表建议不要加索引
- 索引一般应加在查找条件的字段

索引就是用空间换时间。

拓展内容： 索引的数据结构

MySQL索引背后的数据结构与算法原理： https://blog.csdn.net/wufuhuai/article/details/79631466。

```mysql
-- 我们可以在创建上述索引的时候，为其指定索引类型，分两类
hash类型的索引：查询单条快，范围查询慢
btree类型的索引：b+树，层数越多，数据量指数级增长（我们就用它，因为innodb默认支持它）

-- 不同的存储引擎支持的索引类型也不一样
InnoDB 支持事务，支持行级别锁定，支持 B-tree、Full-text 等索引，不支持 Hash 索引；
MyISAM 不支持事务，支持表级别锁定，支持 B-tree、Full-text 等索引，不支持 Hash 索引；
Memory 不支持事务，支持表级别锁定，支持 B-tree、Hash 等索引，不支持 Full-text 索引；
NDB 支持事务，支持行级别锁定，支持 Hash 索引，不支持 B-tree、Full-text 等索引；
Archive 不支持事务，支持表级别锁定，不支持 B-tree、Hash、Full-text 等索引；
```

两种MySQL的学习路线：

1. 业务级别学习（会用，本教程）
2. 运维级别学习（掌握数据库底层原理）

## 2.权限管理和备份

### 2.1 用户权限管理

先来看SQL yog的可视化管理。

找到窗口上方的人像并点击，进入用户管理界面，就可以添加和删除用户，添加时需要给权限。

用户表：`mysql.user`。mysql数据库下的user表。

本质：对这张用户表进行增删改查。

再来学习相应的sql语句。

```mysql
/* 用户和权限管理 */ ------------------
用户信息表：mysql.user

-- 刷新权限
FLUSH PRIVILEGES

-- 增加用户 CREATE USER kuangshen IDENTIFIED BY '123456'
CREATE USER 用户名 IDENTIFIED BY [PASSWORD] 密码(字符串)
  - 必须拥有mysql数据库的全局CREATE USER权限，或拥有INSERT权限。
  - 只能创建用户，不能赋予权限。
  - 用户名，注意引号：如 'user_name'@'192.168.1.1' 或 'user_name'@'localhost'
  - 密码也需引号，纯数字密码也要加引号
  - 要在纯文本中指定密码，需忽略PASSWORD关键词。要把密码指定为由PASSWORD()函数返回的混编值，需包含关键字PASSWORD

-- 重命名用户 RENAME USER kuangshen TO kuangshen2
RENAME USER old_user TO new_user
RENAME USER 'hei'@'localhost' TO 'xiaohei'@'localhost';
-- 设置密码
SET PASSWORD = PASSWORD('密码')    -- 为当前用户设置密码
SET PASSWORD FOR 用户名 = PASSWORD('密码')    -- 为指定用户设置密码
ALTER USER "hei"@"localhost" IDENTIFIED BY "123456"; -- 新版本命令

-- 删除用户 DROP USER kuangshen2
DROP USER 用户名

-- 分配权限/添加用户
GRANT 权限列表 ON 表名 TO 用户名 [IDENTIFIED BY [PASSWORD] 'password']
  - all privileges 表示所有权限
  - *.* 表示所有库的所有表
  - 库名.表名 表示某库下面的某表

-- 查看权限   SHOW GRANTS FOR root@localhost;
SHOW GRANTS FOR 用户名
   -- 查看当前用户权限
  SHOW GRANTS; 或 SHOW GRANTS FOR CURRENT_USER; 或 SHOW GRANTS FOR CURRENT_USER();

-- 撤消权限
REVOKE 权限列表 ON 表名 FROM 用户名
REVOKE ALL PRIVILEGES, GRANT OPTION FROM 用户名    -- 撤销所有权限 
```

### 2.2 MySQL备份

保证重要的数据不丢失

- 直接拷贝物理文件
- 可视化工具，手动导出
- 命令行
- mysqldump

```shell
# 进入cmd命令行
-- 导出
# mysqldump -h主机(不写默认localhost) -u用户名 -p密码 数据库名 表名 >物理磁盘位置
1. 导出一张表 -- mysqldump -uroot -p123456 school student >D:/a.sql
　　mysqldump -u用户名 -p密码 库名 表名 > 文件名(D:/a.sql)
2. 导出多张表 -- mysqldump -uroot -p123456 school student result >D:/a.sql
　　mysqldump -u用户名 -p密码 库名 表1 表2 表3 > 文件名(D:/a.sql)
3. 导出所有表 -- mysqldump -uroot -p123456 school >D:/a.sql
　　mysqldump -u用户名 -p密码 库名 > 文件名(D:/a.sql)
4. 导出一个库 -- mysqldump -uroot -p123456 -B school >D:/a.sql
　　mysqldump -u用户名 -p密码 -B 库名 > 文件名(D:/a.sql)

可以-w携带备份条件

-- 导入
1. 在登录mysql的情况下：-- source D:/a.sql
　　source 备份文件
2. 在不登录的情况下
　　mysql -u用户名 -p密码 库名 < 备份文件
```

## 3.数据库设计三大范式（面试常考）

设计表的依据。按照这三个范式设计的表不会出现数据冗余。

```mysql
三范式都是哪些？

	第一范式(1st NF)：任何一张表都应该有主键，并且每一个字段原子性不可再分。

	第二范式(2nd NF)：建立在第一范式的基础之上，所有非主键字段完全依赖主键，不能产生部分依赖。
		多对多？三张表，关系表两个外键。
		t_student学生表
		sno(pk)		sname
		-------------------
		1				张三
		2				李四
		3				王五

		t_teacher 讲师表
		tno(pk)		tname
		---------------------
		1				王老师
		2				张老师
		3				李老师

		t_student_teacher_relation 学生讲师关系表
		id(pk)		sno(fk)		tno(fk)
		----------------------------------
		1				1				3
		2				1				1
		3				2				2
		4				2				3
		5				3				1
		6				3				3
	
	第三范式(3rd NF)：建立在第二范式的基础之上，所有非主键字段直接依赖主键，不能产生传递依赖。
		一对多？两张表，多的表加外键。
		班级t_class
		cno(pk)			cname
		--------------------------
		1					班级1
		2					班级2

		学生t_student
		sno(pk)			sname				classno(fk)
		---------------------------------------------
		101				张1				1
		102				张2				1
		103				张3				2
		104				张4				2
		105				张5				2
	
	提醒：在实际的开发中，以满足客户的需求为主，有的时候会拿冗余换执行速度。（多表-->单表）
	阿里开发规范：关联查询的表不得超过三张表，要考虑性能
	总结：要综合考虑规范性和性能的问题。
```

数据库三大范式通俗解释（带例子）： https://www.cnblogs.com/flashsun/p/9254880.html。