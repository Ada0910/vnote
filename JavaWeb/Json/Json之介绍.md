# 1. json简介
- JSON(JavaScript Object Notation) 是一种轻量级的数据交换格式。
- JSON是用字符串来表示Javascript对象，例如可以在Servlet中发送一个JSON格式的字符串给客户端Javascript，Javascript可以执行这个字符串，得到一个Javascript对象。
- XML也可以用来佟大为数据交换，前面已经学习过在Servlet中发送XML给Javascript，然后Javascript再去解析XML。
# 2. JSON 语法：
```
数据在名称/值对中
数据由逗号分隔
花括号保存对象
方括号保存数组
var person = {"name":"zhangSan", "age":"18", "sex":"male"};
alert(person.name + ", " + person.age + ", " + person.sex);
```
- 注意，key也要在双引号中！
- JSON值：
```
数字（整数或浮点数）
字符串（在双引号中）
逻辑值（true 或 false）
数组（在方括号中）
对象（在花括号中）
null
var person = {"name":"zhangSan", "age":"18", "sex":"male", "hobby":["cf", "sj", "ddm"]};
alert(person.name + ", " + person.age + ", " + person.sex + ", " + person.hobby);
```

- 带有方法的JSON对象

```
var person = {"name":"zhangSan", "getName":function() {return this.name;}};
alert(person.name);
alert(person.getName());

```
# 3. JSON与XML比较
- 可读性：XML胜出；
- 解码难度：JSON本身就是JS对象（主场作战），所以简单很多；
- 流行度：XML已经流行好多年，但在AJAX领域，JSON更受欢迎。

# 4. 把Java对象转换成JSON对象
- apache提供的json-lib小工具，它可以方便的使用Java语言来创建JSON字符串。也可以把JavaBean转换成JSON字符串。

