# 1. JAVA得到泛型中的T.class
```
Class <T> entityClass = (Class <T>) ((ParameterizedType) getClass().getGenericSuperclass()).getActualTypeArguments()[0]; 
```

- getGenericInterfaces()和getGenericSuperclass()

 - getInterfaces()和getSuperclass()

先来看看这两个方法都是干什么用的：
# 2. public Type getGenericSuperclass()

用来返回表示当前Class 所表示的实体（类、接口、基本类型或 void）的直接超类的Type。如果这个直接超类是参数化类型的，则返回的Type对象必须明确反映在源代码中声明时使用的类型。比如：

```

import java.lang.reflect.ParameterizedType;  
public class GT1 extends GT2<Integer>{  
    public static void main(String[] args) {  
        System.out.println(((ParameterizedType)new GT1().getClass().getGenericSuperclass()));  
    }  
}  
则输出结果即为：
```

```
GT2<java.lang.Integer>
```
如果此Class代表的是Object 类、接口、基本类型或 void，则返回 null。。如果此对象表示一个数组类，则返回表示 Object 类的 Class 对象。

# 3. public Type[] getGenericInterfaces()

  与上面那个方法类似，只不过Java的类可以实现多个接口，所以返回的Type必须用数组来存储。

 以上两个方法返回的都是Type对象或数组，在我们的这个话题中，Class都是代表的参数化类型，因此可以将Type对象Cast成ParameterizedType对象。而 ParameterizedType对象有一个方法， getActualTypeArguments()。
# 4. public Type[] getActualTypeArguments()

用来返回一个Type对象数组，这个数组代表着这个Type声明中实际使用的类型。接着使用上面的例子：

```
import java.lang.reflect.ParameterizedType;  
public class GT1 extends GT2<Integer>{  
    public static void main(String[] args) {  
    System.out.println(((ParameterizedType)new GT1().getClass().getGenericSuperclass()).getActualTypeArguments()[0]);  
    }  
}  

这次的显示结果将是：class java.lang.Integer
```
因此，我们可以通过继承+反射的方法，来的到T.class。
 需要说明的是，江南白衣使用的方法是将关键语句
 
```
Class < T >  entityClass  =  (Class < T > ) ((ParameterizedType) getClass().getGenericSuperclass()).getActualTypeArguments()[ 0 ];
```

放在了超类，也就是声明泛型的那个类的构造方法中。这样一来，子类在继承具有泛型的超类时，会自动调用超类的构造方法。在此超类的构造方法中，调用的getClass返回的是子类的Class类型（与通常的重写机制有悖，呵呵，有待深究，但测试结果确是如此），则在子类中就无需再显式地使用getGenericInterfaces()和getGenericSuperclass()等方法了。

 接着，再使用(Class<T>)对 getActualTypeArguments()返回的元素做casting，即可得到所谓的T.class。