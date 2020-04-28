# 1. 面向对象编程
 - 对象表示真实的单词实体，如：笔，椅子，表等。
 - 面向对象编程是一种使用类和对象来设计程序的方法或模式。它通过提供一些概念简化了软件开发和维护：
对象,类,继承,多态性,抽象,封装,组合
## 1.1. 对象
- 任何具有状态和行为的实体都称为对象
例如：椅子，钢笔，桌子，键盘，自行车等。它可以是物理和逻辑的
## 1.2. 类
对象的集合称为类，它是一个逻辑实体
## 1.3. 继承
当一个对象获取父对象的所有属性和行为时，称为继承。
它提供代码可重用性，它用于实现运行时多态性。继承是面向对象的编程概念，一个对象基于另一个对象构建。继承是代码重用的机制， 被继承的类称为超类，继承超类的类称为子类。
在java中使用extends关键字来实现继承。
下面是java中继承的一个简单示例

```
class SuperClassA {
    public void foo(){
        System.out.println("SuperClassA");
    }

}
// 继承 SuperClassA 类
class SubClassB extends SuperClassA{

    public void bar(){
        System.out.println("SubClassB");
    }

}

public class Test {
    public static void main(String args[]){
        SubClassB a = new SubClassB();

        a.foo();
        a.bar();
    }
}
Java
```
## 1.4. 多态性
当一个任务通过不同的方式执行时，称为多态性。
例如：
以不同的方式说服客户，画一些东西，如：形状或矩形等。在java中，使用方法重载和方法重写来实现多态性。另一个例子是说话，人说人话，猫说话可以是：“喵喵”，而狗说话可能是“旺旺”等，说话时表示和声音也不太一样。
参考以下代码 - 
```
public class Circle {

    public void draw(){
        System.out.println("绘制圆形，默认颜色为黑色，直径为1厘米。");
    }

    public void draw(int diameter){
        System.out.println("绘制圆形，默认颜色为黑色，直径为 "+diameter+"  厘米。");
    }

    public void draw(int diameter, String color){
        System.out.println("绘制圆形，颜色为 "+color+" ，直径为  "+diameter+" 厘米。");
    }
}
```
这里有多种draw()方法，它们都有不同的行为

- 这是方法重载的一种情况，因为所有方法名称都相同且参数不同。这里编译器将能够识别在编译时调用的方法，因此这也称为编译时多态。
- 当在对象之间具有“IS-A”关系时，实现运行时多态性。这也称为方法重写，因为子类必须覆盖超类方法。
如果在超类中，实际的实现类是在运行时决定的。编译器无法决定将调用哪个类方法。此决定在运行时完成，因此这也叫作运行时多态或动态方法分派。
方法重写示例

```
类：Shape.java
public interface Shape {

    public void draw();
}

类：Circle.java
public class Circle implements Shape{

    @Override
    public void draw(){
        System.out.println("绘制圆形");
    }
}


*类：Square.java*
public class Square implements Shape {

    @Override
    public void draw() {
        System.out.println("绘制长方形");
    }

}
```
Shape是超类，它有两个子类Circle和Square，下面是运行时多态性的示例。
类：PolymorphismTest.java
```
public class PolymorphismTest {

    public static void main(String args[]){
        Shape sh = new Circle();
        sh.draw();

        Shape sh1 = getShape(); //一些确定形状的第三方逻辑
        sh1.draw();
    }
}
```

在上面的示例中，java编译器不知道在运行时使用的是哪个Shape的实现类，因此运行时多态性。
## 1.5. 抽象
- 隐藏内部细节和显示功能称为抽象。
例如：电话，但我们不知道内部是如何处理通话/通信的。
- 抽象是隐藏内部细节和用简单的术语描述事物的概念。
例如，添加两个整数的方法。该方法的内部处理对外界是隐藏的。有许多方法可以在面向对象的程序中实现抽象，例如封装和继承。
Java程序也是抽象的一个很好的例子。这里java负责将简单语句转换为机器语言，并隐藏外部世界的内部实现细节。
## 1.6. 封装
- 将代码和数据绑定(或包装)在一起成为单个单元称为封装。
例如：胶囊，它包裹着不同的药物。一个java类是封装的例子。
Java bean是完全封装的类，因为所有的数据成员在这里是私有的

- 封装是用于在面向对象编程中实现抽象的技术。封装用于对类成员和方法的访问限制。
访问修饰符关键字用于面向对象编程中的封装。例如，java中的封装是使用private，protected和public关键字实现的。
## 1.7. 组合
- 组合是聚合的特例。
组合是一种更具限制性的聚合形式。当“HAS-A”关系中包含的对象不能独立存在时，那就是组合的情况。例如，房子里有房间。没有房子，这里的房间不可能存在。
## 1.8. 面向对象编程的优点
- OOP使开发和维护变得更容易，因为在面向过程的编程语言中，如果代码随着项目规模的增长而增长，则不容易管理。
- OOP提供数据隐藏，而在面向过程的编程语言中，可以从任何地方访问全局数据。
- OOP提供更有效地模拟真实世界事件的能力。如果使用面向对象的编程语言，我们可以提供真实世界里的问题的解决方案
# 2. 对象存放的位置
- 寄存器
最快的存储区
- 堆栈
位于通用RAM，主要是用于存放对象引用
- 堆
通用的内存池（也位于RAM区），用于存放所有的Java对象
- 常量存储
常量是存储在程序代码内部中
- 非RAM存储
如流和持久化（持久化到哦数据库）

