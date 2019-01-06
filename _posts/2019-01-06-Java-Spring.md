---
layout: post
title:  "Spring学习笔记"
date:   2019-01-06 01
categories: Java
tags:  Java Spring
author: i.itest.ren
---

* content
{:toc}

Spring 是一个开源框架，是一个分层的 JavaEE 一站式框架。







Spring官网文档地址：https://docs.spring.io/spring/docs/5.1.3.RELEASE/spring-framework-reference/

Spring教程：https://www.w3cschool.cn/wkspring/

## Spring简介 ##

Spring 是一个开源框架，是一个分层的 JavaEE 一站式框架。

所谓一站式框架是指 Spring 有 JavaEE 开发的每一层解决方案。

- WEB层：SpringMVC
- Service层：Spring的Bean管理，声明式事务
- DAO层：Spring的JDBC模板，ORM模板

优点：

- IOC：方便解耦合
- AOP：对程序进行扩展
- 轻量级框架
- 方便与其他框架整合


### Spring使用 ###

Spring开发包解压后的目录介绍：

- docs: Spring 开发规范和API
- libs: Spring jar 包和源代码
- schema: Spring 配置文件的约束

DataAccess 用于数据访问，WEB 用于页面显示，核心容器也就是IOC部分。

## 控制反转（IOC） ##

控制反转（Inversion of Control）是指将对象的创建权反转（交给）Spring。

使用IOC就需要导入IOC相关的包，也就是上图中核心容器中的几个包：beans,context,core,expression四个包。

### 实现原理 ###

传统方式创建对象：

```java
UserDAO userDAO=new UserDAO();
```

进一步面向接口编程，可以多态：

```java
UserDAO userDAO=new UserDAOImpl();
```

这种方式的缺点是接口和实现类高耦合，切换底层实现类时，需要修改源代码。程序设计应该满足OCP元祖，在尽量不修改程序源代码的基础上对程序进行扩展。此时，可以使用工厂模式：

```java
class BeanFactory{
    public static UserDAO getUserDAO(){
        return new UserDAOImpl();
    }
}
```

此种方式虽然在接口和实现类之间没有耦合，但是接口和工厂之间存在耦合。

使用工厂+反射+配置文件的方式，实现解耦，**这也是 Spring 框架 IOC 的底层实现**。

```java
//xml配置文件
//<bean id="userDAO" class="xxx.UserDAOImpl"></bean>
class BeanFactory{
    public static Object getBean(String id){
        //解析XML
        //反射
        Class clazz=Class.forName();
        return clazz.newInstance();
    }
}

```

### IOC XML 开发 ###

在 docs 文件中包含了 xsd-configuration.html 文件。其中定义了 beans schema。

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="
        http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
        
    //在此配置bean
    <bean id="userService" class="x.y.UserServiceImpl">
    </bean>

</beans>
```


调用类：

```java
ApplicationContext applicationContext=new ClassPathXmlApplicationContext("applicationContext.xml");
UserService userService=(UserService)applicationContext.getBean("userService");
userService.save();
```

#### IOC 和 DI

DI 指依赖注入，其前提是必须有 IOC 的环境，Spring 管理这个类的时候将类的依赖的属性注入进来。

例如，在UserServiceImpl.java中：

```java
public class UserServiceImpl implements UserService{
	private String name;
	
	public void setName(String name){
		this.name=name;
	}
	public void save(){
		System.out.println("save "+name);
	}
}
```

在配置文件中：

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="
        http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="userService" class="spring.demo1.UserServiceImpl">
    <!--配置依赖的属性-->
    	<property name="name" value="tony"/>
    </bean>

</beans>
```

测试代码：

```java
@Test
	public void demo2(){
		//创建Spring工厂
		ApplicationContext applicationContext=new ClassPathXmlApplicationContext("applicationContext.xml");
		UserService userService=(UserService)applicationContext.getBean("userService");
		userService.save();
	}
```

运行结果：

