---
layout: post
title:  "SpringBoot + Mybatis"
date:   2023-08-31 
categories: SpringBoot Mybatis Java
tags:  SpringBoot Mybatis Java
author: i.itest.ren
---

* content
{:toc}

为方便初始化一个SpringBoot项目，可以利用Spring Initializr（https://start.spring.io/） 初始化一个项目






## SpringBoot + Mybatis 项目搭建
### 01 添加依赖包
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-parent</artifactId>
       <version>2.7.15</version>
       <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>com.itest</groupId>
    <artifactId>springboot-demo</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>springboot-demo</name>
    <description>Demo project for Spring Boot</description>
    <properties>
       <java.version>1.8</java.version>
       <druid-version>1.2.8</druid-version>
       <mysql-connector-java.version>5.1.18</mysql-connector-java.version>
       <fastjson.version>1.2.83</fastjson.version>
    </properties>
    <dependencies>
       <dependency>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter-web</artifactId>
       </dependency>
       <dependency>
          <groupId>org.mybatis.spring.boot</groupId>
          <artifactId>mybatis-spring-boot-starter</artifactId>
          <version>2.3.1</version>
       </dependency>

       <dependency>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-devtools</artifactId>
          <scope>runtime</scope>
          <optional>true</optional>
       </dependency>
       <dependency>
          <groupId>mysql</groupId>
          <artifactId>mysql-connector-java</artifactId>
          <scope>runtime</scope>
          <version>${mysql-connector-java.version}</version>
       </dependency>
       <dependency>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter-test</artifactId>
          <scope>test</scope>
       </dependency>
       <dependency>
          <groupId>org.mybatis.spring.boot</groupId>
          <artifactId>mybatis-spring-boot-starter-test</artifactId>
          <version>2.3.1</version>
          <scope>test</scope>
       </dependency>
       <dependency>
          <groupId>com.alibaba</groupId>
          <artifactId>druid</artifactId>
          <version>${druid-version}</version>
       </dependency>
       <dependency>
          <groupId>com.alibaba</groupId>
          <artifactId>fastjson</artifactId>
          <version>${fastjson.version}</version>
       </dependency>
    </dependencies>

    <build>
       <plugins>
          <plugin>
             <groupId>org.springframework.boot</groupId>
             <artifactId>spring-boot-maven-plugin</artifactId>
          </plugin>
       </plugins>
    </build>

</project>

```

### 02 SpringBoot配置 和 数据库配置
SpringBoot配置文件为application*.yaml / application*.properties / application*.yml 任意一种格式或者命名都可，YAML文件格式是Spring Boot支持的一种JSON超集文件格式，以数据为中心，更简洁直观

本项目命名为application.yaml，内容如下：
```yml
spring:
  datasource:
    username: xxx
    password: xxxxxx
    url: jdbc:mysql://itest.ren:3306/testDS?useUnicode=true&characterEncoding=UTF-8&characterSetResults=UTF-8&zeroDateTimeBehavior=convertToNull
    driver-class-name: com.mysql.jdbc.Driver
    type: com.alibaba.druid.pool.DruidDataSource
    initialSize: 5
    minIdle: 5
    maxActive: 20
    maxWait: 60000
    timeBetweenEvictionRunsMillis: 60000
    minEvictableIdleTimeMillis: 300000
    validationQuery: SELECT 1 FROM emp
    testWhileIdle: true
    testOnBorrow: false
    testOnReturn: false
    poolPreparedStatements: true
    # ?????????filters,??????sql??????wall??????
    filters: stat,wall,log4j
    maxPoolPreparedStatementPerConnectionSize: 20
    userGlobalDataSourceStat: true
    connectionProperties: druid.stat.mergeSql=true;druid.stat.slowSqlMillis=500


server:
  port: 9090
