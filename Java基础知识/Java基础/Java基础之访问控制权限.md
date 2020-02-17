# 1. 访问权限

## 1.1. private 私有的

- private 关键字是访问控制修饰符，可以应用于类、方法或字段（在类中声明的变量）。 
- 只能在声明 private（内部）类、方法或字段的类中引用这些类、方法或字段。在类的外部或者对于子类而言，它们是不可见的。

## 1.2. protected 受保护的

- protected 关键字是可以应用于类、方法或字段（在类中声明的变量）的访问控制修饰符。
- 可以在声明 protected 类、方法或字段的类、同一个包中的其他任何类以及任何子类（无论子类是在哪个包中声明的）中引用这些类、方法或字段。

## 1.3. public 公共的

- public 关键字是可以应用于类、方法或字段（在类中声明的变量）的访问控制修饰符。 
- 可能只会在其他任何类或包中引用 public 类、方法或字段。

## 1.4. 总结
- Java之中的权限访问修饰符（其实还有一种权限访问情况，就是默认情况，暂且称作default吧）：
Java 中有个访问权限修饰符：**private、protected 以及 public**，如果不加访问修饰符，表示包级可见。


- 设计良好的模块会隐藏所有的实现细节，把它的 API 与它的实现清晰地隔离开来。模块之间只通过它们的 API 进行通信，一个模块不需要知道其他模块的内部工作情况，这个概念被称为信息隐藏或封装。因此访问权限应当尽可能地使每个类或者成员不被外界访问。

- 如果子类的方法覆盖了父类的方法，那么子类中该方法的访问级别不允许低于父类的访问级别。这是为了确保可以使用父类实例的地方都可以使用子类实例，也就是确保满足里式替换原则。

- 字段决不能是公有的，因为这么做的话就失去了对这个实例域修改行为的控制，客户端可以对其随意修改。可以使用共有的 getter 和 setter 方法来替换共有字段。

```java
public class AccessExample {
    public int x;
}
```

```java
public class AccessExample {
    private int x;

    public int getX() {
        return x;
    }

    public void setX(int x) {
        this.x = x;
    }
}
```

但是也有例外，如果是包级私有的类或者私有的嵌套类，那么直接暴露成员不会有特别大的影响。

```java
public class AccessWithInnerClassExample {
    private class InnerClass {
        int x;
    }

    private InnerClass innerClass;

    public AccessWithInnerClassExample() {
        innerClass = new InnerClass();
    }

    public int getValue() {
        return innerClass.x; // 直接访问
    }
}
```







