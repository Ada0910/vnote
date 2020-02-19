# 1. String数组的初始化和赋值
```
//一维数组
String[] str = new String[5]; //创建一个长度为5的String(字符串)型的一维数组
String[] str = new String[]{"","","","",""};
String[] str = {"","","","",""};
```

## 1.1. String数组初始化区别

 - 首先应该明白java数组里面存的是对象的引用，所以必须初始化才能用；

　　String[] str = {"1","2","3"}与String[] str = newString[]{"1","2","3"}在内存里有什么区别？
　　编译执行结果没有任何区别。更不可能像有些人想当然说的在栈上分配空间，Java的对象都是在堆上分配空间的。

- 区别

```
      String[] str = {"1","2","3"}; 这种形式叫数组初始化式（ArrayInitializer），只能用在声明同时赋值的情况下。
　　而 String[] str = new String[]{"1","2","3"}是一般形式的赋值，=号的右边叫数组字面量（ArrayLiteral），数组字面量可以用在任何需要一个数组的地方（类型兼容的情况下）。如：
　　String[] str = {"1","2","3"}; // 正确的
　　String[] str = new String[]{"1","2","3"} // 也是正确的
而
　　String[] str;
　　str = {"1","2","3"}; // 编译错误
因为数组初始化式只能用于声明同时赋值的情况下。

改为：
　　String[] str;
　　str = new String[] {"1","2","3"}; // 正确了
又如：
　　void f(String[] str) {
　　}
　　f({"1","2","3"}); // 编译错误
正确的应该是：
　　f(new String[] {"1","2","3"});

还可以 String s=new String[30];
```

如果没有显式赋值，则系统自动赋默认值null。


　　笔者所犯错误为在初始化数组的时候定义为String[] str = newString[]{}，如此定义相当于创建了创建一个长度为0的String(字符串)型的一维数组。在后期为其赋值的时候str[0]="A"，就会抛出异常