# 1. Collections.sort()
在日常开发中，很多时候都需要对一些数据进行排序的操作。然而那些数据一般都是放在一个集合中如：Map ，Set ，List 等集合中。他们都提共了一个排序方法 sort()，要对数据排序直接使用这个方法就行，但是要保证集合中的对象是 可比较的。

怎么让一个对象是 可比较的，那就需要该对象实现 Comparable<T> 接口啦。然后重写里面的 
compareTo()方法。我们可以看到Java中很多类都是实现类这个接口的 如：Integer，Long 等等。。。
## 1.1. 第一种： Comparable 排序接口
 若一个类实现了Comparable接口，就意味着“该类支持排序”。 假设“有一个List列表(或数组)，里面的元素是实现了Comparable接口的类”，则该List列表(或数组)可以通过 Collections.sort（或 Arrays.sort）进行排序。

此外，“实现Comparable接口的类的对象”可以用作“有序映射(如TreeMap)”中的键或“有序集合(TreeSet)”中的元素，而不需要指定比较器。

## 1.2. 第二种：Comparator比较器接口
我们若需要控制某个类的次序，而该类本身不支持排序(即没有实现Comparable接口)；我们可以建立一个“比较器”来进行排序。这个“比较器”只需要实现Comparator接口即可。

Collections.sort(list, new PriceComparator())

参数一：需要排序的list
参数二：比较器，实现Comparator接口的类，返回一个int型的值，就相当于一个标志，告诉sort方法按什么顺序来对list进行排序。
Comparator是个接口，可重写compare()及equals()这两个方法,用于比较功能；如果是null的话，就是使用元素的默认顺序，如a,b,c,d,e,f,g，就是a,b,c,d,e,f,g这样，当然数字也是这样的。

compare（a,b）方法:根据第一个参数小于、等于或大于第二个参数分别返回负整数、零或正整数。
equals（obj）方法：仅当指定的对象也是一个 Comparator，并且强行实施与此 Comparator 相同的排序时才返回 true
