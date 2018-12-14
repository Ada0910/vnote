# 1. Map
key-value 的键值对，key 不允许重复，value 可以

　　严格来说 Map 并不是一个集合，而是两个集合之间 的映射关系。

　　这两个集合没每一条数据通过映射关系，我们可以看成是一条数据。即 Entry(key,value）。Map 可以看成是由多个 Entry 组成。

　　 因为 Map 集合即没有实现于 Collection 接口，也没有实现 Iterable 接口，所以不能对 Map 集合进行 for-each 遍历。

![](_v_images/_1544758510_1057.png)
# 2. 而Map中的每个元素都使用key——>value的形式存储在集合中。

## 2.1. Map集合：该集合存储键值对。一对一对往里存。而且要保证键的唯一性。
```
添加。
put(K key, V value) 
putAll(Map<? extends K,? extends V> m)

删除。
clear() 
remove(Object key)

判断。
containsValue(Object value) 
containsKey(Object key) 
isEmpty()

获取。
get(Object key) 
size() 
values()
entrySet() 
keySet()
```

## 2.2. Map接口的常用子类

Map
|--HashMap：底层是哈希表数据结构，允许使用 null 值和 null 键，该集合是不同步的。将hashtable替代，jdk1.2.效率高。
|--TreeMap：底层是二叉树数据结构。线程不同步。可以用于给map集合中的键进行排序