```java
save tony
```

可以看到，在配置文件中配置的属性，在 Spring 管理该类的时候将其依赖的属性成功进行了设置。如果不使用依赖注入，则无法使用接口，只能使用实现类来进行设置，因为接口中没有该属性。


#### Spring 的工厂类

- BeanFactory: 老版本的工厂类，在调用getBean()方法时，才会生成类的实例。
- ApplicationContext: 在加载配置文件的时候，就会将 Spring 管理的类都实例化。有两个实现类：
	- ClassPathXmlApplicationContext: 加载类路径下的配置文件
	- FileSystemXmlApplicationContext: 加载磁盘下的配置文件


#### bean标签配置

- id: 唯一约束，不能出现特殊字符
- name: 理论上可以重复，但是开发中最好不要。可以出现特殊字符

生命周期：

- init-method: bean被初始化的时候执行的方法
- destroy-method: bean被销毁的时候执行的方法

作用范围：



- scope： bean的作用范围，有如下几种，常用的是前两种
	- singleton: 默认使用单例模式创建
	- prototype: 多例
	- request: 在web项目中，spring 创建类后，将其存入到 request 范围中
	- session: 在web项目中，spring 创建类后，将其存入到 session 范围中
	- globalsession: 在web项目中，必须用在 porlet 环境

#### 属性注入设置

1. 构造方法方式的属性注入： Car 类在构造方法中有两个属性，分别为 name 和 price。

```xml
<bean id="car" class="demo.Car">
    <constructor-arg name="name" value="bmw">
    <constructor-arg name="price" value="123">
</bean>
```

2. set 方法属性注入： Employee 类在有两个 set 方法，分别设置普通类型的 name 和引用类型的 Car （使用 ref 指向引用类型的 id 或 name）。

```xml
<bean id="employee" class="demo.Employee">
    <property name="name" value="xiaoming">
    <property name="car" ref="car">
</bean>
```

3. P名称空间的属性注入： 首先需要引入p名称空间：

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
    //引入p名称空间
    xmlns:p="http://www.springframework.org/schema/p"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="
        http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
</beans>
```

如果是普通属性：

```xml
<bean id="car" class="demo.Car" p:name="bmv" p:price="123">
</bean>
```

如果是引用类型：
```xml
<bean id="employee" class="demo.Employee" p:name="xiaoming" p:car-ref:"car">
</bean>
```

4. SpEL(Spring Expression Language)属性注入（Spring 3.x以上版本）

```xml
<bean id="car" class="demo.Car">
    <property name="name" value="#{'xiaoming'}">
    <property name="car" ref="#{car}">
</bean>
```

5. 集合类型属性注入：

```xml
<bean id="car" class="demo.Car">
    <property name="namelist">
        <list>
            <value>qirui</value>
            <value>baoma</value>
            <value>benchi</value>
        </list>
    </property>
</bean>
```

#### 多模块开发配置

1. 在加载配置文件的时候，加载多个配置文件
2. 在一个配置文件中引入多个配置文件，通过实现

### IOC 注解开发

#### 示例

1. 引入jar包： 除了要引入上述的四个包之外，还需要引入aop包。
1. 创建 applicationContext.xml ，使用注解开发引入 context 约束（xsd-configuration.html）

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context" xsi:schemaLocation="
        http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd"> 
        <!-- bean definitions here -->
</beans>
```

3. 组件扫描： 使用IOC注解开发，需要配置组件扫描，也就是哪些包下的类使用IOC的注解。

```xml
<context:component-scan base-package="demo1">
```

4. 在类上添加注解

5. 使用注解设置属性的值

属性如果有set方法，将属性注入的注解添加到set方法

属性没有set方法，将注解添加到属性上。

```java
@Component("UserDao")//相当于配置了一个<bean> 其id为UserDao,对应的类为该类
public class UserDAOImpl implements UserDAO {

	@Override
	public void save() {
		// TODO Auto-generated method stub
		System.out.println("save");
	} 
}
```

