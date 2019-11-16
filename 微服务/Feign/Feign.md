# 1. Feign是什么
Spring Cloud Feign是一套基于Netflix Feign实现的声明式服务调用客户端。它使得编写Web服务客户端变得更加简单。我们只需要通过创建接口并用注解来配置它既可完成对Web服务接口的绑定。它具备可插拔的注解支持，包括Feign注解、JAX-RS注解。它也支持可插拔的编码器和解码器。Spring Cloud Feign还扩展了对Spring MVC注解的支持，同时还整合了Ribbon和Eureka来提供均衡负载的HTTP客户端实现。
# 2. okhttp3
## 2.1. 使用
- maven
```
<dependency>
            <groupId>io.github.openfeign</groupId>
            <artifactId>feign-okhttp</artifactId>
 </dependency>
```
- 配置文件
```
feign.httpclient.enabled=false
feign.okhttp.enabled=true
```
- 配置类

```
@AutoConfigureBefore(FeignAutoConfiguration.class)
@Configuration
@ConditionalOnClass(Feign.class)
public class FeignOkHttpConfig {
    @Autowired
    OkHttpTokenInterceptor okHttpLoggingInterceptor;

    //读超时时间
    private int feignOkHttpReadTimeout = 60;
    //连接超时时间
    private int feignConnectTimeout = 60;
    //写超时的时间
    private int feignWriteTimeout = 120;

    @Bean
    public okhttp3.OkHttpClient okHttpClient() {
        return new okhttp3.OkHttpClient.Builder()
                //响应时间
                .readTimeout(feignOkHttpReadTimeout, TimeUnit.SECONDS)
                //连接超时
                .connectTimeout(feignConnectTimeout, TimeUnit.SECONDS)
                //写超时
                .writeTimeout(feignWriteTimeout, TimeUnit.SECONDS)
                //连接池
                .connectionPool(new ConnectionPool())
                //添加拦截
                .addInterceptor(okHttpLoggingInterceptor)
                .build();
    }
}

```