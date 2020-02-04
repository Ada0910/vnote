# 1. 各层整合思路
## 1.1. Dao层
使用mybatis框架。创建SqlMapConfig.xml。
创建一个applicationContext-dao.xml
配置数据源
需要让spring容器管理SqlsessionFactory，单例存在。
把mapper的代理对象放到spring容器中。使用扫描包的方式加载mapper的代理对象。
## 1.2. Service层
事务管理
需要把service实现类对象放到spring容器中管理。
## 1.3. 表现层
配置注解驱动
配置视图解析器
需要扫描controller
## 1.4. Web.xml
spring容器的配置
Springmvc前端控制器的配置
Post乱码过滤器