#### 注解详解

##### @Component

组件注解，用于修饰一个类，将这个类交给 Spring 管理。

有三个衍生的注解，功能类似，也用来修饰类。

- @Controller：修饰 web 层类
- @Service：修饰 service 层类
- @Repository：修饰 dao 层类


##### 属性注入

- 普通属性使用 @Value 来设置属性的值
- 对象属性使用 @Autowired  ，这个注解是按照类型来进行属性注入的。如果希望按照 bean 的名称或id进行属性注入，需要用 @Autowired 和 @Qualifier 一起使用
- 实际开发中，使用 @Resource(name=" ") 来进行按照对象的名称完成属性注入

##### 其他注解

- @PostConstruct 相当于 init-method，用于初始化函数的注解
- @PreDestroy 相当于 destroy-method，用于销毁函数的注解
- @Scope 作用范围的注解，常用的是默认单例，还有多例 @Scope("prototype")

##### IOC 的 XML 和注解开发比较

- 适用场景：XML 适用于任何场景；注解只适合自己写的类，不是自己提供的类无法添加注解。
- 可以使用 XML 管理 bean，使用注解来进行属性注入

##### IOC与传统方式的比较

获取对象方式：传统通过 new 关键字主动创建一个对象。IOC 方式中，将对象的生命周期交给 Spring 管理，直接从 Spring 获取对象。也就是控制反转————将控制权从自己手中交到了 Spring 手中。

## AOP开发

AOP 是 Aspect Oriented Programming 的缩写，意为面向切面编程，通过预编译方式和运行期动态代理实现程序功能的统一维护的一种技术，是OOP的延续。
AOP 能够对程序进行增强，在不修改源码的情况下，可以进行权限校验，日志记录，性能监控，事务控制等。

**也就是说功能分为两大类，一类是核心业务功能，一类是辅助增强功能。两类功能彼此独立进行开发。比如登录功能是核心业务功能，日志功能是辅助增强功能，如果有需要，将日志和登录编制在一起。辅助功能就称为切面，这种能选择性的、低耦合的把切面和核心业务功能结合的编程思想称为切面编程。**

### AOP的初衷

DRY：Dont Repeat Yourself SoC: Sepration of Concerns

- 水平分离：展示层->服务层->持久层
- 垂直分离：模块划分
- 切面分离：分离功能性和非功能性需求

### 为何使用AOP

- 集中处理某一关注点/横切逻辑
- 方便添加/删除关注点
- 侵入性少，增强代码可读性和可维护性


主要应用于权限控制，缓存控制，事务控制，审计日志，性能监控，分布式追踪，异常处理等等。

### 底层实现

JDK 动态代理只能对实现了接口的类产生代理。Cglib 动态代理可以对没有实现接口的类产生代理对象，生成的是子类对象。

**使用 JDK 动态代理：**

```java
public interface UserDao {
	public void insert();
	public void delete();
	public void update();
	public void query();
}
```

实现类：

```java
public class UserDaoImpl implements UserDao {
	@Override
	public void insert() {
		System.out.println("insert");
	}
	@Override
	public void delete() {
		System.out.println("delete");

	}
	@Override
	public void update() {
		System.out.println("update");

	}
	@Override
	public void query() {
		System.out.println("query");
	}
}
```

JDK 代理：

```java
public class JDKProxy implements InvocationHandler{
	
	private UserDao userDao;
	public JDKProxy(UserDao userDao){
		this.userDao=userDao;
	}
	
	public UserDao createProxy(){
		
		UserDao userDaoProxy=(UserDao)Proxy.newProxyInstance(userDao.getClass().getClassLoader(),
				userDao.getClass().getInterfaces(), this);
		return userDaoProxy;
	}

	@Override
	public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
		if("update".equals(method.getName())){
			System.out.println("权限校验");
			return method.invoke(userDao, args);
		}
		return method.invoke(userDao, args);
	}
}
```

