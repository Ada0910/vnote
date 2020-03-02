# 1. 连接步骤
## 1.1. 加载JDBC驱动程序
- 在连接数据库之前，首先要加载想要连接的数据库的驱动到JVM（Java虚拟机）,这通过java.lang.Class类的静态方法forName(String  className)实现。   
    例如：   
```
    try{   
    //加载MySql的驱动类   
    Class.forName("com.mysql.jdbc.Driver") ;   
    }catch(ClassNotFoundException e){   
    System.out.println("找不到驱动程序类 ，加载驱动失败！");   
    e.printStackTrace() ;   
    }   
```
   成功加载后，会将Driver类的实例注册到DriverManager类中。
## 1.2. 创建数据连接对象
- 通过DriverManager类创建数据库连接对象Connection。
- DriverManager类作用于Java程序和JDBC驱动程序之间，用于检查所加载的驱动程序是否可以建立连接，然后通过它的getConnection方法，根据数据库的URL、用户名和密码，创建一个JDBC Connection 对象。如：Connection connection =  DriverManager.geiConnection(“连接数据库的URL", "用户名", "密码”)。其中，URL=协议名+IP地址(域名)+端口+数据库名称；用户名和密码是指登录数据库时所使用的用户名和密码。具体示例创建MySQL的数据库连接代码如下：

```
      Connection connectMySQL  =  DriverManager.geiConnection(“jdbc:mysql://localhost:3306/myuser","root" ,"root" );
```
## 1.3. 创建一个Statement   
 •要执行SQL语句，必须获得java.sql.Statement实例，Statement实例分为以下3种类型：   

-  执行静态SQL语句。通常通过Statement实例实现。   
- 执行动态SQL语句。通常通过PreparedStatement实例实现。   
- 执行数据库存储过程。通常通过CallableStatement实例实现。   
    具体的实现方式：   
```
        Statement stmt = con.createStatement() ;   
       PreparedStatement pstmt = con.prepareStatement(sql) ;   
       CallableStatement cstmt =    
                            con.prepareCall("{CALL demoSp(? , ?)}") ;   
```
## 1.4. 执行SQL语句   
    Statement接口提供了三种执行SQL语句的方法：executeQuery 、executeUpdate   
   和execute   
- ResultSet executeQuery(String sqlString)：执行查询数据库的SQL语句 ，返回一个结果集（ResultSet）对象。   
- int executeUpdate(String sqlString)：用于执行INSERT、UPDATE或 DELETE语句以及SQL DDL语句，如：CREATE TABLE和DROP TABLE等   
 - execute(sqlString):用于执行返回多个结果集、多个更新计数或二者组合的 语句。   
   具体实现的代码：   
```
    ResultSet rs = stmt.executeQuery("SELECT * FROM ...") ;   
    int rows = stmt.executeUpdate("INSERT INTO ...") ;   
    boolean flag = stmt.execute(String sql) ;   
```
## 1.5. 关闭数据库连接
使用完数据库或者不需要访问数据库时，通过Connection的close() 方法及时关闭数据连接
