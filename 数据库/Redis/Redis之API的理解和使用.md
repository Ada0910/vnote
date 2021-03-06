# 1. 预备
## 1.1. 全局命令
- 查看所有键值对
```
keys * 查看所有的键值对
```
![](_v_images/_1565014811_28014.png)
![](_v_images/_1565014829_22243.png)

- 键总数,返回当前数据库的键值对总数
```
dbsize
```
![](_v_images/_1565014987_32457.png)

- 检查键是否存在
```
exists key
```
![](_v_images/_1565015158_31925.png)

-  删除键
```
del key{key ......}返回删除的个数 ，否则就返回0
```
![](_v_images/_1565015372_31430.png)

- 设置键过期
```
expire key value
ttl 可以返回剩余的过期时间
当键没有设置过期时间的时候：ttl return -1
当键不存在时，return -2；
```
![](_v_images/_1565015597_20230.png)
![](_v_images/_1565015645_1312.png)

- 查看键的数据类型
```
type key
```
![](_v_images/_1565015743_12211.png)


## 1.2. 数据结构和内部编码

![](_v_images/_1565016153_29095.png)

## 1.3. 为什么单线程还这么快

- 纯内存操作
- 非阻塞I/O，Redis使用epoll作为I/O多路复用技术的实现
- 既然是单线程，那么久避免了线程之间的切换和竞态产生的消耗

# 2. 字符串
## 2.1. 命令
- 设置值
```
set key value [ex seconds][px milliseconds][nx | xx]
ex seconds:键设置过期时间（秒）
px milliseconds:键设置过期时间（毫秒）
nx:必须不存在才能设置成功（添加）
xx:必须存在才能设置成功（更新）
setex:
setnx:
```
- 获取值
```
get key
```
![](_v_images/_1565018555_22744.png)

- 批量设置值
```
mset key value [key value .....]
```
![](_v_images/_1565018657_17707.png)

- 批量获取值
```
mget key ....
```
![](_v_images/_1565018726_23658.png)

- 计数
```
incr key 用于自增
decr（自减）、incrby（加自定数字）、decrby、incrbyfloat（自增浮点数）
```
![](_v_images/_1565018872_31795.png)

- 追加值
```
append key value
```
![](_v_images/_1565019090_15801.png)

- 字符串的长度
```
strlen key
```
![](_v_images/_1565019147_25146.png)
- 设置并返回原值
```
getset key value 
```
![](_v_images/_1565019200_5743.png)
- 设置指定位置的字符
```
setrange key offeset value 
```
![](_v_images/_1565019272_15830.png)
- 获取部分字符串
```
getrange key start end
```
![](_v_images/_1565019333_16382.png)
## 2.2. 内部编码
- int ：8个字节的长整型
- embstr：小于等于39个字节的字符串
- raw：大于39个字节的字符串
## 2.3. 使用场景
- 缓存功能
- 计数
- 共享session
- 限速

# 3. 哈希
## 3.1. 命令
- 设置值
```
hset key xxx  xxx
```
- 获取值
```
hget key  xxx
```
![](_v_images/_1565101726_968.png)


- 删除field
```
hdel key field [field ...]
```
![](_v_images/_1565102146_7189.png)
- 计算field
```
hlen key 
```
![](_v_images/_1565102373_27838.png)
- 批量设置或者获取field-value
```
hmget key field [...] 
hmset key value [...]
```
- 判断field是否存在
```
hexitsts key field 
```
![](_v_images/_1565102491_14146.png)
- 获取所有的field
```
hkeys key
```
- 获取所有的value
```
hvals key 
```
![](_v_images/_1565102709_18092.png)
- 获取所有的field-value
```
hgetall key
```
![](_v_images/_1565102734_7742.png)
- hincrby &&hincrbyfloat
```
hincrby key field
hincrbyfloat key field 
```
- 计算value的字符串长度(3.2以上支持)
```
hstrlen key field
```
## 3.2. 内部编码
- ziplist (压缩列表)
- hashtable(哈希表)
## 3.3. 使用情景

# 4. 列表
## 4.1. 命令
### 4.1.1. 添加操作
- 从右边插入元素
```
rpush key value [value ...]
```
- 从左至右获取所有元素
```
lrange listkey 0 -1
```
![](_v_images/20190807144958713_19730.png)

- 从左边出入元素，将rpush换成lpush
```
lpush key value [value ...]
```
- 向某个元素前或者后插入元素
```
linsert key before | after pivot value
```
![](_v_images/20190807145549625_5576.png)

### 4.1.2. 查找
- 获取指定范围内的元素列表
```
lrange key start end 
```
![](_v_images/20190807145842473_26164.png)

- 获取列表指定下标的元素
```
lindex key index
```
![](_v_images/20190807145938889_24952.png)

- 获取长度
```
llen key
```
![](_v_images/20190807150415960_9603.png)