通过动态代理增强了 update 函数。 测试类：

```java
public class Demo1 {
	@Test
	public void demo1(){
		UserDao userDao=new UserDaoImpl();
		
		UserDao proxy=new JDKProxy(userDao).createProxy();
		proxy.insert();
		proxy.delete();
		proxy.update();
		proxy.query();
	}
}
```

运行结果为：

```java
insert
delete
权限校验
update
query
```

Cglib Cglib 是第三方开源代码生成类库，可以动态添加类的属性和方法。

与上边JDK代理不同，Cglib的使用方式如下：

```java
public class CglibProxy implements MethodInterceptor{
	
	//传入增强的对象
	private UserDao customerDao;
	
	public CglibProxy(UserDao userDao){
		this.userDao=userDao;
	}
	
	public UserDao createProxy(){
		Enhancer enhancer=new Enhancer();
		enhancer.setSuperclass(userDao.getClass());
		enhancer.setCallback(this);
		UserDao proxy=(UserDao)enhancer.create();
		return proxy;
	}

	@Override
	public Object intercept(Object proxy, Method method, Object[] args, MethodProxy methodProxy) throws Throwable {
		if("save".equals(method.getName())){
			System.out.println("enhance function");
			return methodProxy.invokeSuper(proxy, args);
		}
		return methodProxy.invokeSuper(proxy, args);
	}
}
```

**如果实现了接口的类，底层采用JDK代理。如果不是实现了接口的类，底层采用 Cglib代理。**




### Spring 的 AOP 开发（AspectJ 的 XML 方式）

AspectJ 是一个 AOP 的框架，Spring 引入 AspectJ，基于 AspectJ 进行 AOP 的开发。

#### 相关术语

- Joinpoint: 连接点，可以被拦截到的点。也就是可以被增强的方法都是连接点。
- Pointcut: 切入点，真正被拦截到的点，也就是真正被增强的方法
- Advice: 通知，方法层面的增强。对某个方法进行增强的方法，比如对 save 方法进行权限校验，权限校验的方法称为通知。
- Introduction: 引介，类层面的增强。
- Target: 目标，被增强的对象（类）。
- Weaving: 织入，将 advice 应用到 target 的过程。
- Proxy: 代理对象，被增强的对象。
- Aspect: 切面，多个通知和多个切入点的组合。

#### 使用方法

1. 引入相关包
2. 引入配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:aop="http://www.springframework.org/schema/aop" xsi:schemaLocation="
        http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd"> <!-- bean definitions here -->

</beans>
```

3. 编写目标类并配置：

```java
public class ProductDaoImpl implements ProductDao {
	@Override
	public void save() {
		System.out.println("save");
	}
	@Override
	public void update() {
		System.out.println("update");
	}
	@Override
	public void find() {
		System.out.println("find");
	}
	@Override
	public void delete() {
		System.out.println("delete");
	}
}
```

```xml
<bean id="productDao" class="demo1.ProductDaoImpl"></bean>
```

4. 编写切面类，假设用于权限验证并配置

```java
public class MyAspectXML {
	public void checkPri(){
		System.out.println("check auth");
	}
}
```

```xml
<bean id="myAspect" class="demo1.MyAspectXML"></bean>
```

5. 通过AOP配置完成对目标类的增强

```xml
	<aop:config>
		<aop:pointcut expression="execution(* demo1.ProductDaoImpl.save(..))" id="pointcut1"/>
		
		<aop:aspect ref="myAspect">
			<aop:before method="chechPri" pointcut-ref="pointcut1"/>
		</aop:aspect> 
	</aop:config>
