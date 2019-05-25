# 1. WebJars是什么
将客户端上（浏览器）的js，css等资源打包成jar包形式使用，以便于统一管理
# 2. 为什么使用（方便管理）
我们在开发Java web项目的时候会使用像Maven，Gradle等构建工具以实现对jar包版本依赖管理，以及项目的自动化管理，但是对于JavaScript，Css等前端资源包，我们只能采用拷贝到webapp下的方式，这样做就无法对这些资源进行依赖管理。那么WebJars就提供给我们这些前端资源的jar包形势，我们就可以进行依赖管理。
# 3. 实例
 ```
 添加js库或者css库
 <dependency>  
    <groupId>org.webjars</groupId>  
    <artifactId>bootstrap</artifactId>  
    <version>3.3.7-1</version>  
</dependency>  
<dependency>  
    <groupId>org.webjars</groupId>  
    <artifactId>jquery</artifactId>  
    <version>3.1.1</version>  
</dependency> 
 ```