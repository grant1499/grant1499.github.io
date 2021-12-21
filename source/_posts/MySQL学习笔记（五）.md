---
title: MySQL学习笔记（五）
tags:
  - SQL
categories:
  - - SQL
mathjax: false
copyright: true
abbrlink: 498dd110
date: 2021-09-21 13:12:10
---

### 6.3 联表查询

在关系型数据库中，表与表之间是有联系的，所以在实际应用中，经常使用多表查询。多表查询就是同时查询两个或两个以上的表。

<!--more-->

在 MySQL 中，多表查询主要有交叉连接、内连接和外连接。

参考资料： http://c.biancheng.net/view/7463.html。

SQL JOINS：七种join（联结）方式。

![image-20210921150037470](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232137813.png)

内连接的语法格式如下：

`SELECT <字段名> FROM <表1> INNER JOIN <表2> [ON子句]`

内连接使用 **INNER JOIN** 关键字连接两张表，并使用 ON 子句来设置连接条件。

内连接可以查询两个或两个以上的表。

on子句可以用where子句代替，但不建议这样做。`INNER JOIN ... ON` 语法是官方的标准写法，而且 `WHERE` 子句在某些时候会影响查询的性能。

外连接可以分为左连接和右连接。

```mysql
-- 查询参加了考试的同学(学号，姓名，科目编号，分数)

SELECT * FROM student;
SELECT * FROM result;

-- 1. 查哪些字段，来自哪个表
-- 2. 确定使用的联结查询
-- 3. 交集的确定
-- 4. 判断的条件， 表1 xx = 表2 xx

-- inner join
SELECT s.`studentno`, s.`studentname`, r.`subjectno`, r.`studentresult`
FROM student AS s
INNER JOIN result AS r
ON  s.`studentno` = r.`studentno`;


-- right join
SELECT s.`studentno`, s.`studentname`, r.`subjectno`, r.`studentresult`
FROM student AS s
RIGHT JOIN result AS r
ON s.`studentno` = r.`studentno`;


-- left join
SELECT s.`studentno`, s.`studentname`, r.`subjectno`, r.`studentresult`
FROM student AS s
LEFT JOIN result AS r
ON s.`studentno` = r.`studentno`;
```

FROM后面的是主表，JOIN后面的是参考表。

小结：

左连接以左表为基准，右连接以右表为基准。

|操作	|描述|
| ---- | ---- |
|Inner join	|如果表中至少有一个匹配，就返回|
|left join	|会从左表返回所有的值，即使右表没有匹配|
|right join	|会从右表返回所有的值，即使左表没有匹配|

左连接：假设表1为主表，表2为参考表。

如果“表1”的某行在“表2”中没有匹配行，那么在返回结果中，“表2”的字段值均为空值（NULL）。

```mysql
-- 查询缺考的学生
SELECT s.`studentno`, s.`studentname`, r.`subjectno`, r.`studentresult`
FROM student AS s
LEFT JOIN result AS r
ON s.`studentno` = r.`studentno`
WHERE r.`studentresult` IS NULL;
```

查询：参考考试的同学信息：学号，学生姓名，科目名，分数。

需要三表联接。

```mysql
SELECT r.`studentno`,s.`studentname`,s.`gradeid`,r.`studentresult`,sub.`subjectname`
FROM `result` AS r
RIGHT JOIN `student` AS s
ON r.`studentno` = s.`studentno`
INNER JOIN `subject` AS sub
ON sub.`subjectno` = r.`subjectno`;
```

### 6.4 自联结查询

建立测试表数据：

