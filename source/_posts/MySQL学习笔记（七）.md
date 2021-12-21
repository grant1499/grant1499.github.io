---
title: MySQL学习笔记（七）
tags:
  - SQL
categories:
  - - SQL
mathjax: false
copyright: true
abbrlink: fe83ed56
date: 2021-09-23 13:54:03
---

## 1.执行事务

mysql 是默认开启事务自动提交的。

<!--more-->

```mysql
SET autocommit = 0 -- 关闭自动提交
SET autocommit = 1 -- 开启自动提交 默认值

-- 手动处理事务
-- 事务开启 关闭自动提交
START TRANSACTION  -- 标记一个事务的开始，事务开启
-- 这样就不用SET autocommit
INSERT xxx
INSERT xxx

-- 提交 持久化(成功)
COMMIT
-- 在START和COMMIT之间的sql语句属于一个事务

-- 回滚 回到原来的样子(失败)
ROLLBACK


-- 以下是了解内容
SAVEPOINT   -- 设置一个事务的保存点
SAVEPOINT 存档1 
ROLLBACK TO 存档1 -- 回滚到保存点
```

MYSQL 事务处理主要有两种方法：

1、用 BEGIN, ROLLBACK, COMMIT来实现

- **BEGIN** （或者**START TRANSACTION**）开始一个事务
- **ROLLBACK** 事务回滚
- **COMMIT** 事务确认

2、直接用 SET 来改变 MySQL 的自动提交模式:

- **SET AUTOCOMMIT=0** 禁止自动提交
- **SET AUTOCOMMIT=1** 开启自动提交

一个事务使用场景模拟转账：

```mysql
CREATE DATABASE `shop` CHARACTER SET utf8 COLLATE utf8_general_ci;
USE `shop`;

CREATE TABLE `account` (
`id` INT(11) NOT NULL AUTO_INCREMENT,
`name` VARCHAR(32) NOT NULL,
`cash` DECIMAL(9,2) NOT NULL,
PRIMARY KEY (`id`)
) ENGINE=INNODB DEFAULT CHARSET=utf8;

INSERT INTO account (`name`,`cash`)
VALUES('A',2000.00),('B',10000.00);

-- 模拟转账：事务
SET autocommit = 0; -- 关闭自动提交
BEGIN; -- 开启事务
UPDATE `account` SET `cash` = `cash`-500 WHERE `name` = 'A'; -- A给B转账500
UPDATE `account` SET `cash` = `cash`+500 WHERE `name` = 'B'; -- B收到A的500
COMMIT; -- 转账成功，提交事务
-- ROLLBACK; -- 转账失败，回滚，没效果，已经持久化了
SET autocommit = 1; -- 恢复默认值
-- 可以单步执行模拟整个事务流程
```

持久化之后，再回滚钱也已经转走了，回不来了。

```java
// 事务的java业务逻辑
function(){
    try{
        // 正常的业务代码
        commit;
    }
    catch(xxx){
        rollback;
    }
}
```

## 2.索引

参考博客（很全面）： https://blog.csdn.net/wangfeijiu/article/details/113409719。

索引：可以大大提高MySQL的检索速度的数据结构。

拿汉语字典的目录页（索引）打比方，我们可以按拼音、笔画、偏旁部首等排序的目录（索引）快速查找到需要的字。

索引是应用在 SQL 查询语句的条件(一般作为 WHERE 子句的条件)。

索引分单列索引和组合索引。单列索引，即一个索引只包含单个列，一个表可以有多个单列索引，但这不是组合索引。组合索引，即一个索引包含多个列。

索引分类：

1. 主键索引 (Primary Key)：**一张表**只能有**一个**主键索引，不允许重复、不允许为 NULL

    ```mysql
    ALTER TABLE TableName ADD PRIMARY KEY(column_list); 
    ```

2. 唯一索引 (Unique)：避免重复的列出现，索引列的值必须唯一，允许为NULL，唯一索引可以重复，多个列都可以表示为唯一索引

    ```mysql
    CREATE UNIQUE INDEX IndexName ON `TableName`(`字段名`(length));
    # 或者
ALTER TABLE TableName ADD UNIQUE (column_list); 
    ```

3. 常规索引 (Key/Index)：默认值。可以包含多个字段，允许数据重复，允许 NULL 值插入

    ```mysql
    CREATE INDEX IndexName ON `TableName`(`字段名`(length));
    # 或者
    ALTER TABLE TableName ADD INDEX IndexName(`字段名`(length));
    ```

4. 全文索引 (FullText)：查找的是文本中的关键词，主要用于全文检索


索引名称 IndexName 是可以省略的，省略后，索引的名称和索引列名相同，字段名后面的length也可以省略。

除了直接创建和修改添加索引，还可以在创建表时直接指定索引。

```mysql
SHOW INDEX FROM `student`; -- 查看表中的所有索引

ALTER TABLE `student` ADD INDEX (`sex`); -- 将student的sex字段设为index
ALTER TABLE `student` ADD FULLTEXT INDEX (`studentname`); -- 设置全文索引
-- explain 分析sql执行的状况
-- rows:扫描出的行数
EXPLAIN SELECT * FROM `student`;
EXPLAIN SELECT *FROM student WHERE MATCH(studentname) AGAINST('love');
```

