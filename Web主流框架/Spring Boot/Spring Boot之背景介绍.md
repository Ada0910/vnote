# 1. Spring Boot产生的背景
- SpringBoot是伴随着Spring4.0诞生的
- 从字面理解，Boot是引导的意思，因此SpringBoot帮助开发者快速搭建Spring框架
# 2. Spring Boot是什么
- Spring Boot makes it easy to create stand-alone, production-grade Spring based Applications that you can "just run".
We take an opinionated view of the Spring platform and third-party libraries so you can get started with minimum fuss. Most Spring Boot applications need very little Spring configuration.

翻译:
使用Spring Boot可以轻松地创建独立的，基于生产级别的基于Spring的应用程序，您可以“运行”它们。
我们对Spring平台和第三方库持固执己见的观点，因此您可以以最小的麻烦开始使用。 大多数Spring Boot应用程序只需要很少的Spring配置。

其实 Spring Boot 其实不是什么新的框架，它默认配置了很多框架的使用方式，就像 maven 整合了所有的 jar 包，Spring Boot 整合了所有的框架

- springboot的本质
**web应用中嵌了一个servlet容器，使得web应用程序可以转化为可执行的jar文件**
# 3. Spring Boot的优缺点
## 3.1. 优点/特性
- 约定大于配置
- - 约定优于配置（Convention Over Configuration）,也称作按约定编程是一种软件设计范式。目的在于减少软件开发人员所需要做出的决定的数量，从而获得简单的好处，而又不失去其中的灵活性。开发人员仅仅需要规定应用中不符合约定的部分。例如，如果模型中有个名为User的类，数据库中对应的表就会默认命名为user。只有在偏离这一约定的时候,比如将该表命名为"user_info"，才会需要写有关这个名字的配置。如果所用工具的约定与你的期待相符，便可省去配置；反之，你可以配置来达到你所期待的方式

- - 默认有resources文件夹,存放资源配置文件。src-main-resources,src-main-java
- - 默认有target文件夹，将生成class文件盒编程生成的jar存放在target文件夹下
- - spring boot默认的配置文件必须是，也只能是application.命名的yml文件或者properties文件，spring boot默认只会去src-main-resources文件夹下去找application配置文件

- 快速开发
- - 使用 Spring 项目引导页面可以在几秒构建一个项目方便对外输出各种形式的服务，如 REST API、WebSocket、Web、Streaming、Tasks
- - SpringBoot帮助开发者快速启动一个Web容器,比如支持运行期内嵌容器，如 Tomcat、Jetty,像构建好一个默认的Spring Boot项目就是自带tomcat的,一键启动Application而无需启动Tomcat

- 强大的开发包,支持热启动
比如给我们封装了各种经常使用的套件，比如mybatis、hibernate、redis、mongodb等,且支持各种编译器如 IntelliJ IDEA 、NetBean,eclipse等

- 支持关系数据库和非关系数据库

- SpringBoot继承了原有Spring框架的优秀基因且简化了使用Spring的过程
其一:Spring由于其繁琐的配置，一度被人认为“配置地狱”，各种XML、Annotation配置，让人眼花缭乱，而且如果出错了也很难找出原因。Spring Boot更多的是采用Java Config的方式即注解，对Spring进行配置

- 非常简洁的安全策略集成
- Spring Boot 让部署变得更简单,自动管理依赖
- 自带应用监控

## 3.2. 缺点
- 集成度较高，使用过程中不太容易了解底层
- Spring Boot作为一个微框架，离微服务的实现还是有距离的。没有提供相应的服务发现和注册的配套功能，自身的acturator所提供的监控功能，也需要与现有的监控对接。没有配套的安全管控方案，对于REST的落地，还需要自行结合实际进行URI的规范化工作
## 3.3. 应用
- 如果公司需要快速开发或者没有自己的一套SSMXML配置开发规范的话
- 如果想要简化开发的配置,比如要整合各种数据库或者配置各种各样觉得麻烦的话
- 可以一键云部署到远程服务器上
- 业务增长需要拆成微服务或者是分布式,Spring Boot是基础,可以在Spring Boot的基础上慢慢扩展和搭建