```

#### 通知类型

1. 前置通知：在目标方法执行前操作，可以获得切入点信息

```xml
<aop:before method="chechPri" pointcut-ref="pointcut1"/>
```

```java
public void checkPri(JoinPoint joinPoint){
	System.out.println("check auth "+joinPoint);
}
```

2. 后置通知：在目标方法执行后操作，可以获得方法返回值

```xml
<aop:after-returning method="writeLog" pointcut-ref="pointcut2" returning="result"/>
```

```java
public void writeLog(Object result){
    System.out.println("writeLog "+result);
}
```

3. 环绕通知：在目标方法执行前和后操作，可以阻止目标方法执行

```xml
<aop:around method="around" pointcut-ref="pointcut3"/>
```

```java
public Object around(ProceedingJoinPoint joinPoint) throws Throwable{
	System.out.println("before");
	Object result=joinPoint.proceed();
	System.out.println("after");
	return result;
}
```


4. 异常抛出通知：程序出现异常时操作

```xml
<aop:after-throwing method="afterThrowing" pointcut-ref="pointcut4" throwing="ex"/>
```

```java
public void afterThrowing(Throwable ex){
		System.out.println("exception "+ex.getMessage());
	}
```

5. 最终通知：相当于finally块，无论代码是否有异常，都会执行

```xml
<aop:after method="finallyFunc" pointcut-ref="pointcut4"/>
```

```java
public void finallyFunc(){
	System.out.println("finally");
}
```

6. 引介通知：不常用


#### Spring 切入点表达式

基于 execution 函数完成

语法：[访问修饰符] 方法返回值 包名.类名.方法名(参数)

其中任意字段可以使用*代替表示任意值

### Spring 的 AOP 基于 AspectJ 注解开发


#### 开发步骤

1. 引入jar包
2. 设置配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xmlns:tx="http://www.springframework.org/schema/tx"
	xsi:schemaLocation="http://www.springframework.org/schema/beans 
	http://www.springframework.org/schema/beans/spring-beans.xsd
	http://www.springframework.org/schema/context
	http://www.springframework.org/schema/context/spring-context.xsd
	http://www.springframework.org/schema/aop
	http://www.springframework.org/schema/aop/spring-aop.xsd
	http://www.springframework.org/schema/tx 
	http://www.springframework.org/schema/tx/spring-tx.xsd">
</beans>
```

3. 编写配置目标类

```xml
<bean id="orderDao" class="demo1.OrderDao"></bean>
```

```java
public class OrderDao {
	public void save(){
		System.out.println("save order");
	}
	
	public void update(){
		System.out.println("update order");
	}
	public void delete(){
		System.out.println("delete order");
	}
	public void find(){
		System.out.println("find order");
	}
}
```

4. 开启aop注解自动代理

```xml
<aop:aspectj-autoproxy/>
```

5. 编写切面类并配置

```java
@Aspect
public class MyAspectAnno {
	
	@Before(value="execution(* demo1.OrderDao.save(..))")
	public void before(){
		System.out.println("before");
	}
}
```

```xml
<bean id="myAspect" class="demo1.MyAspectAnno">
```

#### 注解类型

注解主要分为三类：

1. @Aspect：表示这是一个切面类
2. @Pointcut：表示切入点，也就是需要增强的类或方法

这个注解需要借助切面表达式组成。切面表达式由三个部分构成：指示器（designators）、通配符（wildcards）以及操作符（operators）。

指示器分为：

- 匹配方法：execution()
```java
execution(
    [modifier-pattern]// 修饰符
    ret-type-pattern//返回类型
    [declaring-type-pattern]//包名
    name-pattern(param-pattern)//方法名
    [throws-pattern]//抛出异常声明
)
```
- 匹配注解：@target(),@args(),@within(),@annotation()
- 匹配包/类：within()
```java
@Pointcut("within(demo.service.*)")
public void pointcut(){}
```
- 匹配对象：this(),bean(),target()
- 匹配参数：args()
```java
//匹配任何只有一个Long参数的方法
@Pointcut("args(Long)")
//匹配第一个参数为Long的方法
@Pointcut("args(Long,..)")
```

通配符有：

- 匹配任意数量的字符
- 匹配指定类及其子列
- .. 用于匹配任意数的子包或参数

