# 1. bean的作用域
在idea中,bean的作用域有以下几个选择
![](_v_images/20200206161359995_406.png)

- prototype
每次从容器中调用Bean时,都会返回一个新的实例,即每次调用getBean()时,相当于执行了new xxxBean()
- request
每次HTTP请求都会创建一个新的Bean,该作用域仅适用于WebApplicationContext环境
- session
同一个HTTP Session共享一个Bean,不同Session使用不同的Bean,该作用域仅适用于WebApplicationContext环境
- singleton
在Spring IoC容器中仅仅存在一个Bean实例,Bean以单例的模式存在**(默认值)**
## 1.1. singleton
- 当一个bean的作用域为Singleton，那么Spring IoC容器中只会存在一个共享的bean实例，并且所有对bean的请求，只要id与该bean定义相匹配，则只会返回bean的同一实例
- **Singleton是单例类型，就是在创建起容器时就同时自动创建了一个bean的对象**，不管你是否使用，他都存在了，每次获取到的对象都是同一个对象。注意，Singleton作用域是Spring中的缺省作用域。要在XML中将bean定义成singleton，可以这样配置：
```
<bean id="mySecurity" class="com.cms.security.MySecurity" scope="singleton"/>
```
## 1.2. prototype
- 当一个bean的作用域为Prototype，表示一个bean定义对应多个对象实例
- Prototype作用域的bean会导致在每次对该bean请求（将其注入到另一个bean中，或者以程序的方式调用容器的getBean()方法）时都会创建一个新的bean实例
- **Prototype是原型类型，它在我们创建容器的时候并没有实例化，而是当我们获取bean的时候才会去创建一个对象，而且我们每次获取到的对象都不是同一个对象**。根据经验，对有状态的bean应该使用prototype作用域，而对无状态的bean则应该使用singleton作用域。在XML中将bean定义成prototype，可以这样配置：

```
<bean id="account" class="com.foo.DefaultAccount" scope="prototype"/>  
 或者
<bean id="account" class="com.foo.DefaultAccount" singleton="false"/> 
```
## 1.3. request
- 当一个bean的作用域为Request，表示在一次HTTP请求中，一个bean定义对应一个实例
- 即每个HTTP请求都会有各自的bean实例，它们依据某个bean定义创建而成。该作用域仅在基于web的Spring ApplicationContext情形下有效。考虑下面bean定义：

```
<bean id="loginAction" class=cn.csdn.LoginAction" scope="request"/>
```
1
　　针对每次HTTP请求，Spring容器会根据loginAction bean的定义创建一个全新的LoginAction bean实例，且该loginAction bean实例仅在当前HTTP request内有效
　　因此可以根据需要放心的更改所建实例的内部状态，而其他请求中根据loginAction bean定义创建的实例，将不会看到这些特定于某个请求的状态变化。当处理请求结束，request作用域的bean实例将被销毁。

## 1.4. session
- 当一个bean的作用域为Session，表示在一个HTTP Session中，一个bean定义对应一个实例
- 该作用域仅在基于web的Spring ApplicationContext情形下有效。考虑下面bean定义：

```
<bean id="userPreferences" class="com.foo.UserPreferences" scope="session"/>
```
1
　　针对某个HTTP Session，Spring容器会根据userPreferences bean定义创建一个全新的userPreferences bean实例，且该userPreferences bean仅在当前HTTP Session内有效。
　　与request作用域一样，可以根据需要放心的更改所创建实例的内部状态，而别的HTTP Session中根据userPreferences创建的实例，将不会看到这些特定于某个HTTP Session的状态变化。当HTTP Session最终被废弃的时候，在该HTTP Session作用域内的bean也会被废弃掉
# 2. bean的生命周期
## 2.1. 图解
![](_v_images/20200206230916850_4419.png)

## 2.2. 过程

1.  Bean容器在配置文件中找到Spring Bean的定义。
2. Bean容器使用Java Reflection API创建Bean的实例。
3. 如果声明了任何属性，声明的属性会被设置。如果属性本身是Bean，则将对其进行解析和设置。
4. 如果Bean类实现BeanNameAware接口，则将通过传递Bean的名称来调用setBeanName()方法。
5. 如果Bean类实现BeanClassLoaderAware接口，则将通过传递加载此Bean的ClassLoader对象的实例来调用setBeanClassLoader()方法。
6. 如果Bean类实现BeanFactoryAware接口，则将通过传递BeanFactory对象的实例来调用setBeanFactory()方法。
7. 如果有任何与BeanFactory关联的BeanPostProcessors对象已加载Bean，则将在设置Bean属性之前调用postProcessBeforeInitialization()方法。
8. 如果Bean类实现了InitializingBean接口，则在设置了配置文件中定义的所有Bean属性后，将调用afterPropertiesSet()方法。
9. 如果配置文件中的Bean定义包含init-method属性，则该属性的值将解析为Bean类中的方法名称，并将调用该方法。
10. 如果为Bean Factory对象附加了任何Bean 后置处理器，则将调用postProcessAfterInitialization()方法。
11. 如果Bean类实现DisposableBean接口，则当Application不再需要Bean引用时，将调用destroy()方法。
12. 如果配置文件中的Bean定义包含destroy-method属性，那么将调用Bean类中的相应方法定义。
# 3. bean生命周期代码演示
## 3.1. 项目结构图
![](_v_images/20200206232403138_7635.png =807x)
## 3.2. 代码
- Application.java
```
public class Application {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("spring-config.xml");
        ((ClassPathXmlApplicationContext) context).destroy();
    }
}
```
- MyBeanPostProcessor.java
```
public class MyBeanPostProcessor implements BeanPostProcessor {
    /**4.postProcessBeforeInitialization*/
    public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("postProcessBeforeInitialization Before 方法执行了");
        return bean;
    }

    /**7.postProcessAfterInitialization*/
    public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("postProcessAfterInitialization After 方法执行了");
        return bean;
    }
}
```
- Person.java
```
public class Person implements InitializingBean, DisposableBean, BeanFactoryAware, BeanNameAware {

    private String name;

    public void setName(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }


    /**
     * 1.构造方法
     */
    public Person() {
        System.out.println("Person构造方法执行了");
    }

    /**
     * 2.BeanNameAware
     */
    public void setBeanName(String s) {
        System.out.println("setBeanName()执行了");
    }

    /**
     * 3.BeanFactoryAware
     */
    public void setBeanFactory(BeanFactory beanFactory) throws BeansException {
        System.out.println("setBeanFactory()执行了");
    }

    /**
     * 5
     */
    public void afterPropertiesSet() throws Exception {
        System.out.println("afterPropertiesSet()方法执行了");
    }

    /**
     * 6
     */
    public void init() {
        System.out.println("init初始化方法");
    }

    /**
     * 8
     */
    public void destroy() throws Exception {
        System.out.println("destroy()方法执行了");
    }

    /**
     * 9
     */
    public void destroyMethod() {
        System.out.println("destroyMethod()方法执行了");
    }
}
```

- spring-config.xml

``` 
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean name="myBeanPostProcessor" class="MyBeanPostProcessor" />
    <bean name="personBean" class="Person"
          init-method="init" destroy-method="destroyMethod">
        <property name="name" value="Ada" />
    </bean>

</beans>
```
## 3.3. 结果
![](_v_images/20200206232609579_9088.png)



