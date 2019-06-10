# 1. Springboot整合mybatis
添加依赖：
```
mybatis整合包
<dependency>
			<groupId>org.mybatis.spring.boot</groupId>
			<artifactId>mybatis-spring-boot-starter</artifactId>
			<version>1.3.0</version>
		</dependency>
		
mysql的驱动
<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<version>5.1.35</version>
		</dependency>

连接池
<dependency>
			<groupId>com.alibaba</groupId>
			<artifactId>druid-spring-boot-starter</artifactId>
			<version>1.1.0</version>
		</dependency>

```
在application.properties中配置
```
spring.datasource.driverClassName=com.mysql.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306:/ada
spring.datasource.username=root
spring.datasource.password=123

spring.datasource.type = com.alibaba.druid.pool.DruidDataSource
mybatis.type-aliases-package= com.ada.demo.pojo;
```