运算符主要有&&，||，！也就是与或非三个。

3. Advice: 在什么时候执行增强的方法

- @Before： 前置通知
- @AfterReturning: 后置通知

```java
@AfterReturning(value="execution(* demo1.OrderDao.save(..))",returning="result")
public void after(Object result){
	System.out.println("after "+result);
}
```

- @Around：环绕通知

```java
@Around(value="execution(* demo1.OrderDao.save(..))")
	public Object around(ProceedingJoinPoint joinPoint) throws Throwable{
		System.out.println("before");
		Object obj=joinPoint.proceed();
		System.out.println("after");
		return obj;
	}
```

- @AfterThrowing: 抛出异常

```java
@AfterThrowing(value="execution(* demo1.OrderDao.save(..))",throwing="e")
public void afterThrowing(Throwable e){
	System.out.println("exception:"+e.getMessage();
}
```


- @After: 最终通知

```java
@After(value="execution(* demo1.OrderDao.save(..))")
	public void after(){
		System.out.println("finally");
	}
```

- @PointCut:切入点注解
```java
@PointCut(value="execution(* demo1.OrderDao.save(..))")
	private void pointcut1(){}
```

注解的好处是，只需要维护切入点即可，不用在修改时修改每个注解。

## Spring 的 JDBC 模板


Spring 对持久层也提供了解决方案，也就是 ORM 模块和 JDBC 的模板。针对 JDBC ，提供了 org.springframework.jdbc.core.JdbcTemplate 作为模板类。

### 使用 JDBC 模板

1. 引入jar包，数据库驱动，Spring 的 jdbc 相关包。
2. 基本使用：

```java
public void demo1(){
    //创建连接池
	DriverManagerDataSource dataSource=new DriverManagerDataSource();
	dataSource.setDriverClassName("com.mysql.jdbc.Driver");
	dataSource.setUrl("jdbc:mysql:///spring4");
	dataSource.setUsername("root");
	dataSource.setPassword("123456");
	
	//创建JDBC模板 
	JdbcTemplate jdbcTemplate=new JdbcTemplate(dataSource);
	jdbcTemplate.update("insert into account values (null,?,?)", "xiaoming",1000d);
	}
```

3. 将连接池和模板交给 Spring 管理

- 配置文件：

```xml
<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource;">
		<property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
		<property name="url" value="jdbc:mysql:///spring4"></property>
		<property name="username" value="root"></property>
		<property name="password" value="123456"></property>
	</bean>
	
	<bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate;"> 
		<property name="dataSource" ref="dataSource"></property>
	</bean>
```

- 测试文件：

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:applicationContext.xml")
public class JdbcDemo2 {
	
	@Resource(name="jdbcTemplate")
	private JdbcTemplate jdbcTemplate;
	
	@Test
	public void demo2(){
		jdbcTemplate.update("insert into account values (null,?,?)", "xiaolan",1000d);
	}
}
```

#### 使用开源数据库连接池

1. 使用 DBCP 的配置：

```xml
<bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource">
		<property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
		<property name="url" value="jdbc:mysql://192.168.66.128/spring4"></property>
		<property name="username" value="root"></property>
		<property name="password" value="123456"></property>
```

2. 使用 C3P0 的配置：

```xml
<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
		<property name="driverClass" value="com.mysql.jdbc.Driver"></property>
		<property name="jdbcUrl" value="jdbc:mysql://192.168.66.128/spring4"></property>
		<property name="user" value="root"></property>
		<property name="password" value="123456"></property>
	</bean>
```

3. 引入外部属性文件

首先建立外部属性文件：

```java
jdbc.driverClass=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://192.168.66.128/spring4
jdbc.username=root
jdbc.password=123456
```

然后对属性文件进行配置：

```xml
<context:property-placeholder location="classpath:jdbc.properties"/>
	
	<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
		<property name="driverClass" value="${jdbc.driverClass}"></property>
		<property name="jdbcUrl" value="${jdbc.url}"></property>
		<property name="user" value="${jdbc.username}"></property>
		<property name="password" value="${jdbc.password}"></property>
	</bean>
