# 1. 通用Mapper
通用Mapper就是为了解决单表增删改查，基于Mybatis的插件。开发人员不需要编写SQL，不需要在DAO中增加方法，只要写好实体类，就能支持相应的增删改查方法
# 2. 如何使用
以MySQL为例，假设存在这样一张表：

```
CREATE TABLE `test_table` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT,
  `name` varchar(255) DEFAULT '',
  `create_time` datetime DEFAULT NULL,
  `create_user_id` varchar(32) DEFAULT NULL,
  `update_time` datetime DEFAULT NULL,
  `update_user_id` varchar(32) DEFAULT NULL,
  `is_delete` int(8) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;
```
## 2.1. Maven依赖
```
<!-- 通用Mapper -->
<dependency>
    <groupId>tk.mybatis</groupId>
    <artifactId>mapper</artifactId>
    <version>3.3.9</version>
</dependency>
```
## 2.2. SpringMVC配置
```
<!-- 通用 Mapper -->
<bean class="tk.mybatis.spring.mapper.MapperScannerConfigurer">
    <property name="basePackage" value="cn.com.bluemoon.bd.service.spider.dao"/>
    <property name="properties">
        <value>
            mappers=tk.mybatis.mapper.common.Mapper
        </value>
    </property>
</bean>
```
注意这里使用tk.mybatis.spring.mapper.MapperScannerConfigure替换原来Mybatis的org.mybatis.spring.mapper.MapperScannerConfigurer

## 2.3. 实体类的写法
记住一个原则：实体类的字段数量 >= 数据库表中需要操作的字段数量。默认情况下，实体类中的所有字段都会作为表中的字段来操作，如果有额外的字段，必须加上@Transient注解。

```
@Table(name = "test_table")
public class TestTableVO implements Serializable {

    private static final long serialVersionUID = 1L;

    @Id
    @GeneratedValue(generator = "JDBC")
    private Long id;

    @Transient
    private String userId;

    private String name;

    private Timestamp createTime;

    private String createUserId;

    private Timestamp updateTime;

    private String updateUserId;

    private Integer isDelete;
    
    // 省略get、set...
    
}
```

说明：

- 表名默认使用类名,驼峰转下划线(只对大写字母进行处理),如UserInfo默认对应的表名为user_info
- 表名可以使用@Table(name = "tableName")进行指定,对不符合第一条默认规则的可以通过这种方式指定表名.
- 字段默认和@Column一样,都会作为表字段,表字段默认为Java对象的Field名字驼峰转下划线形式.
- 可以使用@Column(name = "fieldName")指定不符合第3条规则的字段名
- 使用@Transient注解可以忽略字段,添加该注解的字段不会作为表字段使用.
- 建议一定是有一个@Id注解作为主键的字段,可以有多个@Id注解的字段作为联合主键.
- 如果是MySQL的自增字段，加上@GeneratedValue(generator = "JDBC")即可。如果是其他数据库，可以参考官网文档

## 2.4. DAO的写法
在传统的Mybatis写法中，DAO接口需要与Mapper文件关联，即需要编写SQL来实现DAO接口中的方法。而在通用Mapper中，DAO只需要继承一个通用接口，即可拥有丰富的方法：

继承通用的Mapper，必须指定泛型

```
public interface TestTableDao extends Mapper<TestTableVO> {
}
```
一旦继承了Mapper，继承的Mapper就拥有了Mapper 所有的通用方法：

# 3. 常见方法
## 3.1. Select
```
方法：List<T> select(T record);
说明：根据实体中的属性值进行查询，查询条件使用等号

方法：T selectByPrimaryKey(Object key);
说明：根据主键字段进行查询，方法参数必须包含完整的主键属性，查询条件使用等号

方法：List<T> selectAll();
说明：查询全部结果，select(null)方法能达到同样的效果

方法：T selectOne(T record);
说明：根据实体中的属性进行查询，只能有一个返回值，有多个结果是抛出异常，查询条件使用等号

方法：int selectCount(T record);
说明：根据实体中的属性查询总数，查询条件使用等号
```

## 3.2. Insert
```
方法：int insert(T record);
说明：保存一个实体，null的属性也会保存，不会使用数据库默认值

方法：int insertSelective(T record);
说明：保存一个实体，null的属性不会保存，会使用数据库默认值
```

## 3.3. Update
```
方法：int updateByPrimaryKey(T record);
说明：根据主键更新实体全部字段，null值会被更新

方法：int updateByPrimaryKeySelective(T record);
说明：根据主键更新属性不为null的值
```

## 3.4. Delete
```
方法：int delete(T record);
说明：根据实体属性作为条件进行删除，查询条件使用等号

方法：int deleteByPrimaryKey(Object key);
说明：根据主键字段进行删除，方法参数必须包含完整的主键属性

```
## 3.5. Example方法
```
方法：List<T> selectByExample(Object example);
说明：根据Example条件进行查询
重点：这个查询支持通过Example类指定查询列，通过selectProperties方法指定查询列

方法：int selectCountByExample(Object example);
说明：根据Example条件进行查询总数

方法：int updateByExample(@Param("record") T record, @Param("example") Object example);
说明：根据Example条件更新实体record包含的全部属性，null值会被更新

方法：int updateByExampleSelective(@Param("record") T record, @Param("example") Object example);
说明：根据Example条件更新实体record包含的不是null的属性值

方法：int deleteByExample(Object example);
说明：根据Example条件删除数据
```