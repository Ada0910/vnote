# 1. Spring的AOP的基于AspectJ注解开发 
## 1.1. 新建工程
导入相关的jar包，新建xml文件，然后在applicationContext.xml之中加入
<aop:aspectj-autoproxy />
## 1.2. Spring的注解AOP的通知类型
@Before：前置通知
![前置通知](_v_images/_前置通知_1531839793_19074.png)
@afeterReturning:后置通知
![后置通知](_v_images/_后置通知_1531839817_11252.png)
@Around：环绕通知
![环绕通知](_v_images/_环绕通知_1531840083_6634.png)
@AfterThrowing：；异常通知
![异常通知](_v_images/_异常通知_1531840426_16024.png)
@After：最终通知
# 2. Spring的JDBC的模板的使用
