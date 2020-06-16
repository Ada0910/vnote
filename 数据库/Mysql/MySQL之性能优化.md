# 1. 优化思路
![](_v_images/20200616221831959_6745.png =916x)
# 2. 连接
## 2.1. 两方面解决连接数不够
- 从服务端来说，可以增加服务端的可用连接数
a.修改配置参数增加可用连接数
```
show VARIABLES like "max_connections"
```
![](_v_images/20200616223643822_16074.png =690x)
b.释放掉不活动的连接
```
SHOW GLOBAL VARIABLES LIKE "wait_timeout"
```
![](_v_images/20200616223819615_8296.png =902x)
- 客户端
可以引入连接池，实现连接的复用
# 3. 缓存
## 3.1. 缓存
## 3.2. 集群，主从复制
## 3.3. 分库分表
# 4. 优化器
## 4.1. 慢查询日志
- 打开慢查询日志开关
```
SHOW  VARIABLES LIKE "slow_query%"
```
![](_v_images/20200616230211278_23529.png =1608x)

```
SHOW  VARIABLES LIKE "long_query%"
```
![](_v_images/20200616230420972_24773.png =944x)

- 查看profile统计
```
show PROFILES;
```
![](_v_images/20200616231325157_25287.png =1260x)

- 分析server层的运行信息
```
SHOW GLOBAL STATUS;
```

- 运行线程（分析服务层的连接信息）
```
SHOW PROCESSLIST;
```