#### 键相关操作

```sql
-- 查看当前redis数据库中包含的所有键值
keys *
-- 判断某个键是否存在(没有显示0)
exists <key>
-- 查看键的类型
type <key>
-- 删除键
del <key>
-- 为键值设置过期时间单位为秒
expire <key><seconds>
-- 查看键值手否过期 (-2 代表已经过期 -1 代表永不过期)
ttl <key>
-- 查看当前数据库键的数量
dbsize
```

#### string类型相关操作

```sql
-- 新增一个字符串
set key1 value1
--根据键获取值
get key1
-- 追加值
append key value
--查询值的长度
strlen key
-- 数据库中没有key键，数据库才创建。有的话不改变原来的值
setnx <key>
-- 为数字型字符串+1
incr <key>
-- 为数字型字符串-1
decr <key>
--自定义加与减
incrby/decrby <key> 步长
-- 截取字符串长度
getrange <key> <起始位置><结束位置>   //开始与结束包含
-- 覆写指定位置的字符串值
set range key start <value>
-- 同时设置一个或多个key-value
mset key1 value1 key2 value2 key3 value3......
-- 同时获取一个或者多个值
mget key1 key2......
-- 数据库中没有key键，数据库才创建。有的话不改变原来的值
msetnx <key>
-- s设定值的同时设置过期时间
setex key 过期时间 value
```

#### set类型相关操作

```sql
-- 添加元素
sadd key value1 value2.....
-- 取出该集合中所有的值
smembers key
-- 判断key集合中是否有value值 没有返回0
sismember key value
-- 随机取出集合的一个元素
spop key
-- 随机取出集合的n个元素
srandmember key n
-- 返回两个集合的并集元素
sunion key1 key2
-- 返回两个集合的交集元素
sinter key1 key2
-- 返回两个集合的差集元素
sdiff key1 key2
```

#### list类型相关操作

```sql
-- 添加操作
lpush/rpush key value1 value2......
-- 删除list集合中的左右一个元素
lpop/rpop key 
-- 查询集合中的元素
lrange key 开始位置 结束位置       -- 通常查询全部为【0，-1】
-- 在value的前后插入值
linsert key before/after value newvalue
-- 从左边开始删除n个value
lrem key n value
注意： n为0代表删除所有
	  n为1 代表从左到右删除1个
	  n为-1 代表从右到左删除1个
```

#### hash类型相关操作

```sql
-- 添加键值对
hset key field value
hmset key1 field1 value1 field2 value2......
-- 从hash中能够取出值
hget key field
-- 查看哈希表key中是否有field存在
hexists key field
-- 列出所有的field 
hkeys key
-- 列出所有的value
hvals key
-- 为哈希表key中的域field的值增加增量increment( increment 为数字类型)
hincrby key field increment
-- 将哈希表key中的域字段field的值设置为value
hsetnx key field value

```

#### zset类型相关操作

```sql
-- 添加排序元素
zadd key score1 value1 score2 value2
-- 获取数据 WITHSCORES 代表带分数返回结果集
zrange key start stop [WITHSCORES]
-- 返回有序集key中所有score值鉴于min和max之间包括min和max的成员。有序集成员按score 值传递从小到大排列
zrangebyscore key min max [withscores][limit offset count]
-- 在上一个的基础上从大到小排列
zrevrangebyscore key max min [withscores][limit offset count] 
-- 为元素的score加上增量
zincrby key increment value
-- 删除key中的值
zrem key value
-- 统计该集合，分数区间内的元素个数
zcount key min  max
-- 返回该值在集合中的排名，从0开始。
zrank key value
```

#### Redis 配置文件说明