关于`explain`的解释： https://blog.csdn.net/jiadajing267/article/details/81269067。

```mysql
/*
#方法一：创建表时
  　　CREATE TABLE 表名 (
               字段名1 数据类型 [完整性约束条件…],
               字段名2 数据类型 [完整性约束条件…],
               [UNIQUE | FULLTEXT | SPATIAL ]   INDEX | KEY
               [索引名] (字段名[(长度)] [ASC |DESC])
               );


#方法二：CREATE在已存在的表上创建索引
       CREATE [UNIQUE | FULLTEXT | SPATIAL ] INDEX 索引名
                    ON 表名 (字段名[(长度)] [ASC |DESC]) ;


#方法三：ALTER TABLE在已存在的表上创建索引
       ALTER TABLE 表名 ADD [UNIQUE | FULLTEXT | SPATIAL ] INDEX
                            索引名 (字段名[(长度)] [ASC |DESC]) ;


#删除索引：DROP INDEX 索引名 ON 表名字;
#删除主键索引: ALTER TABLE 表名 DROP PRIMARY KEY;


#显示索引信息: SHOW INDEX FROM student;
*/

/*增加全文索引*/
ALTER TABLE `school`.`student` ADD FULLTEXT INDEX `studentname` (`StudentName`);

/*EXPLAIN : 分析SQL语句执行性能*/
EXPLAIN SELECT * FROM student WHERE studentno='1000';

/*使用全文索引*/
-- 全文搜索通过 MATCH() 函数完成。
-- 搜索字符串作为 against() 的参数被给定。搜索以忽略字母大小写的方式执行。对于表中的每个记录行，MATCH() 返回一个相关性值。即，在搜索字符串与记录行在 MATCH() 列表中指定的列的文本之间的相似性尺度。
EXPLAIN SELECT *FROM student WHERE MATCH(studentname) AGAINST('love');

/*
开始之前，先说一下全文索引的版本、存储引擎、数据类型的支持情况

MySQL 5.6 以前的版本，只有 MyISAM 存储引擎支持全文索引；
MySQL 5.6 及以后的版本，MyISAM 和 InnoDB 存储引擎均支持全文索引;
只有字段的数据类型为 char、varchar、text 及其系列才可以建全文索引。
测试或使用全文索引时，要先看一下自己的 MySQL 版本、存储引擎和数据类型是否支持全文索引。
*/
```

测试索引：

```mysql
-- 建表
CREATE TABLE `app_user` (
`id` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
`name` varchar(50) DEFAULT '' COMMENT '用户昵称',
`email` varchar(50) NOT NULL COMMENT '用户邮箱',
`phone` varchar(20) DEFAULT '' COMMENT '手机号',
`gender` tinyint(4) unsigned DEFAULT '0' COMMENT '性别（0:男；1：女）',
`password` varchar(100) NOT NULL COMMENT '密码',
`age` tinyint(4) DEFAULT '0' COMMENT '年龄',
`create_time` datetime DEFAULT CURRENT_TIMESTAMP,
`update_time` timestamp NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COMMENT='app用户表';

-- 批量插入100万条数据，不需要掌握，一般用不到
DROP FUNCTION IF EXISTS mock_data;
DELIMITER $$ -- 写函数之前的标志
CREATE FUNCTION mock_data()
RETURNS INT
BEGIN
DECLARE num INT DEFAULT 1000000;
DECLARE i INT DEFAULT 0;
WHILE i < num DO
  INSERT INTO app_user(`name`, `email`, `phone`, `gender`, `password`, `age`)
   VALUES(CONCAT('用户', i), '24736743@qq.com', CONCAT('18', FLOOR(RAND()*(999999999-100000000)+100000000)),FLOOR(RAND()*2),UUID(), FLOOR(RAND()*100));
  SET i = i + 1;
END WHILE;
RETURN i;
END;
SELECT mock_data();

-- 索引创建前：
SELECT * FROM app_user WHERE name = '用户9999'; -- 耗时0.492 sec
EXPLAIN SELECT * FROM app_user WHERE name = '用户9999'; -- 无索引查找性能测试
/*id	select_type	table	partitions	type	possible_keys	key	key_len	ref	rows	filtered	Extra
1	SIMPLE	         app_user   \N	        ALL	       \N	\N	    \N	 \N	 992391	 10.00	       Using where  */

-- 索引一般命名方式：idx _ 表名 _ 字段名
-- CREATE INDEX [索引名] ON 表名(字段名);
CREATE INDEX idx_app_user_name ON `app_user`(`name`);

-- 索引创建后：
SELECT * FROM app_user WHERE name = '用户9999'; -- 耗时0.003 sec
EXPLAIN SELECT * FROM app_user WHERE NAME = '用户9999'; -- 有索引查找性能测试
/*id	select_type	table	partitions	type	     possible_keys	    key	                key_len	         ref	 rows   filtered  Extra
1	SIMPLE	      app_user       \N	         ref	    idx_app_user_name	  idx_app_user_name       203	        const	 1	  100.00   \N*/

-- 删除索引：在表名处右键 改变表 选择索引并删除 保存，也可以用sql语句删除
```

结论：索引在小数据量时优势不明显，在大数据量时用处很大。