```

### CRUD操作

insert, update, delete 语句都借助模板的 update 方法进行操作。

```java
public void demo(){
	jdbcTemplate.update("insert into account values (null,?,?)", "xiaoda",1000d);
	jdbcTemplate.update("update account set name=?,money=? where id=?", "xiaoda",1000d,2);
	jdbcTemplate.update("delete from account where id=?", 6);
}
```

查询操作：

```java
public void demo3(){
		String name=jdbcTemplate.queryForObject("select name from account where id=?",String.class,5);
		long count=jdbcTemplate.queryForObject("select count(*) from account",Long.class);
	}
```

将返回的结果封装成为类：

```java
public void demo4(){
	Account account=jdbcTemplate.queryForObject("select * from account where id=?", new MyRowMapper(),5);
}
```

其中：

```java
class MyRowMapper implements RowMapper<Account>{

	@Override
	public Account mapRow(ResultSet rs, int rowNum) throws SQLException {
		Account account=new Account();
		account.setId(rs.getInt("id"));
		account.setName(rs.getString("name"));
		account.setMoney(rs.getDouble("money"));
		return account;
	}
}
```

## Spring的事务管理

### 事务

事务是指逻辑上的一组操作，组成这组操作的各个单元，要么全部成功，要么全部失败。

具有四个特性：

- 原子性：事务不可分
- 一致性：事务执行前后数据完整性保持一致
- 隔离性：一个事务的执行不应该受到其他事务干扰
- 持久性：一旦事务结束，数据就持久化到数据库

如果不考虑隔离性会引发安全性问题：

- 读问题：
	- 脏读：一个事务读到另一个事务未提交的数据
	- 不可重复读：一个事务读到另一个事务已经提交的 update 数据，导致一个事务中多次查询结果不一致
	- 幻读：一个事务读到另一个事务已经提交的 insert 数据，导致一个事务中多次查询结果不一致


- 写问题：
	- 丢失更新



解决读问题：设置事务隔离级别

- Read uncommitted: 未提交读，无法解决任何读问题
- Read committed: 已提交读，解决脏读问题
- Repeatable read: 重复读，解决脏读和不可重复读问题
- Serializable：序列化，解决所有读问题

### 事务管理API

1. PlatformTransactionManager: 平台事务管理器

这是一个接口，拥有多个不同的实现类，如 DataSourceTransactionManager 底层使用了JDBC 管理事务； HibernateTransactionManager 底层使用了 Hibernate 管理事务。

2. TransactionDefinition: 事务定义信息

用于定义事务的相关信息，如隔离级别、超时信息、传播行为、是否只读等

3. TransactionStatus: 事务的状态

用于记录在事务管理过程中，事务的状态的对象。

**上述API的关系**： Spring 在进行事务管理的时候，首先**平台事务管理器**根据**事务定义**信息进行事务管理，在事务管理过程当中，产生各种此状态，将这些状态信息记录到**事务状态**的对象当中。


### 事务的传播行为

事务的传播行为主要解决业务层（Service）方法相互调用的问题，也就是不同的业务中存在不同的事务时，如何操作。

Spring 中提供了7种事务的传播行为，分为三类：

- 保证多个操作在同一个事务中
	- **PROPAGATION_REQUIRED**: B方法调用A方法，如果A中有事务，使用A中的事务并将B中的操作包含到该事务中；否则新建一个事务，将A和B中的操作包含进来。（默认）
	- PROPAGATION_SUPPORTS：如果A中有事务，使用A的事务；否则不使用事务
	- PROPAGATION_MANDATORY：如果A中有事务，使用A的事务；否则抛出异常
- 保证多个操作不在同一个事务中
	- **PROPAGATION_REQUIRES_NEW**：如果A中有事务，将其挂起，创建新事务，只包含自身操作。否则，新建一个事务，只包含自身操作。
	- PROPAGATION_NOT_SUPPORTED：如果A中有事务，挂起，不使用事务。
	- PROPAGATION_NEVER：如果A中有事务，抛出异常，也即不能用事务运行。
- 嵌套事务
	- **PROPAGATION_NESTED**：如果A有事务，按照A的事务执行，执行完成后，设置一个保存点，然后执行B的操作。如果出现异常，可以回滚到最初状态或保存点状态。

### 实例

以转账为例，业务层的DAO层类如下：

```java
public interface AccountDao {
	public void outMoney(String from,Double money);
	public void inMoney(String to,Double money);
}

