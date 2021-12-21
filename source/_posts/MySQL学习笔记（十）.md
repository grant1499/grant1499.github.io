---
title: MySQL学习笔记（十）
tags:
  - SQL
categories:
  - - SQL
  - - Java
mathjax: false
copyright: true
abbrlink: f617c18e
date: 2021-09-25 15:26:10
---

## 1.SQL注入的问题

sql存在漏洞，会被攻击导致数据泄露

比如登录业务中，需要查询账号密码所对应的用户（比对），可能用到如下sql语句：

<!--more-->

```mysql
select * from users where name='name' and password ='password'
```

其中name和password两个变量都是用户所传入的数据

如果用户构造合适的输入，比如：

```java
String name=" ' or  1=1 -- ";
String password ="12412r1";// password在此例中的值不重要
```

那么如上sql语句拼接成了：

```mysql
select * from users where name=' ' or  1=1 -- ' and password ='12412r1'
-- 显然这段语句可以拿到所有用户的信息
```

## 2.PreparedStatement对象

`Statement`对象是不安全的，存在SQL注入问题。

`PreparedStatement`对象可以防止SQL注入问题，而且效率更高。

`PreparedStatement`对象是对`Statement`对象的继承。

```java
package com.jdbc.java.demo;
import com.jdbc.java.demo.utils.Jdbcutil;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.util.Date;

public class JDBCDemo3 {
    public static void main(String[] args) {
        Connection con = null;
        PreparedStatement st = null;
        ResultSet rs = null;
        try{
            con = Jdbcutil.getConnection();
            // 注意和 Statement 的区别！！！
            String sql = "insert into users(id,name,password,email,birthday)" +
                    "values(?,?,?,?,?);";// ? 作为占位符
            st = con.prepareStatement(sql);// 预编译SQL， 先不执行
            // 手动给参数赋值
            st.setInt(1,4); // 占位符计数从1开始
            st.setString(2,"hhh");
            st.setString(3,"12312313");
            st.setString(4,"15612318@qq.com");
            // 注意点：SQL中插入的时间和Java的时间类有区别!
            // sql.Date  util.Date   new Date().getTime())获取时间戳，再转为sql时间类型
            st.setDate(5,new java.sql.Date(new Date().getTime()));

            // 执行SQL语句
            int i = st.executeUpdate(); // 不用再传入参数，和Statement不一样
            if (i > 0) System.out.println("执行成功！");
        }catch(Exception e){
            e.printStackTrace();
        }finally {
            try{
                Jdbcutil.release(con,st,rs);
            }catch(Exception e){
                e.printStackTrace();
            }
        }
    }
}
```

增删改查还是一样的操作，只要换掉sql语句，以及修改参数赋值就行。

小结：

```mysql
sql语句
预编译statement=connection.prepareStatement(sql);
赋值statement.setInt(1,99);
执行statement.executeUpdate();
```

它防止SQL注入的原理：

将传递进来的参数作为字符串（外面包裹一层引号），对于其中的转义字符就直接忽略。

---

JDBC小结：

后期会学习MyBatis框架，掌握JDBC对后面学习很有帮助。

MyBatis 是一款优秀的持久层框架，它支持自定义 SQL、存储过程以及高级映射。MyBatis 免除了几乎所有的 JDBC 代码以及设置参数和获取结果集的工作。

## 3.IDEA连接数据库

在IDEA界面最右侧找到一小块Datebase区域，点击添加数据库。

![image-20210925170351286](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232137987.png)

如果缺少驱动，可以自己下载后导入jar包，也可以点击下方的download下载。

![image-20210925170603690](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232137296.png)

配置好后点击测试，然后apply连接数据库。

如何查看相应数据库？

![image-20210925170701408](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232137305.png)

选择数据库打勾。

如何修改数据？

查看相应表，编辑数据后点击绿色箭头提交。

![image-20210925171001844](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232137498.jpg)

编写SQL语句：

![image-20210925175258920](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232137639.png)

## 4.IDEA处理事务

事务回顾：

![image-20210925190415857](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232137181.png)

测试表：

```mysql
CREATE TABLE account(
        id Int PRIMARY KEY AUTO_INCREMENT,
        NAME VARCHAR(40),
        money FLOAT
);

/*插入测试数据*/
insert into account(name,money) values('A',1000);
insert into account(name,money) values('B',1000);
insert into account(name,money) values('C',1000);
```

处理事务：

```java
package com.jdbc.java.demo;
import com.jdbc.java.demo.utils.Jdbcutil;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

public class JDBCDemo4 {
    public static void main(String[] args) {
        Connection cn = null;
        PreparedStatement st = null;
        ResultSet rs = null;
        try{
            cn = Jdbcutil.getConnection();

            cn.setAutoCommit(false);// 关闭数据库的自动提交，开启事务

            String sql1 = "update account set money=money-100 where  name='A'";
            String sql2 = "update account set money=money+100 where  name='B'";
            st = cn.prepareStatement(sql1);
            st.executeUpdate();
            st = cn.prepareStatement(sql2);
            st.executeUpdate();

            cn.commit();// 业务结束，提交事务
            System.out.println("事务操作成功！");
        }catch(SQLException e){
            e.printStackTrace();
            try {
                cn.rollback(); // 如果出现异常（失败）就回滚
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }finally {
            try{
                Jdbcutil.release(cn,st,rs);
            }catch (Exception e){
                e.printStackTrace();
            }
        }
    }
}
```

小结：

1. 开启事务，`cn.setAutoCommit(false);`
2. 一组业务执行完毕，提交事务
3. 可以在`catch`语句中显式定义回滚语句，但默认失败就会回滚；如果事务正常结束，无法回滚到执行前

## 5.数据库连接池

