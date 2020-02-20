# 1. 常见配置内容
web.xml用于配置Web应用的相关信息，如：监听器（listener）、过滤器（filter）、 Servlet、相关参数、会话超时时间、安全验证方式、错误页面

## 1.1. 配置Spring上下文加载监听器加载Spring配置文件并创建IoC容器：

```
  <context-param>
     <param-name>contextConfigLocation</param-name>
    <param-value>classpath:applicationContext.xml</param-value>
  </context-param>

  <listener>
     <listener-class>
       org.springframework.web.context.ContextLoaderListener
     </listener-class>
  </listener>
```

## 1.2. 指定欢迎页面
```
<welcome-file-list> 
  <welcome-file-list> 
    <welcome-file>index.jsp</welcome-file> 
    <welcome-file>index1.jsp</welcome-file> 
  </welcome-file-list> 
```
PS：指定了2个欢迎页面，显示时按顺序从第一个找起，如果第一个存在，就显示第一个，后面的不起作用。如果第一个不存在，就找第二个，以此类推。如果都没有就404

## 1.3. 命名与定制URL。
我们可以为Servlet和JSP文件命名并定制URL,其中定制URL是依赖命名的，命名必须在定制URL前。下面拿serlet来举例： 
(1)、为Servlet命名： 
```
<servlet> 
    <servlet-name>servlet1</servlet-name> 
    <servlet-class>org.whatisjava.TestServlet</servlet-class> 
</servlet> 
```

(2)、为Servlet定制URL、 
```
<servlet-mapping> 
    <servlet-name>servlet1</servlet-name> 
    <url-pattern>*.do</url-pattern> 
</servlet-mapping>
```

## 1.4. 定制初始化参数
可以定制servlet、JSP、Context的初始化参数，然后可以再servlet、JSP、Context中获取这些参数值。 

下面用servlet来举例： 
```
<servlet> 
    <servlet-name>servlet1</servlet-name> 
    <servlet-class>org.whatisjava.TestServlet</servlet-class> 
    <init-param> 
          <param-name>userName</param-name> 
          <param-value>Daniel</param-value> 
    </init-param> 
    <init-param> 
          <param-name>E-mail</param-name> 
          <param-value>125485762@qq.com</param-value> 
    </init-param> 
</servlet> 
```
经过上面的配置，在servlet中能够调用getServletConfig().getInitParameter("param1")获得参数名对应的值

## 1.5. 指定错误处理页面
可以通过“异常类型”或“错误码”来指定错误处理页面。 
```
<error-page> 
    <error-code>404</error-code> 
    <location>/error404.jsp</location> 
</error-page> 
----------------------------- 
<error-page> 
    <exception-type>java.lang.Exception<exception-type> 
    <location>/exception.jsp<location> 
</error-page> 
```
## 1.6. 设置过滤器
比如设置一个编码过滤器，过滤所有资源 
```
<filter> 
    <filter-name>XXXCharaSetFilter</filter-name> 
    <filter-class>net.test.CharSetFilter</filter-class> 
</filter> 
<filter-mapping> 
    <filter-name>XXXCharaSetFilter</filter-name> 
    <url-pattern>/*</url-pattern> 
</filter-mapping> 
```

## 1.7. 设置监听器
```
<listener> 
<listener-class>net.test.XXXLisenet</listener-class> 
</listener> 
```



## 1.8. 设置会话(Session)过期时间
其中时间以分钟为单位，假如设置60分钟超时： 
```
<session-config> 
<session-timeout>60</session-timeout> 
</session-config>
```