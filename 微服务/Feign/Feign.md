# 1. Feign是什么

   Spring Cloud Feign是一套基于Netflix Feign实现的**声明式****服务调用****客户端**。它使得编写Web服务客户端变得更加简单
        
  我们只需要通过创建接口并用注解来配置它既可完成对Web服务接口的绑定。它具备可插拔的注解支持，包括Feign注解、JAX-RS注解。它也支持可插拔的编码器和解码器
        
  Spring Cloud Feign还扩展了对Spring MVC注解的支持，同时还整合了Ribbon和Eureka来提供均衡负载的HTTP客户端实现。
# 2. FeignClient注解

```
 FeignClient注解被@Target(ElementType.TYPE)修饰，表示FeignClient注解的作用目标在接口上
@FeignClient(name = "github-client", url = "https://api.github.com", configuration = GitHubExampleConfig.class)
public interface GitHubClient {
    @RequestMapping(value = "/search/repositories", method = RequestMethod.GET)
    String searchRepo(@RequestParam("q") String queryStr);
}
```

- 参数说明
- name：指定FeignClient的名称，如果项目使用了Ribbon，name属性会作为微服务的名称，用于服务发现
- url: url一般用于调试，可以手动指定@FeignClient调用的地址
- decode404:当发生http 404错误时，如果该字段位true，会调用decoder进行解码，否则抛出FeignException
- configuration: Feign配置类，可以自定义Feign的Encoder、Decoder、LogLevel、Contract
- fallback: 定义容错的处理类，当调用远程接口失败或超时时，会调用对应接口的容错逻辑，fallback指定的类必须实现@FeignClient标记的接口
- fallbackFactory: 工厂类，用于生成fallback类示例，通过这个属性我们可以实现每个接口通用的容错逻辑，减少重复的代码
- path: 定义当前FeignClient的统一前缀
## 2.1. Feign请求超时问题
Hystrix默认的超时时间是**1秒**，如果超过这个时间尚未响应，将会进入fallback代码。而首次请求往往会比较慢（因为Spring的懒加载机制，要实例化一些类），这个响应时间可能就大于1秒了


解决方案有三种，以feign为例

- 方法一
hystrix.command.default.execution.isolation.thread.timeoutInMilliseconds: 5000
该配置是让Hystrix的超时时间改为5秒

- 方法二
hystrix.command.default.execution.timeout.enabled: false
该配置，用于禁用Hystrix的超时时间

- 方法三
feign.hystrix.enabled: false
该配置，用于索性禁用feign的hystrix。该做法除非一些特殊场景，不推荐使用

# 3. okhttp3
## 3.1. 使用
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