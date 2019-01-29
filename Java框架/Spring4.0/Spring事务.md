# 1. 事务管理

## 1.1. 事务管理 
### 1.1.1. API
 PlatformTransactionManager：平台事务管理器
（接口，是Spring用于管理事务的真正对象）
DataSourceTransactionManager：底层使用jdbc管理事务
HibernateTransactionManager：底层使用hibernate管理事务

TransactionDefinition：事务定义信息
事务定义：用于定义事务的相关的信息，隔离级别、超时信息、传播行为，是否只读

 TransactionStatus：事务的状态
事务状态：用于记录在事务管理过程中，事务的状态的对象
### 1.1.2. 事务管理的API的关系
Spring进行事务管理的时候，首先平台事务管理器根据**事务定义信息**进行事务管理，在事务管理过程中，产生各种状态，将这些事务信息记录到事态状态对象中
### 1.1.3. 1.1.3.事务传播行为
Spring的七种传播行为：

保证多个操作在同一个事务之中
`**PROPAGATION_REQUIRED(propagetion_required)**` 默认值：如果a中有事务，使用a中的事务，如果a没有，创建一个新的事务，将操作包含进来
`PROPAGATION_SUPPORTS ：`支持事务，如果a中有事务，使用a中的事务，如果a没有事务，不适用事务
`PROPAGATION_MANDATORY：`如果a中有事务，使用a中事务，如果a中没有事务，抛出异 常

保证多个操作不在同一个事务之中
`**PROPAGATION_REQUIRES_NEW**`：如果a中有事务，将a中的事务挂起，创建新事务，只包含自身操作，如果a中没有事务，创建一个新事务，包含自身操作
`PROPAGATION_NOT_SUPPORTED`：如果a中有事务，将a中的事务挂起，不会使用事务
`PROPAGATION_NEVER`  ： 如果a中有事务，直接报异常

嵌套式事务
**PROPAGATION_NESTED**：嵌套事务，如果a中有事务，按照a的事务执行，执行完成后，设置一个保存点，执行b操作，如果没有异常，执行通过，如果有异常，可以选择回滚到最初始位置或者回滚到保存点
### 1.1.4. 事务类型：
一类：编程式事务（需要手动编写代码）

```
<!--配置平台事务管理器：-->
<bean id="transactionManager"
		class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
		<property name="dataSource" ref="dataSource" />
	</bean>

	<!-- 配置事务管理的模板 -->
	<bean id="transactionTemplate"
		class="org.springframework.transaction.support.TransactionTemplate">
		<property name="transactionManager" ref="transactionManager" />
	</bean>

```

二类：声明式事务管理（通过配置实现）AOP
 1.xml方式的声明式事务管理：
 a.引入AOP的jar包
 b.配置事务管理器
 ```
 <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
		<property name="dataSource" ref="dataSource"/>
	</bean>

 ```
 
 	c.配置增强
 	
```
<tx:advice id="txAdvice" transaction-manager="transactionManager">
		<tx:attributes>
		<tx:method name="*" propagation="REQUIRED" read-only="false"/>
		</tx:attributes>
	</tx:advice>
```
d.AOP的配置
```
<aop:config>
		<aop:pointcut expression="execution(* com.itheima.tx.demo2.AccountServiceImpl.*(..))" id="pointcut1"/>
		<aop:advisor advice-ref="txAdvice" pointcut-ref="pointcut1"/>
	</aop:config>
```

 2.注解方式的声明式事务管理：
 a.引入AOP的jar包
 b.配置事务管理器（同上，一定要有，无论是xml还是注解）
 c.开启注解事务
 ```
 <tx:annotation-driven transaction-manager="transactionManager"/>
 ```