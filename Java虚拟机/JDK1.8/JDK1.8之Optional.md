# 1. JDK1.8之Optional

   Optional类是Java8为了解决null值判断问题，借鉴google guava类库的Optional类而引入的一个同名Optional类，使用Optional类可以避免显式的null值判断（null的防御性检查），避免null导致的NPE（NullPointerException）。
    我们来看一段代码：
```
    public static String getGender(Student student)
    {
        if(null == student)
        {
            return "Unkown";
        }
        return student.getGender();
        
    }
```

这是一个获取学生性别的方法，方法入参为一个Student对象，为了防止student对象为null， 做了防御性检查：如果值为null，返回"Unkown"。
    再看使用Optional优化后的方法：
```
    public static String getGender(Student student)
    {
       return Optional.ofNullable(student).map(u -> u.getGender()).orElse("Unkown");
        
    }
```

# 2. Optional对象的创建

我们看下Optional类的部分源码：

```
   private static final Optional<?> EMPTY = new Optional<>();

   private final T value;

    private Optional() {
        this.value = null;
    }

    public static<T> Optional<T> empty() {
        @SuppressWarnings("unchecked")
        Optional<T> t = (Optional<T>) EMPTY;
        return t;
    }

    private Optional(T value) {
        this.value = Objects.requireNonNull(value);
    }

    public static <T> Optional<T> of(T value) {
        return new Optional<>(value);
    }

    public static <T> Optional<T> ofNullable(T value) {
        return value == null ? empty() : of(value);
    }

```
可以看出，Optional类的两个构造方法都是private型的，因此类外部不能显示的使用new Optional()的方式来创建Optional对象，但是Optional类提供了三个静态方法empty()、of(T value)、ofNullable(T value)来创建Optinal对象，示例如下：
```
// 1、创建一个包装对象值为空的Optional对象
Optional<String> optStr = Optional.empty();
// 2、创建包装对象值非空的Optional对象
Optional<String> optStr1 = Optional.of("optional");
// 3、创建包装对象值允许为空的Optional对象
Optional<String> optStr2 = Optional.ofNullable(null);
```
# 3. 常用接口
## 3.1. get()方法
  简单看下get()方法的源码：
```
    public T get() {
        if (value == null) {
            throw new NoSuchElementException("No value present");
        }
        return value;
    }
```

可以看到，get()方法主要用于返回包装对象的实际值，但是如果包装对象值为null，会抛出NoSuchElementException异常。

## 3.2. isPresent()方法
   isPresent()方法的源码：

```
    public boolean isPresent() {
        return value != null;
    }
```
可以看到，isPresent()方法用于判断包装对象的值是否非空

## 3.3. ifPresent()方法
   ifPresent()方法的源码：
```
    public void ifPresent(Consumer<? super T> consumer) {
        if (value != null)
            consumer.accept(value);
    }
```

ifPresent()方法接受一个Consumer对象（消费函数），如果包装对象的值非空，运行Consumer对象的accept()方法。示例如下：
```
    public static void printName(Student student)
    {
        Optional.ofNullable(student).ifPresent(u ->  System.out.println("The student name is : " + u.getName()));
    }
```

上述示例用于打印学生姓名，由于ifPresent()方法内部做了null值检查，调用前无需担心NPE问题。

## 3.4. filter()方法
     filter()方法的源码：
```
```
```
    public Optional<T> filter(Predicate<? super T> predicate) {
        Objects.requireNonNull(predicate);
        if (!isPresent())
            return this;
        else
            return predicate.test(value) ? this : empty();
    }

```
filter()方法接受参数为Predicate对象，用于对Optional对象进行过滤，如果符合Predicate的条件，返回Optional对象本身，否则返回一个空的Optional对象。举例如下：
```
    public static void filterAge(Student student)
    {
        Optional.ofNullable(student).filter( u -> u.getAge() > 18).ifPresent(u ->  System.out.println("The student age is more than 18."));
    }

```
上述示例中，龄大于18的学生的实现了年筛选。

## 3.5. map()方法
     map()方法的源码：
```
    public<U> Optional<U> map(Function<? super T, ? extends U> mapper) {
        Objects.requireNonNull(mapper);
        if (!isPresent())
            return empty();
        else {
            return Optional.ofNullable(mapper.apply(value));
        }
    }
```

map()方法的参数为Function（函数式接口）对象，map()方法将Optional中的包装对象用Function函数进行运算，并包装成新的Optional对象（包装对象的类型可能改变）。举例如下：
```
    public static Optional<Integer> getAge(Student student)
    {
        return Optional.ofNullable(student).map(u -> u.getAge()); 
    }
```

上述代码中，先用ofNullable()方法构造一个Optional<Student>对象，然后用map()计算学生的年龄，返回Optional<Integer>对象（如果student为null, 返回map()方法返回一个空的Optinal对象）。

## 3.6. flatMap()方法
     flatMap()方法的源码：
```
    public<U> Optional<U> flatMap(Function<? super T, Optional<U>> mapper) {
        Objects.requireNonNull(mapper);
        if (!isPresent())
            return empty();
        else {
            return Objects.requireNonNull(mapper.apply(value));
        }
    }
```

跟map()方法不同的是，入参Function函数的返回值类型为Optional<U>类型，而不是U类型，这样flatMap()能将一个二维的Optional对象映射成一个一维的对象。以3.5中示例功能为例，进行faltMap()改写如下：

```
public static Optional<Integer> getAge(Student student)
  {
  return Optional.ofNullable(student).flatMap(u -> Optional.ofNullable(u.getAge())); 
    }
```


## 3.7. orElse()方法
   orElse()方法的源码：
  
```
  public T orElse(T other) {
        return value != null ? value : other;
    }

```

orElse()方法功能比较简单，即如果包装对象值非空，返回包装对象值，否则返回入参other的值（默认值）。如第一章（简介）中提到的代码：
```
    public static String getGender(Student student)
    {
     
     return Optional.ofNullable(student).map(u -> u.getGender()).orElse("Unkown");
        
    }

```

## 3.8. orElseGet()方法
     orElseGet()方法的源码：
```
    public T orElseGet(Supplier<? extends T> other) {
        return value != null ? value : other.get();
    }
```

orElseGet()方法与orElse()方法类似，区别在于orElseGet()方法的入参为一个Supplier对象，用Supplier对象的get()方法的返回值作为默认值。如：
```
    public static String getGender(Student student)
    {
        return Optional.ofNullable(student).map(u -> u.getGender()).orElseGet(() -> "Unkown");      
    }
```

## 3.9. orElseThrow()方法
     orElseThrow()方法的源码：
```
    public <X extends Throwable> T orElseThrow(Supplier<? extends X> exceptionSupplier) throws X {
        if (value != null) {
            return value;
        } else {
            throw exceptionSupplier.get();
        }
    }
```

orElseThrow()方法其实与orElseGet()方法非常相似了，入参都是Supplier对象，只不过orElseThrow()的Supplier对象必须返回一个Throwable异常，并在orElseThrow()中将异常抛出

```
public static String getGender1(Student student)
    {
        return Optional.ofNullable(student).map(u -> u.getGender()).orElseThrow(() -> new RuntimeException("Unkown"));      
    }
```







