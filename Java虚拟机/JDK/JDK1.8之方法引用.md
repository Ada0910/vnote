# 1. JDK1.8之方法引用
方法引用使得开发者可以直接引用现存的方法、Java类的构造方法或者实例对象。方法引用和Lambda表达式配合使用，使得java类的构造方法看起来紧凑而简洁，没有很多复杂的模板代码。
西门的例子中，Car类是不同方法引用的例子，可以帮助读者区分四种类型的方法引用。

```
public static class Car {
    public static Car create( final Supplier< Car > supplier ) {
        return supplier.get();
    }              
        
    public static void collide( final Car car ) {
        System.out.println( "Collided " + car.toString() );
    }
        
    public void follow( final Car another ) {
        System.out.println( "Following the " + another.toString() );
    }
        
    public void repair() {   
        System.out.println( "Repaired " + this.toString() );
    }
}

```
## 1.1. 构造器引用
第一种方法引用的类型是构造器引用，语法是Class::new，或者更一般的形式：`Class<T>::new`。注意：这个构造器没有参数。

```
final Car car = Car.create( Car::new );
final List< Car > cars = Arrays.asList( car );
```

## 1.2. 静态方法的引用
第二种方法引用的类型是静态方法引用，语法是`Class::static_method`。注意：这个方法接受一个Car类型的参数。
```
cars.forEach( Car::collide );
```
## 1.3. 成员方法的引用
第三种方法引用的类型是某个类的成员方法的引用，语法是`Class::method`，注意，这个方法没有定义入参：
```
cars.forEach( Car::repair );
```

## 1.4. 实例对象成员方法的引用
第四种方法引用的类型是某个实例对象的成员方法的引用，语法是`instance::method`。注意：这个方法接受一个Car类型的参数：
```
```
```
final Car police = Car.create( Car::new );
cars.forEach( police::follow );
```
