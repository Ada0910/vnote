# 1. @EnableAutoConfiguration
-  这个注释告诉SpringBoot“猜”你将如何想配置Spring,基于你已经添加jar依赖项。如果spring-boot-starter-web已经添加Tomcat和Spring MVC,这个注释自动将假设您正在开发一个web应用程序并添加相应的spring设置。


- 自动配置被设计用来和“Starters”一起更好的工作,但这两个概念并不直接相关。您可以自由挑选starter依赖项以外的jar包，springboot仍将尽力自动配置您的应用程序。

- spring通常建议我们将main方法所在的类放到一个root包下，@EnableAutoConfiguration（开启自动配置）注解通常都放到main所在类的上面，下面是一个典型的结构布局：


com
```
 +- example
     +- myproject
         +- Application.java
         |
         +- bean
         |   +- Customer.java
         |
         +- service
         |   +- CustomerService.java
         |
         +- web
             +- CustomerController.java
```

- 使用@EnableAutoConfiguration注解时，必须得配置@ComponentScan(basePackages = "com.example.web, com.example.service")，才能扫描service及web下的类，并进行调用

# 2. @SpringBootApplication
- 使用@SpringbootApplication注解 ，可以解决根类或者配置类（我自己的说法，就是main所在类）头上注解过多的问题，一个@SpringbootApplication相当于
`@Configuration,  @EnableAutoConfiguration和 @ComponentScan`，并具有他们的默认属性值
- 查看@SpringBootApplication注解源码：
```
　@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(
    excludeFilters = {@Filter(
    type = FilterType.CUSTOM,
    classes = {TypeExcludeFilter.class}
), @Filter(
    type = FilterType.CUSTOM,
    classes = {AutoConfigurationExcludeFilter.class}
)}
)
public @interface SpringBootApplication {
    .....
}
```
- 看似有这么多注解，实际上@SpringBootApplication是一个"三体"结构，重要的只有三个Annotation：

```
@Configuration
@EnableAutoConfiguration
@ComponentScan
```

- 为什么@SpringBootApplication注解里没有包含@Configuration,实际上是在@SpringBootConfiguration里面
```
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Configuration
public @interface SpringBootConfiguration {
}
```
- @SpringBootConfiguration继承自@Configuration，二者功能也一致，标注当前类是配置类，
并会将当前类内声明的一个或多个以@Bean注解标记的方法的实例纳入到spring容器中，并且实例名就是方法名。

- @ComponentScan 可以解决根类或者配，告诉Spring哪个package的用注解标识的类会被spring自动扫描并且装入bean容器

- 默认情况下是加载和Application类所在同一个目录下的所有类，包括所有子目录下的类