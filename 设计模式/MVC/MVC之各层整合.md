# 1. 各层整合思路
## 1.1. 配置文件
- db.properties（文件编码修改为utf-8）
```
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/cms?characterEncoding=utf-8
jdbc.username=root
jdbc.password=123
#定义初始连接数
initialSize=0
#定义最大连接数
maxActive=20
#定义最大空闲
maxIdle=20
#定义最小空闲
minIdle=1
#定义最长等待时间
maxWait=60000
```
- log4j.properties

```
#定义LOG输出级别
log4j.rootLogger=INFO,Console,File
#定义日志输出目的地为控制台
log4j.appender.Console=org.apache.log4j.ConsoleAppender
log4j.appender.Console.Target=System.out
#可以灵活地指定日志输出格式，下面一行是指定具体的格式
log4j.appender.Console.layout = org.apache.log4j.PatternLayout
log4j.appender.Console.layout.ConversionPattern=[%c] - %m%n
 
#文件大小到达指定尺寸的时候产生一个新的文件
log4j.appender.File = org.apache.log4j.RollingFileAppender
#指定输出目录
log4j.appender.File.File = logs/ssm.log
#定义文件最大大小
log4j.appender.File.MaxFileSize = 10MB
# 2. 输出所以日志，如果换成DEBUG表示输出DEBUG以上级别日志
log4j.appender.File.Threshold = ALL
log4j.appender.File.layout = org.apache.log4j.PatternLayout
log4j.appender.File.layout.ConversionPattern =[%p] [%d{yyyy-MM-dd HH\:mm\:ss}][%c]%m%n
```
- 使用mybatis框架。创建SqlMapConfig.xml
```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!-- 别名 -->
    <typeAliases>
        <package name="com.cms.pojo"/>
    </typeAliases>
</configuration>
```
## 1.2. Service层和数据层
- applicationContext.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:p="http://www.springframework.org/schema/p"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:jee="http://www.springframework.org/schema/jee" xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:task="http://www.springframework.org/schema/task"
       xsi:schemaLocation="
        http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.0.xsd
        http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.0.xsd
        http://www.springframework.org/schema/jee http://www.springframework.org/schema/jee/spring-jee-4.0.xsd
        http://www.springframework.org/schema/task  http://www.springframework.org/schema/task/spring-task.xsd
        http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.0.xsd">


    <!-- 加载配置文件 -->
    <context:property-placeholder location="classpath:*.properties"/>

    <!-- 数据库连接池 -->
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource"
          destroy-method="close">
        <property name="url" value="${jdbc.url}"/>
        <property name="username" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
        <property name="driverClassName" value="${jdbc.driver}"/>
        <property name="maxActive" value="10"/>
        <property name="minIdle" value="5"/>
    </bean>


    <!-- 配置mybatis的sqlSessionFactory -->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"/>
        <!-- mybatis配置文件 -->
        <property name="configLocation" value="classpath:SqlMapConfig.xml"></property>
    </bean>


    <!-- 配置扫描包，加载mapper代理对象 -->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="basePackage" value="com.cms.mapper"></property>
    </bean>


    <!-- (事务管理)transaction manager, use JtaTransactionManager for global tx -->
    <bean id="transactionManager"
          class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"/>
    </bean>

    <!-- 自定义Realm -->
    <bean id="mySecurity" class="com.cms.security.MySecurity"/>

    <!-- 安全管理器 -->
    <bean id="securityManager" class="org.apache.shiro.web.mgt.DefaultWebSecurityManager">
        <property name="realm" ref="mySecurity"/>
    </bean>

    <!-- Shiro过滤器 -->
    <bean id="shiroFilter" class="org.apache.shiro.spring.web.ShiroFilterFactoryBean">

        <!-- Shiro的核心安全接口,这个属性是必须的 -->
        <property name="securityManager" ref="securityManager"/>

        <!-- 身份认证失败，则跳转到登录页面的配置 -->
        <property name="loginUrl" value="/login"/>

        <!-- 角色 权限 认证失败，则跳转到的配置 -->
        <property name="unauthorizedUrl" value="/fail"/>

        <!-- Shiro连接约束配置,即过滤链的定义 -->
        <property name="filterChainDefinitions">
            <value>
                /login=anon
                /admin/**=authc
            </value>
        </property>
    </bean>
    <!-- 保证实现了Shiro内部lifecycle函数的bean执行 -->
    <bean id="lifecycleBeanPostProcessor" class="org.apache.shiro.spring.LifecycleBeanPostProcessor"/>
    <!-- 开启Shiro注解 -->
    <bean
            class="org.springframework.aop.framework.autoproxy.DefaultAdvisorAutoProxyCreator"
            depends-on="lifecycleBeanPostProcessor"/>
    <bean
            class="org.apache.shiro.spring.security.interceptor.AuthorizationAttributeSourceAdvisor">
        <property name="securityManager" ref="securityManager"/>
    </bean>
    <!-- 配置事务通知属性 -->
    <tx:advice id="txAdvice" transaction-manager="transactionManager">
        <!-- 定义事务传播属性 -->
        <tx:attributes>
            <tx:method name="insert*" propagation="REQUIRED"/>
            <tx:method name="update*" propagation="REQUIRED"/>
            <tx:method name="edit*" propagation="REQUIRED"/>
            <tx:method name="save*" propagation="REQUIRED"/>
            <tx:method name="add*" propagation="REQUIRED"/>
            <tx:method name="new*" propagation="REQUIRED"/>
            <tx:method name="set*" propagation="REQUIRED"/>
            <tx:method name="remove*" propagation="REQUIRED"/>
            <tx:method name="delete*" propagation="REQUIRED"/>
            <tx:method name="change*" propagation="REQUIRED"/>
            <tx:method name="check*" propagation="REQUIRED"/>
            <tx:method name="doSubscribe" propagation="REQUIRED"/>
            <tx:method name="doSCAN" propagation="REQUIRED"/>
            <tx:method name="get*" propagation="REQUIRED" read-only="true"/>
            <tx:method name="find*" propagation="REQUIRED" read-only="true"/>
            <tx:method name="load*" propagation="REQUIRED" read-only="true"/>
            <tx:method name="*" propagation="REQUIRED" read-only="true"/>
        </tx:attributes>
    </tx:advice>
    <!-- 配置事务切面 -->
    <aop:config>
        <aop:pointcut id="serviceOperation" expression="execution(* com.cms.service.*.*(..))"/>
        <aop:advisor advice-ref="txAdvice" pointcut-ref="serviceOperation"/>
    </aop:config>
    <!-- 自动扫描 -->
    <context:component-scan base-package="com.cms.service"/>
    <!-- 定时任务 -->
    <!-- <context:component-scan base-package="com.cms.quartz"/> -->
    <!--加载定时任务 -->
    <task:annotation-driven/>
</beans>
```

## 1.3. SpringMVC整合
配置注解驱动
配置视图解析器
需要扫描controller
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="
        http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
        http://www.springframework.org/schema/mvc   http://www.springframework.org/schema/mvc/spring-mvc.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.0.xsd
       ">
    <mvc:annotation-driven/>

    <!-- 加载配置文件 -->
    <context:property-placeholder location="classpath:*.properties"/>
    <mvc:resources mapping="/static/**" location="/static/"/>

    <!-- 视图解析器 -->
    <bean id="viewResolver"
          class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/"/>
        <property name="suffix" value=".jsp"></property>
    </bean>

    <!-- 文件上传大小  限制 大概是9.5mb -->
    <bean id="multipartResolver"
          class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
        <property name="defaultEncoding" value="UTF-8"/>
        <property name="maxUploadSize" value="10000000"/>
    </bean>

    <!-- 使用注解的包，包括子集 -->
    <context:component-scan base-package="com.cms.controller"/>
</beans>

```
## 1.4. Web.xml
spring容器的配置
Springmvc前端控制器的配置
Post乱码过滤器
```
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
         id="WebApp_ID" version="3.1">

    <display-name>cms</display-name>
    <welcome-file-list>
        <welcome-file>index.jsp</welcome-file>

    </welcome-file-list>

    <!-- shiro过滤器定义 -->
    <filter>
        <filter-name>shiroFilter</filter-name>
        <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
        <init-param>
            <!-- 该值缺省为false,表示生命周期由SpringApplicationContext管理,设置为true则表示由ServletContainer管理 -->
            <param-name>targetFilterLifecycle</param-name>
            <param-value>true</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>shiroFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
    <!-- Spring配置文件 -->
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:applicationContext.xml</param-value>
    </context-param>
    <!-- 编码过滤器 -->
    <filter>
        <filter-name>encodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <!--<async-supported>true</async-supported>-->
        <init-param>
            <param-name>encoding</param-name>
            <param-value>UTF-8</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>encodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
    <!-- Spring监听器 -->
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>
    <!--  初始化 application  -->
    <listener>
        <listener-class>com.cms.service.impl.InitComponent</listener-class>
    </listener>
    <!-- 添加对springmvc的支持 -->
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
        <!--		<async-supported>true</async-supported>-->
    </servlet>
    <!-- 过虑所有 -->
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
</web-app>
```
