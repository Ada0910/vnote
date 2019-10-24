# 1. WebMvcConfigurer
WebMvcConfigurer配置类其实是Spring内部的一种配置方式，采用JavaBean的形式来代替传统的xml配置文件形式进行针对框架个性化定制
## 1.1. 实现方式
- **一实现WebMvcConfigurer接口（推荐）**
- 二继承WebMvcConfigurationSupport类
# 2. 方法

```
package org.springframework.web.servlet.config.annotation;
import java.util.List;
import org.springframework.format.FormatterRegistry;
import org.springframework.http.converter.HttpMessageConverter;
import org.springframework.lang.Nullable;
import org.springframework.validation.MessageCodesResolver;
import org.springframework.validation.Validator;
import org.springframework.web.method.support.HandlerMethodArgumentResolver;
import org.springframework.web.method.support.HandlerMethodReturnValueHandler;
import org.springframework.web.servlet.HandlerExceptionResolver;

public interface WebMvcConfigurer {
    default void configurePathMatch(PathMatchConfigurer configurer) {
    }

    default void configureContentNegotiation(ContentNegotiationConfigurer configurer) {
    }

    default void configureAsyncSupport(AsyncSupportConfigurer configurer) {
    }

    default void configureDefaultServletHandling(DefaultServletHandlerConfigurer configurer) {
    }

    default void addFormatters(FormatterRegistry registry) {
    }

    default void addInterceptors(InterceptorRegistry registry) {
    }

    default void addResourceHandlers(ResourceHandlerRegistry registry) {
    }

    default void addCorsMappings(CorsRegistry registry) {
    }

    default void addViewControllers(ViewControllerRegistry registry) {
    }

    default void configureViewResolvers(ViewResolverRegistry registry) {
    }

    default void addArgumentResolvers(List<HandlerMethodArgumentResolver> resolvers) {
    }

    default void addReturnValueHandlers(List<HandlerMethodReturnValueHandler> handlers) {
    }

    default void configureMessageConverters(List<HttpMessageConverter<?>> converters) {
    }

    default void extendMessageConverters(List<HttpMessageConverter<?>> converters) {
    }

    default void configureHandlerExceptionResolvers(List<HandlerExceptionResolver> resolvers) {
    }

    default void extendHandlerExceptionResolvers(List<HandlerExceptionResolver> resolvers) {
    }

    @Nullable
    default Validator getValidator() {
        return null;
    }

    @Nullable
    default MessageCodesResolver getMessageCodesResolver() {
        return null;
    }
}

```
# 3. 常见方法
## 3.1. addInterceptors(InterceptorRegistry registry)
 /* 拦截器配置 */
 此方法用来专门注册一个Interceptor，如HandlerInterceptorAdapter

```
@Configuration
public class MyWebMvcConfigurer implements WebMvcConfigurer { 
@Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new MyInterceptor()).addPathPatterns("/**").excludePathPatterns("/emp/toLogin","/emp/login","/js/**","/css/**","/images/**");
    }
}
```
addPathPatterns("/**")对所有请求都拦截，但是排除了/toLogin和/login请求的拦截。

当spring boot版本升级为2.x时，访问静态资源就会被HandlerInterceptor拦截,网上有很多处理办法都是如下写法

```
.excludePathPatterns("/index.html","/","/user/login","/static/**");
```
## 3.2. 页面跳转addViewControllers

```
/**
     * 以前要访问一个页面需要先创建个Controller控制类，再写方法跳转到页面
     * 在这里配置后就不需要那么麻烦了，直接访问http://localhost:8080/toLogin就跳转到login.jsp页面了
     * @param registry
     */
    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        registry.addViewController("/toLogin").setViewName("login");
        
    }
```
值的指出的是，在这里重写addViewControllers方法，并不会覆盖WebMvcAutoConfiguration中的addViewControllers（在此方法中，Spring Boot将“/”映射至index.html），这也就意味着我们自己的配置和Spring Boot的自动配置同时有效，这也是我们推荐添加自己的MVC配置的方式
## 3.3. 自定义资源映射addResourceHandlers
- 比如，我们想自定义静态资源映射目录的话，只需重写addResourceHandlers方法即可。

注：如果继承WebMvcConfigurationSupport类实现配置时必须要重写该方法，具体见其它文章

