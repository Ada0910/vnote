# 1. Java中的关键字有哪些
|        类型        |           |             |                 |              |                |              |         |
| ------------------ | --------- | ----------- | --------------- | ------------ | -------------- | ------------ | ------- |
| 访问控制            | private   | protected   | public          |              |                |              |         |
| 类，方法和变量修饰符 | abstract  | 	class      | 	extends	     | final	     | implements     | 	interface |         |
|                    | new	     | static      | 	strictfp	 | synchronized | 	transient	 | volatile	    |         |
| 程序控制            | 	break    | 	 continue  | 	 return      | 	 do   	 | while          | if           | 	else |
|                    | for       | 	instanceof | 	switch       | 	case     | 	default       | 	         |         |
| 错误处理            | 	try      | catch       | 	throw        | 	throws	    | finally        | 	           | 	    |
| 包相关             | 	import  | 	package	  | 	            |              | 	              |              | 	     |
| 基本类型            | 	boolean  | 	byte       | 	char	     | double       | 	float	         | int	        | long    |
|                    | short	 | null        | 	true	     | false	     | 	            | 	           |         |
| 变量引用            | 	super	 | this	       | void            | 	         | 	             | 	           | 	     |

## 1.1. 48个关键字
abstract、assert、boolean、break、byte、case、catch、char、class、continue、default、do、double、else、enum、extends、final、finally、float、for、if、implements、import、int、interface、instanceof、long、native、new、package、private、protected、public、return、short、static、strictfp、super、switch、synchronized、this、throw、throws、transient、try、void、volatile、while。
## 1.2. 2个保留字
- （现在没用以后可能用到作为关键字）：goto、const。
## 1.3. 3个特殊直接量
true、false、null
# 2. 部分详解
## 2.1. final

**（1）. 数据** 

声明数据为常量，可以是编译时常量，也可以是在运行时被初始化后不能被改变的常量。

- 对于基本类型，final 使数值不变；
- 对于引用类型，final 使引用不变，也就不能引用其它对象，但是被引用的对象本身是可以修改的。

```java
final int x = 1;
// x = 2;  // cannot assign value to final variable 'x'
final A y = new A();
y.a = 1;
```

**（2）. 方法**  </font> </br>

声明方法不能被子类覆盖。

private 方法隐式地被指定为 final，如果在子类中定义的方法和基类中的一个 private 方法签名相同，此时子类的方法不是覆盖基类方法，而是重载了。

**（3）. 类** 

声明类不允许被继承

## 2.2. static

**（1）. 静态变量** 

静态变量在内存中只存在一份，只在类第一次实例化时初始化一次。

- 静态变量：类所有的实例都共享静态变量，可以直接通过类名来访问它；
- 实例变量：每创建一个实例就会产生一个实例变量，它与该实例同生共死。

```java
public class A {
    private int x;        // 实例变量
    public static int y;  // 静态变量
}
```

**（2）. 静态方法** 

静态方法在类加载的时候就存在了，它不依赖于任何实例，所以 static 方法必须实现，也就是说它不能是抽象方法（abstract）。

**（3）. 静态语句块** 

静态语句块和静态变量一样在类第一次实例化时运行一次。

**（4）. 初始化顺序** 

静态数据优先于其它数据的初始化，静态变量和静态语句块哪个先运行取决于它们在代码中的顺序。

```java
public static String staticField = "静态变量";
```

```java
static {
    System.out.println("静态语句块");
}
```

实例变量和普通语句块的初始化在静态变量和静态语句块初始化结束之后。

```java
public String field = "实例变量";
```

```java
{
    System.out.println("普通语句块");
}
```

最后才是构造函数中的数据进行初始化

```java
public InitialOrderTest() {
    System.out.println("构造函数");
}
```

存在继承的情况下，初始化顺序为：

a . 父类（静态变量、静态语句块块）
b . 子类（静态变量、静态语句块）
c . 父类（实例变量、普通语句块）
d . 父类（构造函数）
e . 子类（实例变量、普通语句块）
f . 子类（构造函数）
## 2.3. super和this
- 在JAVA类中使用**super**来引用父类的成分,用**this**来引用当前对象,在子类中调用父类的属性或方法
- 如果一个类从另外一个类继承，我们new这个子类的实例对象的时候，这个子类对象里面会有一个父类对象。怎么去引用里面的父类对象呢？使用super来引用，this指的是当前对象的引用，super是当前对象里面的父对象的引用。
如：

