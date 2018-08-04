---
layout: post
title:  "JDBC连接数据库"
date:   2016-12-19 01
categories: Java
tags:  Java 数据库 MySQL
author: i.itest.ren
---

* content
{:toc}

JDBC（Java Data Base Connectivity,java数据库连接）是一种用于执行SQL语句的Java API，可以为多种关系数据库提供统一访问，它由一组用Java语言编写的类和接口组成。

JDBC提供了一种基准，据此可以构建更高级的工具和接口，使数据库开发人员能够编写数据库应用程序。

如果要使用数据库就要添加数据库的驱动，不同的数据库有不用的驱动，例如mysql，需要把`mysql-connector-java-3.1.14-bin.jar`build Path到工程里面。






## JDBC连接数据库步骤 ##

### 0x01 加载JDBC驱动程序 ###

在连接数据库之前，首先要加载想要连接的数据库的驱动到JVM（Java虚拟机）， 这通过`java.lang.Class`类的静态方法`forName(String className)`实现。 

例如：

	try{ 
		//加载MySQL的驱动类
		Class.forName("com.mysql.jdbc.Driver");
	}catch(ClassNotFoundException e){   
	    System.out.println("找不到驱动程序类，加载驱动失败！");   
	    e.printStackTrace() ;   
	}

成功加载后，会将Driver类的实例注册到`DriverManager`类中。

### 提供JDBC连接的URL ###

- 连接URL定义了连接数据库时的协议、子协议、数据源标识。
- 书写形式：协议：子协议：数据源标识 
	- 协议：在JDBC中总是以jdbc开始
	- 子协议：是桥连接的驱动程序或是数据库管理系统名称。
	- 数据源标识：标记找到数据库来源的地址与连接端口。 

例如：（MySql的连接URL）

	jdbc:mysql://localhost:3306/testDB?useUnicode=true&characterEncoding=gbk; 

`useUnicode=true`：表示使用Unicode字符集。如果`characterEncoding`设置为gb2312或GBK，本参数必须设置为true。

`characterEncoding=gbk`：字符编码方式。 

### 创建数据库的连接 ###

要连接数据库，需要向`java.sql.DriverManager`请求并获得`Connection`对象，该对象就代表一个数据库的连接。

使用`DriverManager`的`getConnectin(String url, String username, String password)`方法传入指定的欲连接的数据库的路径、数据库的用户名和密码来获得。

例如： 

	//连接MySql数据库，数据库名为testDB，用户名和密码都是root

     String url = "jdbc:mysql://localhost:3306/testDB";   
     String username = "root";
     String password = "root";
     try{
    	 Connection con = DriverManager.getConnection(url, username, password);
     }catch(SQLException se){
    	System.out.println("数据库连接失败！");
    	se.printStackTrace();
     }

### 创建一个Statement ###

要执行SQL语句，必须获得`java.sql.Statement`实例，`Statement`实例分为以下3种类型：

- 执行静态SQL语句。通常通过Statement实例实现。
- 执行动态SQL语句。通常通过PreparedStatement实例实现。
- 执行数据库存储过程。通常通过CallableStatement实例实现。

具体的实现方式：

	//执行静态SQL语句
    Statement stmt = con.createStatement();

	//执行动态SQL语句
    PreparedStatement pstmt = con.prepareStatement(sql);

	//执行数据库存储过程
    CallableStatement cstmt = con.prepareCall("{CALL demoSp(? , ?)}");

### 执行SQL语句 ###

Statement接口提供了三种执行SQL语句的方法：`executeQuery` 、`executeUpdate`和`execute`

- `ResultSet executeQuery(String sqlString)`：执行查询数据库的SQL语句，返回一个结果集`(ResultSet)`对象。
- `int executeUpdate(String sqlString)`：用于执行`INSERT`、`UPDATE`或 `DELETE`语句以及`SQL DD`L语句，如：`CREATE TABLE`和`DROP TABLE`等
- `execute(sqlString)`:用于执行返回多个结果集、多个更新计数或二者组合的语句。

具体实现的代码：

	//三种执行SQL语句的方法
    ResultSet rs = stmt.executeQuery("SELECT * FROM ...");

    int rows = stmt.executeUpdate("INSERT INTO ...");

    boolean flag = stmt.execute(String sql);

### 处理结果 ###

两种情况：

- 执行更新返回的是本次操作影响到的记录数。
- 执行查询返回的结果是一个`ResultSet`对象。


`ResultSet`包含符合SQL语句中条件的所有行，并且它通过一套get方法提供了对这些行中数据的访问。

使用结果集（`ResultSet`）对象的访问方法获取数据：


    while(rs.next()){
    	String name = rs.getString("name");
    	String pass = rs.getString(1);  //此方法比较高效
    }

	//（列是从左到右编号的，并且从列1开始）  

### 关闭JDBC对象 ###

操作完成以后要把所有使用的JDBC对象全都关闭，以释放JDBC资源，关闭顺序和声明顺序相反： 

1. 关闭记录集
2. 关闭声明
3. 关闭连接对象