```sql
1、include
	可以将其他配置文件包含进来。与jsp的include类似
2.ip地址的绑定 bind
   bind 127.0.0.1     //只允许本机访问
如果不写绑定可以无限制接受任何的ip地址
protected-mode true
如果开启了protected-mode，那么在没有bind ip的情况下，Redis只允许接受本机的相应。
3.tcp-backlog
一个请求到达后至到接受处理前的队列backlog队列总和=未完成三册握手队列+已经完成三次握手队列

高并发环境tcp-backlog 设置跟超时时间内的redis吞吐量
4.timeout
	一个空闲的客户端维持多少秒会关闭，0为永不关闭
5.TCP keepalive
	对访问客户端的一种心跳检测，每隔n秒检测一次。官方推荐设为60秒
6.daemonize
	是为后台进程
7.pidfile
	存放pid文件的位置，每隔实例产生一个不同的pid的文件
8.logfile
	日志文件名称
9.loglevel
	日志级别
10 密码设置
设置临时密码
config get requirepass
config set requirepass "123456"
-- 登录
auth 123456
永久密码设置
配置文件找到 requirepss 进行设置

11.maxclient
最大客户端连接数
12.maxmemeory.
设置redis可以使用的内存量。一旦到达内存使用上限，Redis将会试图移除内部数据，移除规则可以通过maxmemory-policy来指定。
如果Redis无法根据移除规则来移除内存中的数据，或者设置了不移出那么redis会针对那些需要申请内存的指令返回错误信息。比如SET、LPUSH等。
maxmemory-policy
	12.1volatile-lru:使用LRU（最近最少使用）算法移除key，只对设置过期时间的键
	12.2 allkeys-lru:使用LRU算法移除key
	12.3 volatile-random:在过期集合中移除随机的key，只对设置过期时间的键
	12.4 allkeys-random:随机移除key
	12.5 volatile-ttl（即将过期）：移除那些TTL最小的key，那些块过期的key
	12.6 noeviction：不移出。只针对写操作，只是返回错误信息
	13 Maxmemory samples
	设置样本数量，LRU算法和最小TTL算法都是非常精确的算法，而是估算值，所以你可以设置样本的大小。一般设置3到7的数字，数值越小样本越不准确，但是性能消耗也越小
```

#### redis事务

```sql
redis事务主要是一个单独的隔离操作：事务中的所有命令都会序列化、按顺序地执行。事务在执行的过程中，不会被其他客户端发送来的命令请求所打断。（批量执行指令的功能）

redis中事务特点
	1.单独的隔离操作
		事务中所有的命令都会序列化、按顺序地执行。事务在执行过程中，不会被其他客户端发送来的命令请求所打断。
	2.没有隔离级别的概念
		队列中的命令没有提交之前都不会实际的执行，因为事务提交前任何指令都不会被实际执行，也就不存在事务的查询看到事务的更新。
	3.不保证原子性
		3.1.如果发生语法错误，那么redis会回滚；
		3.2.如果发生执行错误，那么redis不会回滚；
		3.3 如果watch key key值发生变化其他事务都会回滚。
redis事务关键命令
	1.multi   -- 开启事务
	2.exec    -- 执行
	3.discard -- 回滚
```

#### 主从复制



```sql
主从复制就是主机数据更新后根据配置和策略，自动同步到备机的master/slqver机制，Master以写为主，slave以读为主。

配置原则以及配置事项
	配置从服务器不配置主服务器
	.拷贝多个redis.conf文件include
	.开启daemonize yes(后台进程)
	.Pid文件名字pidfile
	.指定端口port
	.log文件名字
	.dump.rdb名字dbfilename
	.Appendonly 关掉或者替换名字
	
-- 打印主从复制的相关信息
info replication
成为某个实例的从服务器
slaveof ip port
-- 哨兵模式
redis主从关系中。slave监测master是否down机。选择另外的salve为master.
***配置哨兵模式***
自定义sentinel.conf文件
在配置中填写
sentinel monitor 监视的master名字 ip port numofsentinel
reids-sentinel  sentinel.conf(路径)
-- redis使用ruby搭建集群
cluster-enabled 打开集群模式
clusrer-config-file node-port.conf 设置节点配置文件
cluster-node-time 时间    设定节点失联时间，超过改时间（毫秒），集群自动进行主从切换

-- 创建集群
./redis-trib.rb create --replics 从redisd的个数 ip1:port1 ip2:port2......
--  查看节点信息
cluster nodes
-- 集群中录入值
1.查询是 redis-clid  -c
2.不在一个slot下的键，是不能用mget与mset等多键操作。
2.2可以通过{}定义组的概念，从而是key中{}的键值对放在一个slot中
3计算键应该放在哪个slot上
	cluster keyslot key
4.返回slot目前包含的键值对数量。
cluster countkeysinslot slot
5.返回count个slot槽中的键。
	cluster getkeysinslot slot count
```



## 性质

原子性

​	所谓的原子操作指的是不会被线程调度机制打断的操作；这种操作一旦开始就会一直运行到结束，中间不会有任何context switch切换到另一个线程。

1.在单线程中，能够在单条指令中完成的操作都可以认为是原子操作。因为中断只能发生于指令之间。

2.在多线程中，不被其它进程（线程）打断的操作叫原子操作。  