```
@Configuration
public class MyWebMvcConfigurerAdapter implements WebMvcConfigurer {
    /**
     * 配置静态访问资源
     * @param registry
     */
    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("/my/**").addResourceLocations("classpath:/my/");
       
    }
}
```
通过addResourceHandler添加映射路径，然后通过addResourceLocations来指定路径。我们访问自定义my文件夹中的elephant.jpg 图片的地址为 http://localhost:8080/my/elephant.jpg

- 如果你想指定外部的目录也很简单，直接addResourceLocations指定即可，代码如下：

```
@Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("/my/**").addResourceLocations("file:E:/my/");
        
    }
```
addResourceLocations指的是文件放置的目录，addResoureHandler指的是对外暴露的访问路径

## 3.4. configureDefaultServletHandling(DefaultServletHandlerConfigurer configurer)
```

    @Override
    public void configureDefaultServletHandling(DefaultServletHandlerConfigurer configurer) {
        configurer.enable();
        configurer.enable("defaultServletName");
    }
```
例如：在webroot目录下有一个图片：1.png 我们知道Servelt规范中web根目录（webroot）下的文件可以直接访问的，但是由于DispatcherServlet配置了映射路径是：/ ，它几乎把所有的请求都拦截了，从而导致1.png 访问不到，这时注册一个DefaultServletHttpRequestHandler 就可以解决这个问题。其实可以理解为DispatcherServlet破坏了Servlet的一个特性（根目录下的文件可以直接访问），DefaultServletHttpRequestHandler是帮助回归这个特性的
## 3.5. configureViewResolvers(ViewResolverRegistry registry)
　　从方法名称我们就能看出这个方法是用来配置视图解析器的，该方法的参数ViewResolverRegistry 是一个注册器，用来注册你想自定义的视图解析器等。ViewResolverRegistry 常用的几个方法
### 3.5.1. enableContentNegotiation()
```
/** 启用内容裁决视图解析器*/
public void enableContentNegotiation(View... defaultViews) {
    initContentNegotiatingViewResolver(defaultViews);
}
```
　　该方法会创建一个内容裁决解析器ContentNegotiatingViewResolver ，该解析器不进行具体视图的解析，而是管理你注册的所有视图解析器，所有的视图会先经过它进行解析，然后由它来决定具体使用哪个解析器进行解析。具体的映射规则是根据请求的media types来决定的
### 3.5.2. UrlBasedViewResolverRegistration()
```
    public UrlBasedViewResolverRegistration jsp(String prefix, String suffix) {
        InternalResourceViewResolver resolver = new InternalResourceViewResolver();
        resolver.setPrefix(prefix);
        resolver.setSuffix(suffix);
        this.viewResolvers.add(resolver);
        return new UrlBasedViewResolverRegistration(resolver);
    }
```
　　该方法会注册一个内部资源视图解析器InternalResourceViewResolver 显然访问的所有jsp都是它进行解析的。该方法参数用来指定路径的前缀和文件后缀，如：　　

registry.jsp("/WEB-INF/jsp/", ".jsp");
　对于以上配置，假如返回的视图名称是example，它会返回/WEB-INF/jsp/example.jsp给前端，找不到则报404。　　
### 3.5.3. beanName()
```
    public void beanName() {
        BeanNameViewResolver resolver = new BeanNameViewResolver();
        this.viewResolvers.add(resolver);
    }
```
　　该方法会注册一个BeanNameViewResolver 视图解析器，这个解析器是干嘛的呢？它主要是将视图名称解析成对应的bean。什么意思呢？假如返回的视图名称是example，它会到spring容器中找有没有一个叫example的bean，并且这个bean是View.class类型的？如果有，返回这个bean

### 3.5.4. viewResolver()
```

    public void viewResolver(ViewResolver viewResolver) {
        if (viewResolver instanceof ContentNegotiatingViewResolver) {
            throw new BeanInitializationException(
                    "addViewResolver cannot be used to configure a ContentNegotiatingViewResolver. Please use the method enableContentNegotiation instead.");
        }
        this.viewResolvers.add(viewResolver);
    }
```
　　这个方法想必看名字就知道了，它就是用来注册各种各样的视图解析器的，包括自己定义的。

## 3.6. configureContentNegotiation(ContentNegotiationConfigurer configurer)