例如：

     if(rs != null)
     {   // 关闭记录集   
        try{   
            rs.close() ;   
        }catch(SQLException e){   
            e.printStackTrace() ;   
        }   
     }   
     if(stmt != null)
     {   // 关闭声明   
        try{   
            stmt.close() ;   
        }catch(SQLException e){   
            e.printStackTrace() ;   
        }   
     }   
     if(conn != null)
     {  // 关闭连接对象   
         try{   
            conn.close() ;   
         }catch(SQLException e){   
            e.printStackTrace() ;   
         }   
     }

## 程序实例 ##

### 实例1 ###

	/**
	 * @author ：
	 * @date ：
	 */
	import java.sql.DriverManager;
	import java.sql.ResultSet;
	import java.sql.SQLException;
	import java.sql.Connection;
	import java.sql.Statement;
	
	
	public class MysqlDemo {
	    public static void main(String[] args) throws Exception {
	        Connection conn = null;
	        String sql;
	        // MySQL的JDBC URL编写方式：jdbc:mysql://主机名称：连接端口/数据库的名称?参数=值
	        // 避免中文乱码要指定useUnicode和characterEncoding
	        // 执行数据库操作之前要在数据库管理系统上创建一个数据库，名字自己定，
	        // 下面语句之前就要先创建javademo数据库
	        String url = "jdbc:mysql://localhost:3306/sdesm?"
	                + "user=root&password=111111&useUnicode=true&characterEncoding=UTF8";
	
	        try {
	            // 之所以要使用下面这条语句，是因为要使用MySQL的驱动，所以我们要把它驱动起来，
	            // 可以通过Class.forName把它加载进去，也可以通过初始化来驱动起来，下面三种形式都可以
	            Class.forName("com.mysql.jdbc.Driver");// 动态加载mysql驱动
	            // or:
	            // com.mysql.jdbc.Driver driver = new com.mysql.jdbc.Driver();
	            // or：
	            // new com.mysql.jdbc.Driver();
	
	            System.out.println("成功加载MySQL驱动程序");
	            // 一个Connection代表一个数据库连接
	            conn = DriverManager.getConnection(url);
	            // Statement里面带有很多方法，比如executeUpdate可以实现插入，更新和删除等
	            Statement stmt = conn.createStatement();
	            sql = "create table student(NO char(20),name varchar(20),primary key(NO))";
	            int result = stmt.executeUpdate(sql);// executeUpdate语句会返回一个受影响的行数，如果返回-1就没有成功
	            if (result != -1) {
	                System.out.println("创建数据表成功");
	                sql = "insert into student(NO,name) values('2012001','陶伟基')";
	                result = stmt.executeUpdate(sql);
	                sql = "insert into student(NO,name) values('2012002','周小俊')";
	                result = stmt.executeUpdate(sql);
	                sql = "select * from student";
	                ResultSet rs = stmt.executeQuery(sql);// executeQuery会返回结果的集合，否则返回空值
	                System.out.println("学号\t姓名");
	                while (rs.next()) {
	                    System.out
	                            .println(rs.getString(1) + "\t" + rs.getString(2));// 入如果返回的是int类型可以用getInt()
	                }
	            }
	        } catch (SQLException e) {
	            System.out.println("MySQL操作错误");
	            e.printStackTrace();
	        } catch (Exception e) {
	            e.printStackTrace();
	        } finally {
	            conn.close();
	        }
	
	    }
	
	}

### 实例2 ###

	package com.b510;
	 
	import java.sql.Connection;
	import java.sql.DriverManager;
	import java.sql.PreparedStatement;
	import java.sql.ResultSet;
	import java.sql.SQLException;
	/**
	 *
	 * @author Hongten</br>
	 * @date 2012-7-16
	 *
	 */
	public class JDBCTest {
	    public static void main(String[] args) {
	        String driver = "com.mysql.jdbc.Driver";
	        String dbName = "spring";
	        String passwrod = "root";
	        String userName = "root";
	        String url = "jdbc:mysql://localhost:3308/" + dbName;
	        String sql = "select * from users";
	 
	        try {
	            Class.forName(driver);
	            Connection conn = DriverManager.getConnection(url, userName,
	                    passwrod);
	            PreparedStatement ps = conn.prepareStatement(sql);
	            ResultSet rs = ps.executeQuery();
	            while (rs.next()) {
	                System.out.println("id : " + rs.getInt(1) + " name : "
	                        + rs.getString(2) + " password : " + rs.getString(3));
	            }
	 
	            // 关闭记录集
	            if (rs != null) {
	                try {
	                    rs.close();
	                } catch (SQLException e) {
	                    e.printStackTrace();
	                }
	            }
	 
	            // 关闭声明
	            if (ps != null) {
	                try {
	                    ps.close();
	                } catch (SQLException e) {
	                    e.printStackTrace();
	                }
	            }
	 
	            // 关闭链接对象
	            if (conn != null) {
	                try {
	                    conn.close();
	                } catch (SQLException e) {
	                    e.printStackTrace();
	                }
	            }
	 
	        } catch (Exception e) {
	            e.printStackTrace();
	        }
	    }
	 
	}

运行效果：

	id : 3 name : hongten password : 123


参考链接：

http://blog.csdn.net/zhanghong0603/article/details/52621304

http://www.cnblogs.com/hongten/archive/2011/03/29/1998311.html