```
/**
 * @author 谢伟宁
 * @date2018/12/20 10:01
 */


    class Student {
        public int age;
        public void std(){ //声明Student类的方法std()
            age = 15;
            System.out.println("学生平均年龄为："+age);
        }
    }

    class ThisStudent extends Student{
        public int age;
        public void std(){
            super.std();  //使用super作为父类对象的引用对象来调用父类对象里面的Std()方法
            age = 18;
            System.out.println("这个学生的年龄为："+age);
            System.out.println(super.age);  //使用super作为父类对象的引用对象来调用父类对象中的age值
            System.out.println(age);
        }
    }

public class SuperTest {
        public static void main(String[] args) {
            （1）ThisStudent a = new ThisStudent();
            （2）a.std();
        }
    }

执行结果：
学生平均年龄为：15
这个学生的年龄为：18
15
18
```

分析：
```
（1）ThisStudent a = new ThisStudent();
```

- 首先在栈空间里面会产生一个变量a（a里面的值是什么这不好说），通过这个值我们可以找到new出来的ThisStudent对象。由于子类ThisStudent是从父类Student继承下来的，所以当我们new一个子类对象的时候，这个子类对象里面会包含有一个父类对象，而这个父类对象拥有他自身的属性age

- 这个age成员变量在Student类里面声明的时候并没有对他进行初始化，所以系统默认给它初始化为0，**成员变量（在类里面声明）在声明时可以不给它初始化**，编译器会自动给这个成员变量初始化，但**局部变量（在方法里面声明）在声明时一定要给它初始化**，因为编译器不会自动给局部变量初始化，任何变量在使用之前必须对它进行初始化

- 子类在继承父类age属性的同时，自己也单独定义了一个age属性，所以当我们new出一个子类对象的时候，这个对象会有两个age属性，一个是从父类继承下来的age，另一个是自己的age。在子类里定义的成员变量age在声明时也没有给它初始化，所以编译器默认给它初始化为0

因此，执行完第一句话以后，系统内存的布局如下图所示：
![](_v_images/_1545271575_15969.png)

```
（2）a.std();
```

- 当new一个对象出来的时候，这个对象会产生一个this的引用，这个this引用指向对象自身。如果new出来的对象是一个子类对象的话，那么这个子类对象里面还会有一个super引用，这个super指向当前对象里面的父对象。所以相当于程序里面有一个this，this指向对象自己，还有一个super，super指向当前对象里面的父对象

- 调用重写之后的std()方法，方法体内的第一句话：“`super.std();`”是让这个子类对象里面的父对象自己调用自己的f()方法去改变自己age属性的值，父对象通过指向他的引用super来调用自己的std()方法，所以执行完这一句以后，父对象里面的age的值变成了15。接着执行“age=18；”这里的age是子类对象自己声明的value，不是从父类继承下来的那个age。所以这句话执行完毕后，子类对象自己本身的age值变成了18。


此时的内存布局如下图所示：
![](_v_images/_1545271593_7598.png)

- 方法体内的最后三句话都是执行打印age值的命令，前两句打印出来的是子类对象自己的那个age值，因此打印出来的结果为18，最后一句话打印的是这个子类对象里面的父类对象自己的age值，打印出来的结果为15。

### 2.3.1. 应用范围

  - 只能用于子类的构造函数和实例方法中，不能用于子类的类（静态）方法中。
 原因是super指代的是一个父类的对象，它需要在运行时被创建，而静态方法是类方法，它是类的一部分。当类被加载时，方法已经存在，但是这时候父类对象还没有被初始化
## 2.4. instanceof
通常来讲，使用 instanceof 就是判断一个实例是否属于某种类型
- instanceof 常规用法
// 判断 foo 是否是 Foo 类的实例
```
function Foo(){} 
var foo = new Foo(); 
console.log(foo instanceof Foo)//true
```
另外，更重的一点是 instanceof 可以在继承关系中用来判断一个实例是否属于它的父类型。例如：

- instanceof 在继承中关系中的用法
// 判断 foo 是否是 Foo 类的实例 , 并且是否是其父类型的实例

```
function Aoo(){} 
function Foo(){} 
Foo.prototype = new Aoo();//JavaScript 原型继承
 
var foo = new Foo(); 
console.log(foo instanceof Foo)//true 
console.log(foo instanceof Aoo)//true
```