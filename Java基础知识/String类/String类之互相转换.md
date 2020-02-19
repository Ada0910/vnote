# 1. 基本数据类型互相转换
## 1.1. parse()
是SimpleDateFomat里面的方法，你说的应该是parseInt()或parsefloat()这种方法吧，
顾名思义
- 比如说parseInt()就是把String类型转化为int类型。
```
如 String a= "123";
      int b = Integer.parseInt(a);
这样b就等于123了。
```

## 1.2. ValueOf()方法
- 比如说 Integer.valueOf() 是把String类型转化为Integer类型(注意：是Integer类型，而不是int类型，int类型是表示数字的简单类型，Integer类型是一个引用的复杂类型)
```
如：
String a= "123";
Integer c =Integer.valueOf(a);
//Integer类型可以用intValue方法转化为int类型
int b =c.intValue();
这时候这个b就等于123了
```

## 1.3. toString()
- 可以把一个引用类型转化为String字符串类型。
下面举个例子与2相反，把Integer转化为String类型：
```
Integer a = new Integer(123);
String b =a.toString();
这时候b就是 "123" 了
```
