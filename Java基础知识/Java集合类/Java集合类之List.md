
# 1. List 
**有序**，**可以重复的集合**
由于 List 接口是继承于 Collection 接口，所以基本的方法如上所示。
```
 List list1 = new ArrayList();
底层数据结构是数组，查询快，增删慢;线程不安全，效率高
```
```
List list2 = new Vector();
底层数据结构是数组，查询快，增删慢;线程安全，效率低,几乎已经淘汰了这个集合
```
```
List list3 = new LinkedList();
底层数据结构是链表，查询慢，增删快;线程不安全，效率高
```
# 2. Arrays.asList()方法
- Arrays.asList() 是将数组作为列表

```
public class Test {
    public static void main(String[] args) {
        int[] a = {1,2,3,4};
        List list = Arrays.asList(a);
        System.out.println(list.size());  //1
    }

}
```
期望的输出是 list里面也有4个元素，也就是size为4，然而结果是1.

原因如下：

- 在Arrays.asList中，该方法接受一个变长参数，一般可看做数组参数，但是因为int[] 本身就是一个类型，所以a变量作为参数传递时，编译器认为只传了一个变量，这个变量的类型是int数组，所以size为1，相当于是List中数组的个数
- 基本类型是不能作为泛型的参数，按道理应该使用包装类型，但这里缺没有报错，因为数组是可以泛型化的，所以转换后在list中就有一个类型为int的数组
- 返回一个受指定数组支持的固定大小的列表。（对返回列表的更改会“直写”到数组。）此方法同 Collection.toArray 一起，充当了基于数组的 API 与基于 collection 的 API 之间的桥梁。返回的列表是可序列化的.

- 所以，如果是创建多个列表，在传参数时候，最好使用Arrays.copyOf(a)方法，不然，对列表的更改就相当于对数组的更改

```
public class Test {
    public static void main(String[] args) {
        Integer[] a = {1,2,3,4};
        List list = Arrays.asList(a);
        System.out.println(list.size());  //4
    }

}
```
最后提醒，如果Integer[]数组没有赋值的话，默认是null，而不是像int[]数组默认是0。