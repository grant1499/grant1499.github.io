---
title: SQL语句大全
date: 2021-04-16 23:32:10
tags: [SQL]
categories: 
	- [SQL]
mathjax: true
copyright: true
---

转载：https://mp.weixin.qq.com/s/EbdgRbGBgzzRa2pET-6qRA

## 一、基础

### 1、说明：创建数据库

CREATE DATABASE database-name

### 2、说明：删除数据库

drop database dbname

<!--more-->

### 3、说明：备份sql server

--- 创建 备份数据的 device
USE master
EXEC sp_addumpdevice 'disk', 'testBack', 'c:mssql7backupMyNwind_1.dat'
--- 开始 备份
BACKUP DATABASE pubs TO testBack 

### 4、说明：创建新表

create table tabname(col1 type1 [not null] [primary key],col2 type2 [not null],..)

根据已有的表创建新表： 
A：create table tab_new like tab_old (使用旧表创建新表)
B：create table tab_new as select col1,col2… from tab_old definition only

### 5、说明：删除新表

drop table tabname 

### 6、说明：增加一个列

Alter table tabname add column col type注：列增加后将不能删除。DB2中列加上后数据类型也不能改变，唯一能改变的是增加varchar类型的长度。

### 7、说明：添加主键： Alter table tabname add primary key(col) 

说明：删除主键： Alter table tabname drop primary key(col) 

### 8、说明：创建索引：create [unique] index idxname on tabname(col….) 

删除索引：drop index idxname
注：索引是不可更改的，想更改必须删除重新建。

### 9、说明：创建视图：create view viewname as select statement 

删除视图：drop view viewname

待更新。。。