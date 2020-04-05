# 1. 是什么
ConcurrentHashMap是线程安全且高效的HashMap
# 2. 使用ConcurrentHashMap的原因
- 线程不安全的HashMap
- 效率低下的HashTable
- ConcurrentHashMap的锁分段技术可有效提升并发访问率
# 3. 结构
- 组成
Segment数组+HashEntry数组
- Segment是一种可重入锁(ReentrantLock),结果于HashMap相似(是一种数组和链表结构)
- HashEntry是用于存储键值对数据 
# 4. 初始化 
```
 throws java.io.IOException {
        // For serialization compatibility
        // Emulate segment calculation from previous version of this class
        int sshift = 0;//ssize从1向左移位的次数
        int ssize = 1;
        while (ssize < DEFAULT_CONCURRENCY_LEVEL) {
            ++sshift;
            ssize <<= 1;
        }
        int segmentShift = 32 - sshift;
        int segmentMask = ssize - 1;
```
## 4.1. 初始化segment数组
ConcurrentHashMap初始化方法是通过initialCapacity,loadFactor和concurrencyLevel等这几个参数来初始化segment数组,段偏移量segmentShift,段掩码segmentMask和每个segment里的HashEntry数组来实现
## 4.2. 初始化segmentShift和segmentMask
- segmentShift用于定位参与散列运算的位数
# 5. 操作
## 5.1. get
  - Segment的get的操作实现非常简单和高效
  原因:get方法里的共享变量都是定义成volatile类型(volatile字段的写入操作优于读操作,即使两个线程同时被修改和获取volatile变量,get操作哦也能拿到最新的值,这是volatile替换锁的经典应用场景)
## 5.2. put
## 5.3. size