mybatis:
#  config-location: classpath:mybatis/mybatis-config.xml
  mapper-locations: classpath:mappers/*.xml
```

### 03 编写Mybatis代码，生成表 -> pojo -> mapper接口 -> Mapper.xml
需求：使用mybatis，根据ID查询表中的数据
#### 3-1 表结构信息：
```sql
CREATE TABLE `emp` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(50) DEFAULT NULL,
  `job` varchar(50) DEFAULT NULL,
  `salary` double DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=6 DEFAULT CHARSET=utf8
```

#### 3-2 pojo
编写/生成对应表字段的实体类，并生成getter/setter方法

```java
package com.itest.springbootdemo.pojo;

public class Emp {

    //1.声明实体类中的属性
    private Integer id;

    private String name;

    private String job;

    private Double salary;

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getJob() {
        return job;
    }

    public void setJob(String job) {
        this.job = job;
    }

    public Double getSalary() {
        return salary;
    }

    public void setSalary(Double salary) {
        this.salary = salary;
    }

    @Override
    public String toString() {
        return "Emp{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", job='" + job + '\'' +
                ", salary=" + salary +
                '}';
    }
}
```
#### 3-3 mapper interface
企业中都是通过mapper接口进行开发;

注意：此处添加了@Mapper注解，用以Spring把该mapper接口添加到bean容器，否则会报找不到的错误，也可以使用其他bean配置方法

```java
package com.itest.springbootdemo.mapper;

import com.itest.springbootdemo.pojo.Emp;
import org.apache.ibatis.annotations.Mapper;

@Mapper
public interface EmpMapper {

    /**
     * 根据id查询员工信息
     *
     * @param id
     * @return
     */
    public Emp findById(Integer id);
}
```
#### 3-4 Mapper.xml
注意，namespace需要对应pojo类，id需要对应mapper接口中的方法名
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.itest.springbootdemo.mapper.EmpMapper">
    <select id="findById" resultType="com.itest.springbootdemo.pojo.Emp">
        select * from emp where id = #{id}
    </select>
</mapper>
```

### 04 编写SpringBoot代码，Service -> impl -> Controller
#### 4-1 Service interface
给Controller提供调用
```java
package com.itest.springbootdemo.service;

import com.itest.springbootdemo.pojo.Emp;

public interface EmpService {
    /**
     * 根据id查询员工信息
     *
     * @param id
     * @return
     */
    public Emp findById(Integer id);
}
```

#### 4-2 impl
实现上述接口，

注意，实现类中需要增加spring-ioc注解@Service

在此实现类中，调用mybatis mapper接口

```java
package com.itest.springbootdemo.service.impl;

import com.itest.springbootdemo.mapper.EmpMapper;
import com.itest.springbootdemo.pojo.Emp;
import com.itest.springbootdemo.service.EmpService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class EmpServiceImpl implements EmpService {
    @Autowired
    EmpMapper empMapper;

    @Override
    public Emp findById(Integer id) {
        return empMapper.findById(id);
    }
}
```
#### 4-3 Controller
此段内容为暴露http接口，

注意，controller类添加了注解@RestController，是用于创建RESTful API的注解

在控制器类的方法上添加@GetMapping注解，指定要处理的HTTP GET请求的URL路径。方法可以返回各种类型的数据，如String、JSON、XML等。

```java
package com.itest.springbootdemo.controller;

import com.alibaba.fastjson.JSONObject;
import com.itest.springbootdemo.pojo.Emp;
import com.itest.springbootdemo.service.EmpService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.CrossOrigin;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@CrossOrigin
@RestController
public class EmpController {

    @Autowired
    EmpService empService;

    @GetMapping("/find")
    public String find(Integer id){
        Emp result = empService.findById(id);
        return JSONObject.toJSONString(result);
    }
}
```
### 05 运行程序
找到@SpringBootApplication标记的类，点击运行即可

SpringBoot内置了Tomcat容器，所以不需要多余操作，就可以运行程序

测试：运行后，请求地址：http://127.0.0.1:9090/find?id=2  可以正常返回数据

## 跨域问题解决
SpringBoot提供了解决跨域问题的注解，只要在controller类上添加@CrossOrigin，即可实现跨域，非常方便快捷

