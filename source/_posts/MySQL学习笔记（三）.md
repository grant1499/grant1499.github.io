---
title: MySQL学习笔记（三）
tags:
  - SQL
categories:
  - - SQL
mathjax: false
copyright: true
abbrlink: 913e0d32
date: 2021-05-04 16:56:01
---

## 1.创建数据库表

<!--more-->

```sql
-- 注意：表的名称和字段尽量用``包起来
-- 字符串使用单引号括起来！ 'xxx'（双引号也行）
-- 所有语句后面加,最后一个不用
-- primary key，主键一般一张表一个
CREATE TABLE IF NOT EXISTS `student` (
`id` INT(4) NOT NULL AUTO_INCREMENT COMMENT '学号',
`name` VARCHAR(30) NOT NULL DEFAULT '匿名' COMMENT '姓名',
`pwd` VARCHAR(15) NOT NULL DEFAULT '12345' COMMENT '密码',
`gender` VARCHAR(3) NOT NULL DEFAULT '女' COMMENT '性别',
`birthday` DATETIME DEFAULT NULL COMMENT '出生日期',
`address` VARCHAR(30) DEFAULT NULL COMMENT '家庭住址',
`email` VARCHAR(30) DEFAULT NULL COMMENT '邮箱',
PRIMARY KEY(`id`)
)ENGINE = INNODB DEFAULT CHARSET = utf8
```

## 2.三个常用命令

```sql
-- 查看数据库和表的创建语句
SHOW CREATE DATABASE school
-- CREATE DATABASE `school` /*!40100 DEFAULT CHARACTER SET utf8 */
SHOW CREATE TABLE `student`
-- CREATE TABLE IF NOT EXISTS `student`(XXX)XXX
DESC student
-- 查看表的结构
```

![image-20210504173551477](MySQL学习笔记（三）/image-20210504173551477.png)

## 3.数据表的类型

关于数据库引擎：（了解）

默认使用INNODB，

早些年使用MYISAM。

两者的区别：

![image-20210504173012750](MySQL学习笔记（三）/image-20210504173012750.png)

补充：INNODB 5.6.4后支持了全文索引。

所有数据库文件都存在data目录下，一个文件夹对应一个数据库。

数据库本质还是用文件存储。

![image-20210504173513323](MySQL学习笔记（三）/image-20210504173513323.png)

了解内容。

charset设置数据库表的字符集编码。

不设置的话，默认的不支持中文。

修改编码：

1. 创建数据库的时候就加上`charset=utf8`
2. 在my.ini配置默认编码 `character-set-server=utf8`

## 4.修改和删除表和表字段

修改表和表字段：

```sql
-- 修改表名
ALTER TABLE teacher RENAME AS teacher1;

-- 增加字段
ALTER TABLE teacher ADD age INT(11);

-- 修改表的字段 1 修改约束
ALTER TABLE teacher MODIFY age VARCHAR(11); -- 修改字段约束
-- 修改表的字段 2 重命名
ALTER TABLE teacher CHANGE age age1 INT(1); -- 字段重命名
！！ 注意两者的区别
```

修改之后在sqlyog左侧栏刷新一下，看到效果。

删除表和表字段：

```sql
-- 删除表的一个字段
ALTER TABLE teacher DROP age1;

-- 删除表
DROP TABLE IF EXISTS teacher ;
```

所有的创建和删除尽量加上IF判断、以免报错。

总结：

```sql
修改表名 :ALTER TABLE 旧表名 RENAME AS 新表名
添加字段 : ALTER TABLE 表名 ADD字段名 列属性[属性]
修改字段 :
ALTER TABLE 表名 MODIFY 字段名 列类型[属性] -- 修改字段约束
ALTER TABLE 表名 CHANGE 旧字段名 新字段名 列属性[属性] -- 字段重命名
删除字段 :  ALTER TABLE 表名 DROP 字段名
```

## 5.MySQL的数据管理

### 5.1 外键

物理外键、了解即可。

创建方式一：创建表的时候增加约束 ，麻烦。

删除有外键关系的表：必须先删除引用别人的表，再删除被引用的表。

创建外键方式二 : 创建表的时候没有外键关系。

```sql
-- 创建外键方式二 : 创建子表完毕后,修改子表添加外键
ALTER TABLE `student`
ADD CONSTRAINT `FK_gradeid` FOREIGN KEY (`gradeid`) REFERENCES `grade` (`gradeid`);
```

以上操作都是物理外键，数据库级别的外键，避免数据库过多造成困恼。

不建议使用。

**最佳实践**

1. 数据库就是单纯的表，只存数据，行（数据），列（字段）
2. 想使用多张表的数据，想使用外键，（程序级别实现）

阿里Java规范：强制不得使用外键与级联，一切外键概念必须在应用层解决。

### 5.2 DML：数据库操作语言（全部记住）



数据库的意义：数据的存储和管理。

DML：增删改查必须都记住。

- insert
- update
- delete

### 5.3 添加

```sql
INSERT INTO 表名(字段名1,字段名2,字段名3,...) -- 插入多个字段，也可以是单个字段
VALUES('值1','值2','值3',...)

INSERT INTO 表名(字段名1,字段名2,字段名3,...) -- 插入多个行
VALUES('值1','值2','值3',...),('值1','值2','值3',...),('值1','值2','值3',...),(...)
```

由于主键自增，往往省略主键的添加。

一般写插入语句，数据与字段一定要一一对应。

字段是可以省略的（一个都不写），但是后面的所有值一定要一一对应，不能少 。不建议这样写。

插入多行：

```sql
insert into `grade`(`gradename`) values
('xxx1'),
('xxx2');
--
INSERT INTO student(`name`,`gender`,`birthday`,`email`)
VALUES('tom','男','2001-01-23','1224@163.com'),
('Lee','女','2002-07-16','3253@163.com'),
('Jack','男','2001-05-28','4352@163.com');
```

### 5.4 修改

```sql
UPDATE 表名 SET column_name=value [,column_name2=value2,...] [WHERE condition];
-- 修改年级信息
UPDATE grade SET gradename = '高中' WHERE gradeid = 1;

-- 修改多个属性，用逗号隔开
UPDATE `student` SET `name`='陈', `email`='chen@163.com' WHERE id = 1;
```

`value`可以是一个值（常量），也可以是一个变量（比如时间等）。

如果不加上`WHERE`子句，会默认修改所有行的相应内容。（最好不要这样做）

如果把所有行的内容都修改了，建议 删库跑路~

条件：where子句 运算符

| 操作符          | 含义                     | 范围  | 结果          |
| --------------- | ------------------------ | ----- | ------------- |
| =               | 等于                     |       | 等于执行sql   |
| <>或!=          | 不等于                   |       | 不等于执行sql |
| > >= < <=       | 大于大于等于小于小于等于 |       |               |
| between … and … | 在某个范围内             | [2,5] |               |
| and             | 我和你&&                 |       |               |
| or              | 我或你\|\|               |       |               |

MySQL完整运算符： https://www.runoob.com/mysql/mysql-operator.html。