数据库连接–执行完毕–施放十分消耗资源。

**池化技术：准备一些预先的资源，过来就连接预先准备好的。**

若常用连接数10个，

最小连接数：10个即可。

最大连接数：15 ，业务最高承载上限，超过此值则排队等待。

等待超时：等待时间超过一定值直接失败。

---

编写连接池：实现DataSource接口。

JDBC2.0 提供了javax.sql.DataSource接口，它负责建立与数据库的连接，当在应用程序中访问数据库时
不必编写连接数据库的代码，直接引用DataSource获取数据库的连接对象即可。用于获取操作数据Connection对象。在jdbc中我们获取数据库连接对象，需要先在DriverManager中注册，再通过DriverManager获取Connection对象。

有一些开源数据源实现：

- DBCP
- C3P0
- Druid：阿里巴巴

---

DBCP连接池：

DBCP所需jar包下载： https://segmentfault.com/a/1190000023874560。

下载DBCP的包和Pool的包。注意下载 Binaries 的 zip 文件，然后解压找到两个 jar 包。

将 jar 包导入lib 目录后，将项目 library 设置清空，然后在 lib 目录右键 add library。

然后在 src 目录下新建 dbcpconfig.properties文件。（测速报错记得把SSL改成false！！！）

用到的 jar 包版本如图：

![image-20210925210851183](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232137761.png)

```java
#连接设置
driverClassName=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/jdbcstudy?useUnicode=true&characterEncoding=utf8&useSSL=false
username=root
password=xxx

#!-- 初始化连接 --
initialSize=10

#最大连接数量
maxActive=50

#!-- 最大空闲连接 --
maxIdle=20

#!-- 最小空闲连接 --
minIdle=5

#!-- 超时等待时间以毫秒为单位 6000毫秒/1000等于60秒 --
maxWait=60000
#JDBC驱动建立连接时附带的连接属性属性的格式必须为这样：【属性名=property;】
#注意：user 与 password 两个属性会被明确地传递，因此这里不需要包含他们。
connectionProperties=useUnicode=true;characterEncoding=UTF8

#指定由连接池所创建的连接的自动提交（auto-commit）状态。
defaultAutoCommit=true

#driver default 指定由连接池所创建的连接的只读（read-only）状态。
#如果没有设置该值，则“setReadOnly”方法将不被调用。（某些驱动并不支持只读模式，如：Informix）
defaultReadOnly=

#driver default 指定由连接池所创建的连接的事务级别（TransactionIsolation）。
#可用值为下列之一：（详情可见javadoc。）NONE,READ_UNCOMMITTED, READ_COMMITTED, REPEATABLE_READ, SERIALIZABLE
defaultTransactionIsolation=READ_UNCOMMITTED
```

```java
// Jdbcutil_DBCP
package com.jdbc.java.demo.utils;
import org.apache.commons.dbcp.BasicDataSourceFactory;
import javax.sql.DataSource;
import java.io.InputStream;
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.Properties;

public class Jdbcutil_DBCP { // 封装成工具类
    private static DataSource datasource = null;
    static{
        InputStream in = null;
        try{
            in = Jdbcutil.class.getClassLoader().getResourceAsStream("dbcpconfig.properties");
            Properties properties = new Properties();
            properties.load(in);

            // 创建数据源，工厂模式 --> 创建
            datasource = BasicDataSourceFactory.createDataSource(properties);
        }catch(Exception e){
            e.printStackTrace();
        }finally {
            if (in != null) {
                try {
                    in.close();
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        }
    }

    // 获取连接
    public static Connection getConnection() throws SQLException {
        return datasource.getConnection();// 从数据源中获取连接
    }

    // 释放连接
    public static void release(Connection con, Statement st, ResultSet rs) throws SQLException {
        if (rs != null){
            try{
                rs.close();
            }catch(Exception e){
                e.printStackTrace();
            }
        }
        if (st != null){
            try{
                st.close();
            }catch(Exception e){
                e.printStackTrace();
            }
        }
        if (con != null){
            try{
                con.close();
            }catch(Exception e){
                e.printStackTrace();
            }
        }
    }
}
```

```java
// 测试代码
package com.jdbc.java.demo;

import com.jdbc.java.demo.utils.Jdbcutil_DBCP;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.Date;

public class JDBCDemo5 {
    public static void main(String[] args) {
        Connection cn = null;
        PreparedStatement st = null;
        ResultSet rs = null;

        try{ // 和自己封装的工具类的唯一区别就是更换了数据源！
            cn = Jdbcutil_DBCP.getConnection();

            String sql = "insert into users(id,name,password,email,birthday)" +
                    "values(?,?,?,?,?);";// ? 作为占位符
            st = cn.prepareStatement(sql);// 预编译SQL， 先不执行
            // 手动给参数赋值
            st.setInt(1,4); // 占位符计数从1开始
            st.setString(2,"hhh");
            st.setString(3,"12312313");
            st.setString(4,"15612318@qq.com");
            // 注意点：SQL中插入的时间和Java的时间类有区别!
            // sql.Date  util.Date   new Date().getTime())获取时间戳，再转为sql时间类型
            st.setDate(5,new java.sql.Date(new Date().getTime()));

            // 执行SQL语句
            int i = st.executeUpdate(); // 不用再传入参数，和Statement不一样
            if (i > 0) System.out.println("执行成功！");
        }catch(SQLException e){
            e.printStackTrace();
            try {
                cn.rollback(); // 如果出现异常（失败）就回滚
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }finally {
            try{
                Jdbcutil_DBCP.release(cn,st,rs);
            }catch (SQLException e){
                e.printStackTrace();
            }
        }
    }
}
```

本系列教程的java相关文件见 ：`E:\Java_learning\jdbc_learn`。

完结撒花~ ~