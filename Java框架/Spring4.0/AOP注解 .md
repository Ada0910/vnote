# 1. Spring的AOP（核心）
## 1.1. AOP的概念
	* 面向切面编程是OOP的 扩展和延伸，解决OOP开发遇到的问题
	* AOP采用的是横向抽取机制（代理机制）取代了传统的纵向继承
	* 底层实现的原理：动态代理（
              JDK动态代理，只能对实现 了接口的类产生代理            
              Cglib动态代理，对没有实现接口的类产生代理对象）
              
## 1.2. Spring的AOP开发（AspectJ的XML的方式）
	* Spring的AOP的简介
	* AspectJ是一个AOP的框架  
## 1.3. AOP开发中的术语
	
### 1.3.1. 连接点(Join point)：

能够被拦截的地方：Spring AOP是基于动态代理的，所以是方法拦截的。每个成员方法都可以称之为连接点~
### 1.3.2. 切点(Poincut)：

具体定位的连接点：上面也说了，每个方法都可以称之为连接点，我们具体定位到某一个方法就成为切点。
### 1.3.3. 增强/通知(Advice)：

表示添加到切点的一段逻辑代码，并定位连接点的方位信息。
简单来说就定义了是干什么的，具体是在哪干
Spring AOP提供了5种Advice类型给我们：前置、后置、返回、异常、环绕给我们使用！
### 1.3.4. 织入(Weaving)：

将增强/通知添加到目标类的具体连接点上的过程。
### 1.3.5. 引入/引介(Introduction)：

引入/引介允许我们向现有的类添加新方法或属性。是一种特殊的增强！
### 1.3.6. 切面(Aspect)：

切面由切点和增强/通知组成，它既包括了横切逻辑的定义、也包括了连接点的定义。
在《Spring 实战 (第4版)》给出的总结是这样子的：

通知/增强包含了需要用于多个应用对象的横切行为；连接点是程序执行过程中能够应用通知的所有点；切点定义了通知/增强被应用的具体位置。其中关键的是切点定义了哪些连接点会得到通知/增强。

总的来说：

这些术语可能翻译过来不太好理解，但对我们正常使用AOP的话影响并没有那么大~~看多了就知道它是什么意思了。

# 2. Spring的AOP入门（AspectJ的XML的方式）
## 2.1. 需要的Java包（Spring基础包加上以下几个）
aspectjrt.jar
aspectjweaver.jar
aopalliance-1.0.jar,
	
	Spring整合Junit开发
![](_v_images/_1531991569_7251.png)

## 2.2. Spring的通知类型
5.2.1前置通知：在目标方法执行之前进行操作
       *可以获得切入点信息
5.2.2后置通知：之后
        获得方法的返回值
5.2.3环绕通知：之前和之后
5.2.4异常抛出通知：在程序出现异常的时候进行操作
5.2.5最终通知：无论代码是否有异常，总是会被执行

## 2.3. 切入点表达式语法
     #语法：（基于execution的函数完成的）
              [访问修饰符] 方法返回值 包名.类名.方法名（参数）
              #public void com.spring.aop.CustomerDao.save(...)
              #*****Dao.save()
              #*com.spring.aop.CustomerDao+.save(..)   ＋表示是子类
              #*com.spring.aop.CustormerDao.*.*(...)
## 2.4. AOP思想

![AOP思想](_v_images/_aop思想_1531811951_1314.png)
# 3. Spring的AOP的基于AspectJ注解开发 
## 3.1. 新建工程
导入相关的jar包，新建xml文件，然后在applicationContext.xml之中加入
<aop:aspectj-autoproxy />

## 3.2. Spring的注解AOP的通知类型
@Before：前置通知
![前置通知](_v_images/_前置通知_1531839793_19074.png)
@afeterReturning:后置通知
![后置通知](_v_images/_后置通知_1531839817_11252.png)
@Around：环绕通知
![环绕通知](_v_images/_环绕通知_1531840083_6634.png)
@AfterThrowing：；异常通知
![异常通知](_v_images/_异常通知_1531840426_16024.png)
@After：最终通知

# 4. Spring的JDBC的模板的使用
## 4.1. Spring提供的模板
![ORM持久化技术](_v_images/_orm持久化技术_1531893480_25137.png)

## 4.2. 将连接池和模板交给Spring管理
在applicationContext.xml文件中配置如下
```
连接池
<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
		<property name="driverClassName" value="com.mysql.jdbc.Driver"/>
		<property name="url" value="jdbc:mysql:///test"/>
		<property name="username" value="root"/>
		<property name="password" value="123"/>
	</bean>
	jdbc模板
<bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
		<property name="dataSource" ref="dataSource" />
	</bean>
```

# 5. 使用开源的数据库连接池
## 5.1. DBCP的使用
### 5.1.1. 引入jar包
![jar包](_v_images/_jar包_1531917499_9507.png)
### 5.1.2. 配置applicationContext.xml文件
```<bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource">
		<property name="driverClassName" value="com.mysql.jdbc.Driver" />
		<property name="url" value="jdbc:mysql:///test" />
		<property name="username" value="root" />
		<property name="password" value="123" />
	</bean>
```

## 5.2. C3P0的使用 
引入Jar包
![](_v_images/_1531917641_18985.png)
配置applicationContext.xml文件
```<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
		<property name="driverClass" value="${jdbc.driverClass}" />
		<property name="jdbcUrl" value="${jdbc.url}" />
		<property name="user" value="${jdbc.username}" />
		<property name="password" value="${jdbc.password}" />
	</bean>
```
属性文件：
```jdbc.driverClass=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql:///test
jdbc.username=root
jdbc.password=123
```
引入属性文件
方式一：（很少情况使用）
```
<bean class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer"> 
		<property name="location" value="classpath:jdbc.properties"/>
```
方式二：
```
<context:property-placeholder location="classpath:jdbc.properties"></context:property-placeholder>
```