```mysql
CREATE TABLE `school`.`category`( `categoryid` INT(3) NOT NULL COMMENT 'id', `pid` INT(3) NOT NULL COMMENT '父id 没有父则为1', `categoryname` VARCHAR(10) NOT NULL COMMENT '种类名字', PRIMARY KEY (`categoryid`) ) ENGINE=INNODB CHARSET=utf8 COLLATE=utf8_general_ci; 

INSERT INTO `school`.`category` (`categoryid`, `pid`, `categoryname`) VALUES ('2', '1', '信息技术');
INSERT INTO `school`.`CATEGOrY` (`categoryid`, `pid`, `categoryname`) VALUES ('3', '1', '软件开发');
INSERT INTO `school`.`category` (`categoryid`, `PId`, `categoryname`) VALUES ('5', '1', '美术设计');
INSERT INTO `School`.`category` (`categoryid`, `pid`, `categorynamE`) VALUES ('4', '3', '数据库'); 
INSERT INTO `school`.`category` (`CATEgoryid`, `pid`, `categoryname`) VALUES ('8', '2', '办公信息');
INSERT INTO `school`.`category` (`categoryid`, `pid`, `CAtegoryname`) VALUES ('6', '3', 'web开发'); 
INSERT INTO `SCHool`.`category` (`categoryid`, `pid`, `categoryname`) VALUES ('7', '5', 'ps技术');
```

上面的测试表实际上是两张表糅合成一张表。

`pid`为1的是最大的分类（父类）：

子类的`pid`对应父类的`categoryid`，确定每个子类属于哪个父类。

![image-20210922100625454](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232137073.png)

整理父类和子类的关系可以得到下图：

![image-20210922100823667](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232137146.png)

下面用`mysql`语句来查询父类和子类的关系：

```mysql
-- 查询父子信息：把一张表拆成两张表
SELECT a.`categoryname` AS '父类', b.`categoryname` AS '子类'
FROM `category` AS a,`category` AS b
WHERE a.`categoryid` = b.`pid`;
```

```mysql
-- 联表查询小练习
-- 方式1
SELECT g.`gradeid`,g.`gradename`,s.`studentname`
FROM `grade` AS g
INNER JOIN `student` AS s
ON g.`gradeid` = s.`gradeid`;
-- 方式2
SELECT g.`gradeid`,g.`gradename`,s.`studentname`
FROM `grade` AS g,`student` AS s
WHERE g.`gradeid` = s.`gradeid`;
-- 两种方式的区别
INNER JOIN 是一样的。 如果换成 left join 或right join ，是有区别的； A left join B on 后面的语句，是对 表 B 数据的过滤 ，而 where 是对结果集（笛卡尔积）的过滤。
```

### 6.5 分页（limit）和排序（order by）

排序：升序asc，降序desc。

```mysql
SELECT s.`studentno`,s.`studentname`,sb.`subjectname`,r.`studentresult`
FROM student AS s
RIGHT JOIN result AS r
ON s.`studentno` = r.`studentno`
INNER JOIN `subject` AS sb
ON r.`subjectno` = sb.`subjectno`
ORDER BY studentresult DESC; -- 按成绩降序排列，升序ASC
```

分页limit：

为啥要分页呢？缓解数据库压力。

瀑布流：没有分页，可以一直往下拉。

语法：`limit 当前页起始值,页面大小`。

```mysql
SELECT s.`studentno`,s.`studentname`,sb.`subjectname`,r.`studentresult`
FROM student AS s
RIGHT JOIN result AS r
ON s.`studentno` = r.`studentno`
INNER JOIN `subject` AS sb
ON r.`subjectno` = sb.`subjectno`
ORDER BY studentresult DESC
LIMIT 0,3;
```

```mysql
-- 第一页
limit 0,5 -- 行数从0开始！！！
-- 第二页
limit 5,5
-- 第三页 
limit 10,5

-- 第n页
limit (n-1)*5,5

limit (n-1)*pagesize,pagesize 
-- n代表当前页，pagesize代表页面大小
-- (n-1)*pagesize 起始值
-- 数据总数/页面大小 = 总页数，上取整
```

小练习：

```mysql
-- 查询 JAVA第一学年 课程成绩前10名并且分数大于80的学生信息(学号,姓名,课程名,分数)
SELECT s.studentno,studentname,subjectname,StudentResult
FROM student s
INNER JOIN result r
ON r.studentno = s.studentno
INNER JOIN `subject` sub
ON r.subjectno = sub.subjectno
WHERE subjectname='JAVA第一学年' AND StudentResult >= 80
ORDER BY StudentResult DESC
LIMIT 0,10
```