### 4.1.3. 删除
- 从列表左侧弹出元素
```
lpop key
```
![](_v_images/20190807151513736_1420.png)

- 从列表右侧弹出
```
rpop key 
```

- 删除指定元素
```
lrem key count value
```
![](_v_images/20190807152142120_14310.png)

- 按照索引范围修剪列表
```
ltrim key start end 
```
![](_v_images/20190807152520408_8539.png)

![](_v_images/20190807152705952_14078.png)

### 4.1.4. 修改
- 修改指定索引下标的元素
```
lset key index newValue
```
![](_v_images/20190807154559671_12389.png)

### 4.1.5. 阻塞操作
- blpop
```
blpop key [key ....] timeout
```

- brpop 
```
brpop key [key ...] timeout
```
## 4.2. 内部编码规则
- ziplist(压缩列表)：
- linkedlist（链表）:
查看编码规则：
```
object encoding key 
```
## 4.3. 使用场景
## 4.4. 使用情景
- 消息队列
- 文章列表
- 开发选择
```
·lpush+lpop=Stack（栈）
·lpush+rpop=Queue（队列）
·lpsh+ltrim=Capped Collection（有限集合）
·lpush+brpop=Message Queue（消息队列）
```
# 5. 集合
## 5.1. 命令
### 5.1.1. 集合内操作
- 添加元素
```
sadd key element[...]
```
![](_v_images/_1565187081_25303.png)

- 删除元素
```
srem key element[...]
```
![](_v_images/_1565187192_21829.png)
- 计算元素个数
```
scard key
```
![](_v_images/_1565187231_12164.png)
- 判断元素是否在集合内
```
sismember key element
```
![](_v_images/_1565187299_16336.png)
- 随机从集合返回指定个数元素
```
srandmember key [count]
```
![](_v_images/_1565187445_26306.png)
- 从集合随机弹出元素
```
 spop key
```
![](_v_images/_1565187581_2968.png)
- 获取所有元素
```
smembers key
```
![](_v_images/_1565187623_11395.png)
### 5.1.2. 集合间操作
- 交集
```
sinter key [...]
```
![](_v_images/_1565188223_17968.png)
- 并集
```
sunion key [....]
```
![](_v_images/_1565188240_17356.png)
- 差集
```
sdiff key [...]
```
![](_v_images/_1565188269_27457.png)
- 保存
```
sinterstore destination key [...]
sunionstore destination key [...]
sdiffstore destination  key [...] 
```
## 5.2. 内部编码
- intset (整数集合)：
- hashtable(哈希表):
## 5.3. 使用情景
- 标签
# 6. 有序集合
## 6.1. 命令
### 6.1.1. 集合内
- 添加成员
```
zadd key score member[score member ....]
```
- 计算成员
```
zcard key
```
![](_v_images/_1565796099_3405.png)

- 计算某个成员的分数
```
zscore key member
```
- 排名
```
升序：zrank key member
降序：zrevrank key member
```
- 删除
```
zrem key member [key member....]
```
![](_v_images/_1565796432_3601.png)
- 增加成员分数
```
zincrby key 分数 member
```

![](_v_images/_1565796951_27799.png)
- 返回指定排名范围的成员
```
zrange key start end [withscores]
zrevrange key start end [withscores]
```
![](_v_images/_1565797110_2656.png)
- 返回指定分数范围的成员
```
zrangebyscore key min max [withscores] [limit offset count]
zrevrangebyscore ......
```

![](_v_images/_1565797156_17982.png)
- 返回指定分数范围的成员个数
```
zcount key min max 
```
![](_v_images/_1565797276_25124.png)
- 删除指定排名内的升序元素
```
zremrangebyrank key start end (删除排名内)
zremrangebuscoure key min max (删除多少分内)
```
### 
- 交集
```
zinterstore destination numkeys key [key ....][weights [weight...]] [aggregate sum | min | max]
```
- 并集
```
zunionstore destination numkeys key [key ....][weights [weight...]] [aggregate sum | min | max]

```
## 6.2. 内部编码
- ziplist
- skiplist 
## 6.3. 使用情景
- 添加用户赞数
- 取消用户赞数
- 展示赞数最多的十个用户
- 展示用户信息以及分数
# 7. 键管理
## 7.1. 单个键
- 键命名

```
rename key newkey
```
- 随机返回一个键

```
randomkey
```
- 键过期

```
expire key seconds :
expireat key timestamp;
persist 可以将键的过期时间清除
```
- 迁移键
```
(1)move
(2)dump+restore
(3)migrate
```
## 7.2. 遍历键
- 全局遍历键
```
keys pattern
```
- 渐进式遍历
```
scan cursor [match pattern][count number]
```
## 7.3. 数据库管理
- 切换数据库
```
select xxx
```
- flushdb/flushall
```
flushdb 删除当前的
flushall 删除全部
```