public class AccountDaoImpl extends JdbcDaoSupport implements AccountDao{
	
	@Override
	public void outMoney(String from, Double money) {
		 this.getJdbcTemplate().update("update account set money = money - ? where name = ?",money,from);
	}

	@Override
	public void inMoney(String to, Double money) {
		this.getJdbcTemplate().update("update account set money = money + ? where name = ?",money,to);
	}
}

public interface AccountService {
	public void transfer(String from,String to,Double money);
}

public class AccountServiceImpl implements AccountService {

	private AccountDao accountDao;
	
	public void setAccountDao(AccountDao accountDao) {
		this.accountDao = accountDao;
	}
	
	@Override
	public void transfer(String from, String to, Double money) {
				accountDao.outMoney(from, money);
				accountDao.inMoney(to, money);
	}
}
```

在xml中进行类的配置：

```xml
<bean id="accountService" class="tx.demo.AccountServiceImpl">
		<property name="accountDao" ref="accountDao"/>
	</bean>
	
	<bean id="accountDao" class="tx.demo.AccountDaoImpl">
		<property name="dataSource" ref="dataSource"/>
	</bean>
```

### 事务管理1: 编程式事务管理

1. 配置平台事务管理器

```xml
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
	<property name="dataSource" ref="dataSource"></property>
</bean>
```

2. 配置事务管理模板类

```xml
<bean id="transactionTemplate" class="org.springframework.transaction.support.TransactionTemplate">
	<property name="transactionManager" ref="transactionManager"></property>
</bean>
```

3. 在业务层注入事务管理模板

```xml
<bean id="accountService" class="tx.demo1.AccountServiceImpl">
	<property name="accountDao" ref="accountDao"/>
	<property name="transactionTemplate" ref="transactionTemplate"/>
</bean>
```

4. 编码实现事务管理

```java
//ServiceImpl类中：
private TransactionTemplate transactionTemplate;
@Override
public void transfer(String from, String to, Double money) {
	transactionTemplate.execute(new TransactionCallbackWithoutResult() {
	@Override
	protected void doInTransactionWithoutResult(TransactionStatus arg0) {
		accountDao.outMoney(from, money);
		accountDao.inMoney(to, money);
	}
});
}
```

### 声明式事务管理（配置实现，基于AOP思想）

1. XML 方式的声明式事务管理

- 配置事务管理器

```xml
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
	<property name="dataSource" ref="dataSource"></property>
</bean>
```

- 配置事务通知

```xml
<tx:advice id="txAdvice" transaction-manager="transactionManager">
	<tx:attributes>
		<tx:method name="transfer" propagation="REQUIRED"/>
	</tx:attributes>
</tx:advice>
```

- 配置aop事务

```xml
<aop:config>
	<aop:pointcut expression="execution(* tx.demo2.AccountServiceImpl.*(..))" id="pointcut1"/>
	<aop:advisor advice-ref="txAdvice" pointcut-ref="pointcut1"/>
</aop:config>
```

2. 注解方式

- 配置事务管理器，和上方一致
- 开启事务管理的注解：
```xml
<tx:annotation-driven transaction-manager="transactionManager"/>
```

- 在使用事务的类上添加一个注解@Transactional


