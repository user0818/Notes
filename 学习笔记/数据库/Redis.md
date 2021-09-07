Redis（==Re==mote ==Di==ctionary ==S==erver )，即远程字典服务，是一个开源的使用ANSI C语言编写、支持网络、可基于内存亦可持久化的日志型、Key-Value数据库，并提供多种语言的API。

redis的使用场景：

- 数据库
- 缓存
- 消息中间件

redis支持的数据结构：

-  字符串（strings）
- 散列（hashes）
- 列表（lists）
- 集合（sets）
- 有序集合（sorted sets）与范围查询
-  bitmaps
- hyperloglogs 地理空间（geospatial） 索引半径查询

# 入门

## 跑起来

> 安装

略

> 设置默认后台启动

默认redis并不是作为守护进程启动，需要把daemonize设为yes来开启后台启动。

![image-20210510211333276](https://gitee.com/mw515031/image/raw/master/image/image-20210510211333276.png)

> 启动服务

启动时要指定从哪个配置文件开始启动redis

启动命令在bin文件夹下

```shell
[root@localhost bin]# pwd
/usr/local/bin

[root@localhost bin]# ll
总用量 7048
-rwxr-xr-x. 1 root root  821224 5月  10 20:41 redis-benchmark
lrwxrwxrwx. 1 root root      12 5月  10 20:41 redis-check-aof -> redis-server
lrwxrwxrwx. 1 root root      12 5月  10 20:41 redis-check-rdb -> redis-server
-rwxr-xr-x. 1 root root  990184 5月  10 20:41 redis-cli
drwxr-xr-x. 2 root root      24 5月  10 20:59 redis-config
lrwxrwxrwx. 1 root root      12 5月  10 20:41 redis-sentinel -> redis-server
-rwxr-xr-x. 1 root root 5400856 5月  10 20:41 redis-server

[root@localhost bin]# redis-server redis-config/redis.conf
```

> 启动客户端

默认本机启动，所以只需要`-p`里指定端口号。

如果不是连接本机，那么就需要`-h`来指定主机

```shell
[root@localhost bin]# redis-cli -p 6379
```

> 关闭redis服务

```shel
127.0.0.1:6379> shutdown
not connected> exit
[root@localhost bin]#
```

注意这里关闭的是服务端，要想再关闭客户端还要执行`exit`命令

## 远程连接

要想实现远程连接redis服务器，需要对服务器的配置文件进行两个配置。

1.  bind ip

redis配置文件中有一个bind选项，它表示了==用户可以通过哪些ip地址访问到本服务器（也就是服务器ip）==，默认是127.0.0.1，代表只能通过本机访问本地的redis数据库。

比如服务器上有两个网卡，ip1和ip2，如果bind绑定了ip1的IP地址，那么外界想要访问redis服务器时只能通过ip1来访问。即使ip2也是redis服务器的网卡，但是外界并不能通过该ip访问redis服务。

当`bind 127.0.0.1`时，只有本机能够访问redis服务器，这样是十分安全的，即使不使用密码。

如果将bind注释掉，那么将会允许所有主机都能访问redis，这时如果没有一些限制将会发生安全隐患。所以将会进入protected-mode

2. 保护模式（protected-mode）

- 如果protected-mode处于yes状态，那么在以下==两种情况都不满足==时，将会开启保护模式，即拒绝外部连接。
  - 未bind任何ip地址
  - 未设置密码

- 如果protected-mode处于no状态，那么就用永远不会进入保护模式（不安全）

3. 注意开启linux的防火墙对应端口（6379）

> 如何解决上述问题？

- bind本地ip或者设置密码
- 设置配置文件`protected-mode no`
- 使用--protected-mode no启动服务器

## 性能测试

redis官方自带性能测试工具：redis-benchmark

![image-20210510212611698](https://gitee.com/mw515031/image/raw/master/image/image-20210510212611698.png)

**实例：**

```shell
redis-benchmark -h 127.0.0.1 -p 6379 -c 100 -n 10000
====== PING_INLINE ======                                         
  10000 requests completed in 0.16 seconds
  100 parallel clients
  3 bytes payload
  keep alive: 1					#处理的服务器个数
  host configuration "save": 3600 1 300 100 60 10000
  host configuration "appendonly": no
  multi-thread: no

Latency by percentile distribution:
0.000% <= 0.183 milliseconds (cumulative count 1)
50.000% <= 0.767 milliseconds (cumulative count 5126)
75.000% <= 0.879 milliseconds (cumulative count 7577)
87.500% <= 1.039 milliseconds (cumulative count 8772)
93.750% <= 1.335 milliseconds (cumulative count 9375)
96.875% <= 1.543 milliseconds (cumulative count 9688)
98.438% <= 1.767 milliseconds (cumulative count 9845)
99.219% <= 1.967 milliseconds (cumulative count 9923)
99.609% <= 2.207 milliseconds (cumulative count 9961)
99.805% <= 2.511 milliseconds (cumulative count 9981)
99.902% <= 2.783 milliseconds (cumulative count 9991)
99.951% <= 2.887 milliseconds (cumulative count 9996)
99.976% <= 2.935 milliseconds (cumulative count 9998)
99.988% <= 2.959 milliseconds (cumulative count 9999)
99.994% <= 3.031 milliseconds (cumulative count 10000)
100.000% <= 3.031 milliseconds (cumulative count 10000)

Cumulative distribution of latencies:
0.000% <= 0.103 milliseconds (cumulative count 0)
0.020% <= 0.207 milliseconds (cumulative count 2)
0.110% <= 0.303 milliseconds (cumulative count 11)
0.310% <= 0.407 milliseconds (cumulative count 31)
1.460% <= 0.503 milliseconds (cumulative count 146)
7.200% <= 0.607 milliseconds (cumulative count 720)
33.260% <= 0.703 milliseconds (cumulative count 3326)
62.580% <= 0.807 milliseconds (cumulative count 6258)
78.920% <= 0.903 milliseconds (cumulative count 7892)
86.370% <= 1.007 milliseconds (cumulative count 8637)
89.510% <= 1.103 milliseconds (cumulative count 8951)
91.540% <= 1.207 milliseconds (cumulative count 9154)
93.140% <= 1.303 milliseconds (cumulative count 9314)
94.890% <= 1.407 milliseconds (cumulative count 9489)
96.380% <= 1.503 milliseconds (cumulative count 9638)
97.430% <= 1.607 milliseconds (cumulative count 9743)
98.120% <= 1.703 milliseconds (cumulative count 9812)
98.630% <= 1.807 milliseconds (cumulative count 9863)
99.070% <= 1.903 milliseconds (cumulative count 9907)
99.310% <= 2.007 milliseconds (cumulative count 9931)
99.480% <= 2.103 milliseconds (cumulative count 9948)
100.000% <= 3.103 milliseconds (cumulative count 10000)

Summary:
  throughput summary: 64516.13 requests per second
  latency summary (msec):
          avg       min       p50       p95       p99       max
        0.830     0.176     0.767     1.415     1.887     3.031
...
```

加上`-q`可以只显示结果

```bash
[root@localhost bin]# redis-benchmark -h 127.0.0.1 -p 6379 -c 100 -n 10000 -q
PING_INLINE: 62111.80 requests per second, p50=0.759 msec
PING_MBULK: 59171.60 requests per second, p50=0.791 msec
SET: 62893.08 requests per second, p50=0.743 msec
GET: 62500.00 requests per second, p50=0.759 msec
INCR: 62111.80 requests per second, p50=0.759 msec
LPUSH: 62500.00 requests per second, p50=0.735 msec
RPUSH: 65359.48 requests per second, p50=0.727 msec
LPOP: 61349.69 requests per second, p50=0.783 msec
RPOP: 58823.53 requests per second, p50=0.799 msec
SADD: 62111.80 requests per second, p50=0.759 msec
HSET: 59880.24 requests per second, p50=0.775 msec
SPOP: 64516.13 requests per second, p50=0.743 msec
ZADD: 64935.07 requests per second, p50=0.759 msec
ZPOPMIN: 62893.08 requests per second, p50=0.759 msec
LPUSH (needed to benchmark LRANGE): 60975.61 requests per second, p50=0.767 msec
LRANGE_100 (first 100 elements): 35587.19 requests per second, p50=1.335 msec
LRANGE_300 (first 300 elements): 16863.41 requests per second, p50=2.927 msec
LRANGE_500 (first 500 elements): 11862.40 requests per second, p50=4.159 msec
LRANGE_600 (first 600 elements): 10570.83 requests per second, p50=4.751 msec
MSET (10 keys): 62893.08 requests per second, p50=0.751 msec
```

## 基础知识

> 切换数据库

redis共有16个数据库，默认使用的是第0个

![image-20210510221008177](https://gitee.com/mw515031/image/raw/master/image/image-20210510221008177.png)

可以使用select进行切换数据库

```shell
127.0.0.1:6379> select 3
OK
127.0.0.1:6379[3]> dbsize
(integer) 0
```

> 常用命令

```shell
set key value
get key
del key
type key		#查看value类型
keys *			#查看所有kye
flushdb			#清空当前数据库
flushall		#清空全部数据库
exists	key		#某一key是否存在
move key 1		#把某一key移动到1号数据库
expire	key 10	#设置某一key的过期时间，单位秒
ttl	key			#查看某一key的剩余生存时间
```

# 五大数据类型

http://www.redis.cn/commands.html

# 三种特殊数据类型

## geospatial

redis可以储存地里位置信息，并对其进行操作。

> GEOADD	添加地理位置

将指定的地理空间位置（纬度、经度、名称）添加到指定的key中。这些数据将会存储到sorted set这样的目的是为了方便使用GEORADIUS或者GEORADIUSBYMEMBER命令对数据进行半径查询等操作。

该命令以采用标准格式的参数x,y,所以经度必须在纬度之前。这些坐标的限制是可以被编入索引的，区域面积可以很接近极点但是不能索引。具体的限制，由EPSG:900913 / EPSG:3785 / OSGEO:41001 规定如下：

有效的经度从-180度到180度。

有效的纬度从-85.05112878度到85.05112878度。
当坐标位置超出上述指定范围时，该命令将会返回一个错误。

**时间复杂度：**每一个元素添加是O(log(N)) ，N是sorted set的元素数量。

```bash
#给china:city变量添加城市，格式为：经度，纬度，名称
127.0.0.1:6379> geoadd china:city 116.40 39.90 beijing
(integer) 1
127.0.0.1:6379> geoadd china:city 121.48 31.22 shanghai
(integer) 1
127.0.0.1:6379> geoadd china:city 102.73 25.04 kunming
(integer) 1
```



> GEODIST	返回两个给定位置之间的距离

如果两个位置之间的其中一个不存在， 那么命令返回空值。

指定单位的参数 unit 必须是以下单位的其中一个：

- **m** 表示单位为米。
- **km** 表示单位为千米。
- **mi** 表示单位为英里。
- **ft** 表示单位为英尺。

如果用户没有显式地指定单位参数， 那么 `GEODIST` 默认使用米作为单位。

`GEODIST` 命令在计算距离时会假设地球为完美的球形， 在极限情况下， 这一假设最大会造成 0.5% 的误差。

```bash
127.0.0.1:6379> GEODIST china:city beijing shanghai
"1068781.6759"
127.0.0.1:6379> GEODIST china:city beijing shanghai km
"1068.7817"
```

> GEOHASH	返回一个或多个位置元素的 Geohash 表示

将二维的经纬度，转换为一维的字符串

```
127.0.0.1:6379> GEOHASH china:city beijing shanghai kunming
1) "wx4fbxxfke0"
2) "wtw3s77j9j0"
3) "wk3n3wmsw60"
```

> GEOPOS	返回指定城市的经纬度

```bash
127.0.0.1:6379> GEOPOS china:city beijing shanghai
1) 1) "116.39999896287918091"
   2) "39.90000009167092543"
2) 1) "121.48000091314315796"
   2) "31.21999956478423854"
```

> GEORADIUS	以给定的经纬度为中心， 返回键包含的位置元素当中， 与中心的距离不超过给定最大距离的所有位置元素。

范围可以使用以下其中一个单位：

- **m** 表示单位为米。
- **km** 表示单位为千米。
- **mi** 表示单位为英里。
- **ft** 表示单位为英尺。

在给定以下可选项时， 命令会返回额外的信息：

- `WITHDIST`: 在返回位置元素的同时， 将位置元素与中心之间的距离也一并返回。 距离的单位和用户给定的范围单位保持一致。
- `WITHCOORD`: 将位置元素的经度和维度也一并返回。

命令默认返回未排序的位置元素。 通过以下两个参数， 用户可以指定被返回位置元素的排序方式：

- `ASC`: 根据中心的位置， 按照从近到远的方式返回位置元素。
- `DESC`: 根据中心的位置， 按照从远到近的方式返回位置元素。

在默认情况下， GEORADIUS 命令会返回所有匹配的位置元素。 虽然用户可以使用 **COUNT `<count>`** 选项去获取前 N 个匹配元素， 但是因为命令在内部可能会需要对所有被匹配的元素进行处理， 所以在对一个非常大的区域进行搜索时， 即使只使用 `COUNT` 选项去获取少量元素， 命令的执行速度也可能会非常慢。 但是从另一方面来说， 使用 `COUNT` 选项去减少需要返回的元素数量， 对于减少带宽来说仍然是非常有用的。

> GEORADIUSBYMEMBER	指定成员的位置被用作查询的中心。

这个命令和 `GEORADIUS` 命令一样， 都可以找出位于指定范围内的元素， 但是 `GEORADIUSBYMEMBER` 的中心点是由给定的位置元素决定的， 而不是像 `GEORADIUS` 那样， 使用输入的经度和纬度来决定中心点

```bash
127.0.0.1:6379> GEORADIUSBYMEMBER china:city beijing 2000 km
1) "shanghai"
2) "beijing"
```

==GEO的底层实现原理是Zset，可以使用Zset命令来操作geo==

## HyperLogLog（有误差）

> **基数：**一个集合中出现过的元素的个数
>
> 比如数据集 {1, 3, 5, 7, 5, 7, 8}， 那么这个数据集的基数集为 {1, 3, 5 ,7, 8}, 基数(不重复元素)为5。 基数估计就是在==误差可接受的范围内==，快速计算基数。

Redis HyperLogLog 是用来做基数统计的算法，HyperLogLog 的优点是，在输入元素的数量或者体积非常非常大时，计算基数所需的空间总是固定很小的。

在 Redis 里面，每个 HyperLogLog 键只需要花费 12 KB 内存，就可以计算接近 2^64 个不同元素的基数。因为HyperLogLog 只会根据输入元素来计算基数，而不会储存输入元素本身。

**应用场景**

计算网站每天访问的独立IP数量。如果使用 set 集合存储每个 ip 的话，1个IP就需要占15个字节（xxx.xxx.xxx.xxx），如果每天有很多用户访问，需要统计很多天，则需要耗费很大的内存空间进行存放。而使用HyperLogLog，每天只需要12KB，占用的内存空间大大减少。

==注意区分和set的区别，这个是不存数据的，也就是说取不出来==

> 命令

```
PFADD	输入元素
PFCOUNT	返回基数
PFMERGE 合并集合
```

## Bitmap

位图，用二进制进行记录，非0即1，适合记录两种状态的数据。

```bash
setbit mykey 0 1	# 给0赋值为1
getbit mykey 0
bitcount mykey		# 统计值为1的bit数
BITPOS mykey 1 2 	# 查找bit位第一次为1的位置
BITOP AND dest key1 key2 	#用AND,OR,XOR,NOT对后续集合做位运算，并将结果保存在dest中
```

# 事务

## 事务的运行

redis保证事务能作为一个整体按顺序执行，期间不会插入其他命令，但是并不保证原子性。即如果事务中某条命令实行失败，剩余命令会继续执行，不会回滚。

redis的事务分为两个阶段：

- 开启事务（multi）
- 命令入队（……）
- 执行事务（exec）

> 开启事务

```bash
127.0.0.1:6379> multi
OK
127.0.0.1:6379(TX)> set k1 v1
QUEUED
127.0.0.1:6379(TX)> set k2 v2
QUEUED
127.0.0.1:6379(TX)> get k2
QUEUED
127.0.0.1:6379(TX)> exec
1) OK
2) OK
3) "v2"
```

> 取消事务

```
127.0.0.1:6379> multi
OK
127.0.0.1:6379(TX)> set k3 v3
QUEUED
127.0.0.1:6379(TX)> DISCARD
OK
127.0.0.1:6379> get k3
(nil)
127.0.0.1:6379> 

```

> 编译时异常

当发生编译时异常（即代码异常）时，事务不能通过检查，所以所有命令都不会执行

```bash
127.0.0.1:6379(TX)> set k1 v1
QUEUED
127.0.0.1:6379(TX)> GETSET k1
(error) ERR wrong number of arguments for 'getset' command
127.0.0.1:6379(TX)> exec
(error) EXECABORT Transaction discarded because of previous errors.
```

> 运行时异常

当事务发生运行时异常，并不会回滚。事务会继续进行错误命令之后的剩余命令，因此不具有原子性。

```bash
127.0.0.1:6379> multi
OK
127.0.0.1:6379(TX)> set k1 v1
QUEUED
127.0.0.1:6379(TX)> INCR k1		#字符串不能自增
QUEUED
127.0.0.1:6379(TX)> set k2 v2
QUEUED
127.0.0.1:6379(TX)> exec
1) OK
2) (error) ERR value is not an integer or out of range
3) OK
127.0.0.1:6379> get k2
"v2"
```

## 事务的监控

> 悲观锁：认为任何时候都会出现问题，无论做什么都会加锁。
>
> 乐观锁：认为任何时候都会出现问题，不会加锁。只是会在更新数据的时候去判断一下，在此期间是否有人修改过这个数据（version）。

mysql乐观锁举例：

- 取出记录时，获取当前version
- 更新时，带上这个version
- 执行更新时， set version = newVersion where version = oldVersion
- 如果version不对，就更新失败

redis中采用了乐观锁的方式保持数据的一致性。

> watch 命令

`watch`命令是用来监控某一key，可以理解为给他加上了乐观锁。

当只有一个事务对其进行操作的时候，事务可以正常的执行

```bash
127.0.0.1:6379> set money 100
OK
127.0.0.1:6379> multi
OK
127.0.0.1:6379(TX)> DECRBY money 10
QUEUED
127.0.0.1:6379(TX)> exec
1) (integer) 90
```

但是如果事务执行时，检测到该字段已经被修改，那么将会执行失败。

例如：进程1开始监控money字段

```bash
127.0.0.1:6379> set money 100
OK
127.0.0.1:6379> WATCH money
OK
```

此时进程2修改了这一字段

```bash
127.0.0.1:6379> set money 200
OK
```

这时进程1再去提交事务，这时我们看到事务可以正常入队，但是不能正常执行。

```bash
127.0.0.1:6379> multi
OK
127.0.0.1:6379(TX)> DECRBY money 10
QUEUED
127.0.0.1:6379(TX)> exec
(nil)
127.0.0.1:6379> get money
"200"
```

**总结：**

- watch命令相当于系统记录了这个属性的oldversion，当提交事务时，如果发现该属性和oldversion不相等，那么提交失败。
- ==执行完毕后，无论事务成功与否，Redis都会取消watch监控==

# Jedis

jedis是官方提供的使用java操作redis的工具包。

导入依赖：

```xml
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>3.6.0</version>
</dependency>
```

redis命令被封装成一个个函数，用户只需要通过这些函数对redis服务器进行操作。

![image-20210512150521357](https://gitee.com/mw515031/image/raw/master/image/image-20210512150521357.png)

# SpringBoot整合

> Spring Data的任务是为数据访问提供一个熟悉的、一致的、基于Spring的编程模型，同时仍然保留底层数据存储的特殊特征。它使得使用数据访问技术、关系和非关系数据库、map-reduce框架以及基于云的数据服务变得容易。这是一个伞式项目，它包含许多特定于给定数据库的子项目。这些项目是通过与这些令人兴奋的技术背后的许多公司和开发人员合作开发的。

SpringData是Spring家族中负责与各种数据库打交道的模块，因此SpringBoot要通过SpringData对redis进行操作。

SpringBoot2.x以后，底层对redis的操作由jedis替换成了lettuce，二者区别如下：

1. Jedis 是直连模式，在多个线程间共享一个 Jedis 实例时是线程不安全的，每个线程都去拿自己的 Jedis 实例，当连接数量增多时，物理连接成本就较高了。

2. Lettuce的连接是基于Netty的，连接实例可以在多个线程间共享，大致意思就是一个多线程的应用可以使用同一个连接实例，而不用担心并发线程的数量。通过异步的方式可以让我们更好地利用系统资源。

> 1、配置依赖

```xml
<dependencies>
    <!--redis启动依赖-->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-redis</artifactId>
    </dependency>
    <!--redi其他依赖 ↓ -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-devtools</artifactId>
        <scope>runtime</scope>
        <optional>true</optional>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-configuration-processor</artifactId>
        <optional>true</optional>
    </dependency>
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <optional>true</optional>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
</dependencies>
```

> 2、配置连接

```properties
spring.redis.host=192.168.188.103
spring.redis.port=6379
spring.redis.password=123456
```

> 3、测试

```java
@SpringBootTest
class RedisSpringbootApplicationTests {

    @Autowired
    private RedisTemplate redisTemplate;

    @Test
    void contextLoads() {
        
        //下面是通过redisTemplate对各种数据结构进行操作。具体的操作直接链式调用跟在后边即可。api和redis命令是一样的。
        redisTemplate.opsForValue().get("k1");
        redisTemplate.opsForList()
        redisTemplate.opsForSet()
        redisTemplate.opsForZSet()
        redisTemplate.opsForHash()
        redisTemplate.opsForGeo()
        redisTemplate.opsForHyperLogLog()
            
        //一些操作直接封装在了redisTemplate中，比如事务和基本的crud
        
        //获取redis连接对象
        RedisConnection connection = redisTemplate.getConnectionFactory().getConnection();
        connection.flushAll();
        connection.flushDb();
    }
}
```

==上边的这些操作，一般不会使用原生调用方式，而是封装成一个工具类对其进行操作。==网上有很多，一般公司里也会有。

# 自定义RedisTemplate实现序列化

当我们要把一个对象存到redis中，就难免要进行对象的网络传输。而我们知道对象是不能直接进行网络传输的，所以一般有两个方案：

- 转换成JSON格式（有各种工具包可以实现，这里就不提了）
- 对象序列化（实体类实现serializable接口）

redis的默认序列化方式为JDK序列化，这种序列化方式会使传进去的值前边有很多转移字符，所以我们可以编写自己的序列化方式来避免这一现象。

```java
@Configuration
public class RedisConfig {

    @Bean
    @ConditionalOnSingleCandidate(RedisConnectionFactory.class)
    public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory redisConnectionFactory) {
        // 为了开发方便，一般使用<String, Object>
        RedisTemplate<String, Object> template = new RedisTemplate();
        template.setConnectionFactory(redisConnectionFactory);

        // Json序列化设置，把所有对象都用Json序列化
        Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);
        // 配置ObjectMapper
        ObjectMapper objectMapper = new ObjectMapper();
        objectMapper.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        //objectMapper.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);这是个过时的方法，可能会出问题，。注①
        //objectMapper.activateDefaultTyping(LaissezFaireSubTypeValidator.instance,ObjectMapper.DefaultTyping.NON_FINAL);
        //上边这两个方法都不带反而能成功
        jackson2JsonRedisSerializer.setObjectMapper(objectMapper);
        // String序列化
        StringRedisSerializer stringRedisSerializer = new StringRedisSerializer();

        // Key采用String的序列化方式
        template.setKeySerializer(stringRedisSerializer);
        // HashKey采用String的序列化方式
        template.setHashKeySerializer(stringRedisSerializer);
        // Value采用jackson的序列化方式
        template.setValueSerializer(jackson2JsonRedisSerializer);
        // HashValue采用jackson的序列化方式
        template.setHashValueSerializer(jackson2JsonRedisSerializer);
        template.afterPropertiesSet();

        return template;
    }
}
```

注①：

Spring源码中是使用容器中的ObjectMapper对象进行序列化和反序列化。

当我们将自定义的ObjectMapper对象放入IOC容器中后，会自动覆盖SpringBoot自动装载的ObjectMapper对象。

若是我们在自定义的ObjectMapper中设置了objectMapper.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);属性。

那么可能会影响我们的@RequestBody反序列化JSON串，所以要将配置类的om.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL); 注释掉.

# Redis.conf详解

> 自定义单位大小

![image-20210513130237793](https://gitee.com/mw515031/image/raw/master/image/image-20210513130237793.png)

单位对大小写不敏感

> INCLUDE：该配置文件可以包含其他配置文件

![image-20210513130440075](https://gitee.com/mw515031/image/raw/master/image/image-20210513130440075.png)

> NETWORK

- bind：绑定服务器的ip [见上文](#远程连接)
- protected-mode：保护模式[见上文](#远程连接)
- port：端口号
- timeout：客户端一段时间内没操作就关闭连接。（0代表不开启此功能）

> GENERAL

```bash
#是否作为守护进程开启
daemonize yes

#如果以守护进程开启，我们就需要指定一个pid文件
pidfile /var/run/redis_6379.pid

#日志级别
# Specify the server verbosity level.
# This can be one of:
# debug (a lot of information, useful for development/testing)
# verbose (many rarely useful info, but not a mess like the debug level)
# notice (moderately verbose, what you want in production probably)
# warning (only very important / critical messages are logged)
loglevel notice

#日志文件名
logfile ""

#数据库数量
database 16

#redis服务启动时是否显示logo
always-show-logo no
```

> SNAPSHOTTING

持久化设置，什么情况下会持久化到文件中

```bash
# Unless specified otherwise, by default Redis will save the DB:
#   * After 3600 seconds (an hour) if at least 1 key changed
#   * After 300 seconds (5 minutes) if at least 100 keys changed
#   * After 60 seconds if at least 10000 keys changed
#
# You can set these explicitly by uncommenting the three following lines.
#
# save 3600 1
# save 300 100
# save 60 10000

#持久化出错时，是否继续工作
stop-writes-on-bgsave-error yes

#是否压缩rdb文件
rdbcompression yes

#是否检验rdb文件
rdbchecksum yes

#rdb文件保存目录
dir./
```

> SECURITY

```bash
# redis登录密码
requirepass 123456
```

> CLIENTS

```bash
#客户端数量限制
maxclients 10000
```

> MEMORY MANAGEMENT

```bash
#最大内存设置
maxmemory <bytes>

#内存达到上限的处理策略
# volatile-lru -> Evict using approximated LRU, only keys with an expire set.
# allkeys-lru -> Evict any key using approximated LRU.
# volatile-lfu -> Evict using approximated LFU, only keys with an expire set.
# allkeys-lfu -> Evict any key using approximated LFU.
# volatile-random -> Remove a random key having an expire set.
# allkeys-random -> Remove a random key, any key.
# volatile-ttl -> Remove the key with the nearest expire time (minor TTL)
# noeviction -> Don't evict anything, just return an error on write operations.
maxmemory-policy noeviction
```

> APPEND ONLY MODE：AOF配置

```bash
#默认是不开启的，默认使用rdb方式进行持久化，在大部分情况下，rdb完全够用
appendonly no

#持久化文件名
appendfilename “”

# appendfsync always	#每次修改都fsync()，慢，安全
appendfsync everysec	#每秒执行一次fsync()，折中
# appendfsync no		#不执行同步fsync()，由操作系统自动同步，速度最快
```

# Redis持久化

redis是一个内存数据库，因此是断电即失的。所以要按某种机制将其保存子磁盘中才能保证永久储存和安全。

redis有两种持久化方案：

- rdb：默认方案
- aof

## RDB

![image-20210513145040397](https://gitee.com/mw515031/image/raw/master/image/image-20210513145040397.png)

rdb有两种触发机制，分别是自动触发和手动触发。

 **①、自动触发**

rdb方案会按某种规则（redis.sconf中的Snapchat类别里设置），定期保存.rdb文件（默认文件名为dump.rdb），从而实现持久化。

触发机制：

1. save规则满足
2. 执行flushall命令，但rdb文件里边是空的
3. 退出redis

满足上述任何一个条件，都会生成dump.rdb文件.

**②、手动触发**

手动触发Redis进行RDB持久化的命令有两种：

　　1、save

　　该命令会阻塞当前Redis服务器，执行save命令期间，Redis不能处理其他命令，直到RDB过程完成为止。

　　显然该命令对于内存比较大的实例会造成长时间阻塞，这是致命的缺陷，为了解决此问题，Redis提供了第二种方式。

　　2、bgsave

　　执行该命令时，Redis会在后台异步进行快照操作，快照同时还可以响应客户端请求。具体操作是Redis进程执行fork操作创建子进程，RDB持久化过程由子进程负责，完成后自动结束。阻塞只发生在fork阶段，一般时间很短。

　　==基本上 Redis 内部所有的RDB操作都是采用 bgsave 命令。==

**恢复rdb文件：**

只需要把rdb文件放在redis-server同级目录，redis启动时就会自动检查dump.rdb文件，恢复其中的数据

## RDB的优缺点

1. 优势

（1）RDB文件紧凑，全量备份，非常适合用于进行备份和灾难恢复。

（2）生成RDB文件的时候，redis主进程会fork()一个子进程来处理所有保存工作，主进程不需要进行任何磁盘IO操作。

（3）RDB 在恢复大数据集时的速度比 AOF 的恢复速度要快。

2. 劣势

RDB快照是一次全量备份，存储的是内存数据的二进制序列化形式，存储上非常紧凑。当进行快照持久化时，会开启一个子进程专门负责快照持久化，子进程会拥有父进程的内存数据，父进程修改内存子进程不会反应出来，所以在快照持久化期间修改的数据不会被保存，可能丢失数据。

所以在生产环境，我们会将dump文件备份。

## AOF

![image-20210513161112464](https://gitee.com/mw515031/image/raw/master/image/image-20210513161112464.png)

以日志形式记录每个写操作，将Redis执行过的所有写指令记录下来。只需追加文件但不可以改写文件，redis启动之初会读取该文件重新构建数据库。因此当redis重启时，系统就会根据日志文件的内容将写指令从前到后执行一次从而完成对数据库的恢复。

AOF默认保存文件为appendonly.aof

AOF默认是不开启的，要在配置文件中设置`appendonly yes`

**AOF重写流程**

![image-20210513161335271](https://gitee.com/mw515031/image/raw/master/image/image-20210513161335271.png)

**redis-check-aof**

该文件的位置与redis-server同级，作用是检查aof文件是否被破坏。比如人为的加一些其他字符破坏了aof文件的结构 。 

aof文件格式不符，可能会引起可以启动服务器但拒绝连接的情况。

## AOF优缺点

**优点：**

1. 使用AOF 会让你的Redis更加耐久: 你可以使用不同的fsync策略：无fsync，每秒fsync，每次写的时候fsync。使用默认的每秒fsync策略，Redis的性能依然很好(fsync是由后台线程进行处理的，主线程会尽力处理客户端请求)，一旦出现故障，你最多丢失1秒的数据。
2. AOF文件是一个只进行追加的日志文件，所以不需要写入seek，即使由于某些原因(磁盘空间已满，写的过程中宕机等等)未执行完整的写入命令，你也也可使用redis-check-aof工具修复这些问题。
3. Redis 可以在 AOF 文件体积变得过大时，自动地在后台对 AOF 进行重写： 重写后的新 AOF 文件包含了恢复当前数据集所需的最小命令集合。 整个重写操作是绝对安全的，因为 Redis 在创建新 AOF 文件的过程中，会继续将命令追加到现有的 AOF 文件里面，即使重写过程中发生停机，现有的 AOF 文件也不会丢失。 而一旦新 AOF 文件创建完毕，Redis 就会从旧 AOF 文件切换到新 AOF 文件，并开始对新 AOF 文件进行追加操作。
4. AOF 文件有序地保存了对数据库执行的所有写入操作， 这些写入操作以 Redis 协议的格式保存， 因此 AOF 文件的内容非常容易被人读懂， 对文件进行分析（parse）也很轻松。 导出（export） AOF 文件也非常简单： 举个例子， 如果你不小心执行了 FLUSHALL 命令， 但只要 AOF 文件未被重写， 那么只要停止服务器， 移除 AOF 文件末尾的 FLUSHALL 命令， 并重启 Redis ， 就可以将数据集恢复到 FLUSHALL 执行之前的状态。

**缺点：**

1. 对于相同的数据集来说，AOF 文件的体积通常要大于 RDB 文件的体积。
2. 根据所使用的 fsync 策略，AOF 的速度可能会慢于 RDB 。 在一般情况下， 每秒 fsync 的性能依然非常高， 而关闭 fsync 可以让 AOF 的速度和 RDB 一样快， 即使在高负荷之下也是如此。 不过在处理巨大的写入载入时，RDB 可以提供更有保证的最大延迟时间（latency）。

## 同时开启两种持久化方式

- 当redis重启时，会优先载入AOF文件来恢复原始数据。因为通常AOF文件保存的数据集要比RDB更完整

- 既然AOF那么好，那要不要只启用AOF？作者不建议这么做，因为RDB跟适合备份数据库，快速重启，而且不会有AOF可能潜在的BUG，要留个备用手段以防万一。

# Redis发布订阅

Redis 发布订阅 (pub/sub) 是一种消息通信模式：发送者 (pub) 发送消息，订阅者 (sub) 接收消息。

Redis 客户端可以订阅任意数量的频道。

发布订阅示意图：

下图展示了频道 channel1 ， 以及订阅这个频道的三个客户端 —— client2 、 client5 和 client1 之间的关系：

![img](https://gitee.com/mw515031/image/raw/master/image/pubsub1.png)

当有新消息通过 PUBLISH 命令发送给频道 channel1 时， 这个消息就会被发送给订阅它的三个客户端：

![img](https://gitee.com/mw515031/image/raw/master/image/pubsub2.png)

发布订阅常见命令：

| 序号 | 命令及描述                                                   |
| :--- | :----------------------------------------------------------- |
| 1    | [PSUBSCRIBE pattern pattern ...] 订阅一个或多个符合给定模式的频道。 |
| 2    | PUBSUB subcommand [argument [argument ...]] 查看订阅与发布系统状态。 |
| 3    | PUBLISH channel message 将信息发送到指定的频道。             |
| 4    | PUNSUBSCRIBE [pattern [pattern ...]] 退订所有给定模式的频道。 |
| 5    | [SUBSCRIBE channel channel ...] 订阅给定的一个或多个频道的信息。 |
| 6    | UNSUBSCRIBE [channel [channel ...]] 指退订给定的频道。       |

## 订阅频道

每个 Redis 服务器进程都维持着一个表示服务器状态的 `redis.h/redisServer` 结构， 结构的 `pubsub_channels` 属性是一个字典， 这个字典就用于保存订阅频道的信息。

```c
struct redisServer {
    // ...
    dict *pubsub_channels;
    // ...
};
```

其中，字典的键为正在被订阅的频道， 而字典的值则是一个链表， 链表中保存了所有订阅这个频道的客户端。

比如说，在下图展示的这个 `pubsub_channels` 示例中， `client2` 、 `client5` 和 `client1` 就订阅了 `channel1` ， 而其他频道也分别被别的客户端所订阅：

![image-20210513182356891](https://gitee.com/mw515031/image/raw/master/image/image-20210513182356891.png)

当客户端调用 [SUBSCRIBE](http://redis.readthedocs.org/en/latest/pub_sub/subscribe.html#subscribe) 命令时， 程序就将客户端和要订阅的频道在 `pubsub_channels` 字典中关联起来。

举个例子，如果客户端 `client10086` 执行命令 `SUBSCRIBE channel1 channel2 channel3` ，那么前面展示的 `pubsub_channels` 将变成下面这个样子：

![image-20210513182423981](https://gitee.com/mw515031/image/raw/master/image/image-20210513182423981.png)

通过 `pubsub_channels` 字典， 程序只要检查某个频道是否为字典的键， 就可以知道该频道是否正在被客户端订阅； 只要取出某个键的值， 就可以得到所有订阅该频道的客户端的信息。

```python
def SUBSCRIBE(client, channels):

    # 遍历所有输入频道
    for channel in channels:

        # 将客户端添加到链表的末尾
        redisServer.pubsub_channels[channel].append(client)
```

通过 `pubsub_channels` 字典， 程序只要检查某个频道是否为字典的键， 就可以知道该频道是否正在被客户端订阅； 只要取出某个键的值， 就可以得到所有订阅该频道的客户端的信息。

## 发送信息到频道

了解了 `pubsub_channels` 字典的结构之后， 解释 [PUBLISH](http://redis.readthedocs.org/en/latest/pub_sub/publish.html#publish) 命令的实现就非常简单了： 当调用 `PUBLISH channel message` 命令， 程序首先根据 `channel` 定位到字典的键， 然后将信息发送给字典值链表中的所有客户端。

比如说，对于以下这个 `pubsub_channels` 实例， 如果某个客户端执行命令 `PUBLISH channel1 "hello moto"` ，那么 `client2` 、 `client5` 和 `client1` 三个客户端都将接收到 `"hello moto"` 信息：

![image-20210513182600383](https://gitee.com/mw515031/image/raw/master/image/image-20210513182600383.png)

[PUBLISH](http://redis.readthedocs.org/en/latest/pub_sub/publish.html#publish) 命令的实现可以用以下伪代码来描述：

```python
def PUBLISH(channel, message):

    # 遍历所有订阅频道 channel 的客户端
    for client in server.pubsub_channels[channel]:

        # 将信息发送给它们
        send_message(client, message)
```

## 退订频道

使用 UNSUBSCRIBE 命令可以退订指定的频道， 这个命令执行的是订阅的反操作： 它从 pubsub_channels 字典的给定频道（键）中， 删除关于当前客户端的信息， 这样被退订频道的信息就不会再发送给这个客户端。

## 模式的订阅与信息发送

当使用 [PUBLISH](http://redis.readthedocs.org/en/latest/pub_sub/publish.html#publish) 命令发送信息到某个频道时， 不仅所有订阅该频道的客户端会收到信息， 如果有某个/某些模式和这个频道匹配的话， 那么所有订阅这个/这些频道的客户端也同样会收到信息。

下图展示了一个带有频道和模式的例子， 其中 `tweet.shop.*` 模式匹配了 `tweet.shop.kindle` 频道和 `tweet.shop.ipad` 频道， 并且有不同的客户端分别订阅它们三个：

![image-20210513182951102](https://gitee.com/mw515031/image/raw/master/image/image-20210513182951102.png)

当有信息发送到 `tweet.shop.kindle` 频道时， 信息除了发送给 `clientX` 和 `clientY` 之外， 还会发送给订阅 `tweet.shop.*` 模式的 `client123` 和 `client256` ：

![](https://gitee.com/mw515031/image/raw/master/image/graphviz-3d1f513ee0718a326d53152b2b97f82977e38ad6.svg)

另一方面， 如果接收到信息的是频道 `tweet.shop.ipad` ， 那么 `client123` 和 `client256` 同样会收到信息：

![](https://gitee.com/mw515031/image/raw/master/image/graphviz-ba8c4d4dd538464659aeb52d6c366f23ad3d0dc1.svg)

## 订阅模式

`redisServer.pubsub_patterns` 属性是一个链表，链表中保存着所有和模式相关的信息：

```c
struct redisServer {
    // ...
    list *pubsub_patterns;
    // ...
};
```

链表中的每个节点都包含一个 `redis.h/pubsubPattern` 结构：

```c
typedef struct pubsubPattern {
    redisClient *client;
    robj *pattern;
} pubsubPattern;
```

`client` 属性保存着订阅模式的客户端，而 `pattern` 属性则保存着被订阅的模式。

每当调用 `PSUBSCRIBE` 命令订阅一个模式时， 程序就创建一个包含客户端信息和被订阅模式的 `pubsubPattern` 结构， 并将该结构添加到 `redisServer.pubsub_patterns` 链表中。

作为例子，下图展示了一个包含两个模式的 `pubsub_patterns` 链表， 其中 `client123` 和 `client256` 都正在订阅 `tweet.shop.*` 模式：

![](https://gitee.com/mw515031/image/raw/master/image/graphviz-b8d101c1b582531bce2b0daef87adbaf30ebc195.svg)

如果这时客户端 `client10086` 执行 `PSUBSCRIBE broadcast.list.*` ， 那么 `pubsub_patterns` 链表将被更新成这样：

![](https://gitee.com/mw515031/image/raw/master/image/graphviz-a84f3abf466ca19297faaa4e11d37f9257355c60.svg)

通过遍历整个 `pubsub_patterns` 链表，程序可以检查所有正在被订阅的模式，以及订阅这些模式的客户端。

## 发送信息到模式

发送信息到模式的工作也是由 [PUBLISH](http://redis.readthedocs.org/en/latest/pub_sub/publish.html#publish) 命令进行的， 在前面讲解频道的时候， 我们给出了这样一段伪代码， 说它定义了 [PUBLISH](http://redis.readthedocs.org/en/latest/pub_sub/publish.html#publish) 命令的行为：

```python
def PUBLISH(channel, message):

    # 遍历所有订阅频道 channel 的客户端
    for client in server.pubsub_channels[channel]:

        # 将信息发送给它们
        send_message(client, message)
```

但是，这段伪代码并没有完整描述 [PUBLISH](http://redis.readthedocs.org/en/latest/pub_sub/publish.html#publish) 命令的行为， 因为 [PUBLISH](http://redis.readthedocs.org/en/latest/pub_sub/publish.html#publish) 除了将 `message` 发送到所有订阅 `channel` 的客户端之外， 它还会将 `channel` 和 `pubsub_patterns` 中的模式进行对比， 如果 `channel` 和某个模式匹配的话， 那么也将 `message` 发送到订阅那个模式的客户端。

完整描述 [PUBLISH](http://redis.readthedocs.org/en/latest/pub_sub/publish.html#publish) 功能的伪代码定于如下：

```python
def PUBLISH(channel, message):

    # 遍历所有订阅频道 channel 的客户端
    for client in server.pubsub_channels[channel]:

        # 将信息发送给它们
        send_message(client, message)

    # 取出所有模式，以及订阅模式的客户端
    for pattern, client in server.pubsub_patterns:

        # 如果 channel 和模式匹配
        if match(channel, pattern):

            # 那么也将信息发给订阅这个模式的客户端
            send_message(client, message)
```

举个例子，如果 Redis 服务器的 `pubsub_patterns` 状态如下：

![](https://gitee.com/mw515031/image/raw/master/image/graphviz-a84f3abf466ca19297faaa4e11d37f9257355c60.svg)

那么当某个客户端发送信息 `"Amazon Kindle, $69."` 到 `tweet.shop.kindle` 频道时， 除了所有订阅了 `tweet.shop.kindle` 频道的客户端会收到信息之外， 客户端 `client123` 和 `client256` 也同样会收到信息， 因为这两个客户端订阅的 `tweet.shop.*` 模式和 `tweet.shop.kindle` 频道匹配。

## 退订模式

使用 [PUNSUBSCRIBE](http://redis.readthedocs.org/en/latest/pub_sub/punsubscribe.html#punsubscribe) 命令可以退订指定的模式， 这个命令执行的是订阅模式的反操作： 程序会删除 `redisServer.pubsub_patterns` 链表中， 所有和被退订模式相关联的 `pubsubPattern` 结构， 这样客户端就不会再收到和模式相匹配的频道发来的信息。

## 小结

- 订阅信息由服务器进程维持的 `redisServer.pubsub_channels` 字典保存，字典的键为被订阅的频道，字典的值为订阅频道的所有客户端。
- 当有新消息发送到频道时，程序遍历频道（键）所对应的（值）所有客户端，然后将消息发送到所有订阅频道的客户端上。
- 订阅模式的信息由服务器进程维持的 `redisServer.pubsub_patterns` 链表保存，链表的每个节点都保存着一个 `pubsubPattern` 结构，结构中保存着被订阅的模式，以及订阅该模式的客户端。程序通过遍历链表来查找某个频道是否和某个模式匹配。
- 当有新消息发送到频道时，除了订阅频道的客户端会收到消息之外，所有订阅了匹配频道的模式的客户端，也同样会收到消息。
- 退订频道和退订模式分别是订阅频道和订阅模式的反操作。

redis主业毕竟不是做消息队列的，所以只能实现一些简单操作，要想真正实现复杂功能，还是要使用其他消息中间件来实现。如kafka，rabbitMQ等。

## 示例

当一个用户订阅某一频道时，会等待这一频道发出消息，如下：

![image-20210513182002556](https://gitee.com/mw515031/image/raw/master/image/image-20210513182002556.png)

这时，如果有人向这一频道发出消息

```bash
127.0.0.1:6379> PUBLISH mw hello
(integer) 1
```

那么所有订阅这一频道的用户都会收到该消息

![image-20210513181825305](https://gitee.com/mw515031/image/raw/master/image/image-20210513181825305.png)

#Redis集群

对一个数据库来说，大部分操作都是读操作，因此实现读写分离能很好的提高服务器的承受能力。

主服务器只接受写请求，所有的读请求则分发给其他的从节点。

## Redis集群搭建

**显示当前服务器信息**：

```bash
127.0.0.1:6379> INFO replication
# Replication
role:master
connected_slaves:0
master_failover_state:no-failover
master_replid:6e88479469dc20aadbee5e9829e0620b07a5a2d2
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:0
second_repl_offset:-1
repl_backlog_active:0
repl_backlog_size:1048576
repl_backlog_first_byte_offset:0
repl_backlog_histlen:0
```

**需要修改的配置文件表项**

```bash
port 6379
pidfile /var/run/redis_6379.pid
logfile "6379"
dbfilename dump6379.rdb
```

启动多个服务，这里演示了单机多服务的形式

![image-20210513195741705](https://gitee.com/mw515031/image/raw/master/image/image-20210513195741705.png)

**配置一主二从**

默认情况下，每台redis服务器都是主节点，所以我们一般只需配置从机即可。

假设这里我们把79端口当做主机，80、81端口当做从机

```bash
# 为主机80指定从机
127.0.0.1:6380> slaveof 127.0.0.1 6379
OK

# 为主机81指定从机
127.0.0.1:6381> slaveof 127.0.0.1 6379
OK
```

ps：如果要给一个结点恢复主机身份可以用这个命令:`slaveof no one`

==这里是通过命令来配置从机，重启服务后即失效。要想永久配置集群结构，要在配置文件中指定==

```bash
# 指定主机
replicaof <masterip> <masterport>

# 如果主机设置了密码，还要带上密码
masterauth <master-password>
```



## 主从复制原理

为了避免单点故障，数据存储需要进行多副本构建。同时由于 Redis 的核心操作是单线程模型的，单个 Redis 实例能处理的请求 TPS 有限。因此 Redis 自面世起，基本就提供了复制功能，而且对复制策略不断进行优化。

![image-20210513190233883](https://gitee.com/mw515031/image/raw/master/image/image-20210513190233883.png)

通过数据复制，Redis 的一个 master 可以挂载多个 slave，而 slave 下还可以挂载多个 slave，形成多层嵌套结构。所有写操作都在 master 实例中进行，master 执行完毕后，将写指令分发给挂在自己下面的 slave 节点。slave 节点下如果有嵌套的 slave，会将收到的写指令进一步分发给挂在自己下面的 slave。每个结点都可以进行读操作。

每一个 Redis master 都有一个 replication ID ：这是一个较大的伪随机字符串，标记了一个给定的数据集。每个 master 也持有一个偏移量，master 将自己产生的复制流发送给 slave 时，发送多少个字节的数据，自身的偏移量就会增加多少，目的是当有新的操作修改自己的数据集时，它可以以此更新 slave 的状态。复制偏移量即使在没有一个 slave 连接到 master 时，也会自增，所以基本上每一对给定的

`Replication ID, offset`

都会标识一个 master 数据集的确切版本。

当 slave 连接到 master 时，它们使用 PSYNC 命令来发送它们记录的旧的 master replication ID 和它们至今为止处理的偏移量。通过这种方式， master 能够仅发送 slave 所需的增量部分。但是如果 master 的缓冲区中没有足够的命令积压缓冲记录，或者如果 slave 引用了不再知道的历史记录（replication ID），则会转而进行一个全量重同步：在这种情况下， slave 会得到一个完整的数据集副本，从头开始。

**全量同步：**

master 开启一个后台保存进程，以便于生产一个 RDB 文件。同时它开始缓冲所有从客户端接收到的新的写入命令。当后台保存完成时， master 将数据集文件传输给 slave， slave将之保存在磁盘上，然后加载文件到内存。再然后 master 会发送所有缓冲的命令发给 slave。这个过程以指令流的形式完成并且和 Redis 协议本身的格式相同。

**增量同步：**

master 只发送 slave 上次复制位置之后的写指令，不用构建 rdb，而且传输内容非常有限，对 master、slave 的负荷影响很小，对带宽的影响可以忽略，整个系统受影响非常小。

在 Redis 2.8 之前，Redis 基本只支持全量复制。在 slave 与 master 断开连接，或 slave 重启后，都需要进行全量复制。在 2.8 版本之后，Redis 引入 psync，增加了一个复制积压缓冲，在将写指令同步给 slave 时，会同时在复制积压缓冲中也写一份。

在 slave 短时断开重连后，上报master runid 及复制偏移量。如果 runid 与 master 一致，且偏移量offset仍然在 master 的复制缓冲积压中，则 master 进行增量同步。

但如果 slave 重启后，master runid 会丢失，或者切换 master 后，runid 会变化，仍然需要全量同步。

因此 Redis 自 4.0 强化了 psync，引入了 psync2。在 pysnc2 中，主从复制不再使用 runid，而使用 replid（即复制id） 来作为复制判断依据。同时 Redis 实例在构建 rdb 时，会将 replid 作为 aux 辅助信息存入 rbd。重启时，加载 rdb 时即可得到 master 的复制 id。从而在 slave 重启后仍然可以增量同步。

在 psync2 中，Redis 每个实例除了会有一个复制 id 即 replid 外，还有一个 replid2。Redis 启动后，会创建一个长度为 40 的随机字符串，作为 replid 的初值，在建立主从连接后，会用 master的 replid 替换自己的 replid。同时会用 replid2 存储上次 master 主库的 replid。这样切主时，即便 slave 汇报的复制 id 与新 master 的 replid 不同，但和新 master 的 replid2 相同，同时复制偏移仍然在复制积压缓冲区内，仍然可以实现增量复制。

总结：

说白了就是每个slave会保存它==新master的replid==和==旧master的replid2==。当某一收到从库的复制请求和replid或replid2二者中的任意一个相同，自己都会试图进行增量复制（因为还要检查偏移量是否在自己的复制缓冲积压中）。4.0以前只要salve重启或者切换master都得全量复制。[replid的变化过程点这里](#示例：一主二从选举中==replid==和==replid2==的变化)

### 主从连接流程

![image-20210514133105936](https://gitee.com/mw515031/image/raw/master/image/image-20210514133105936.png)

slave 创建与 master 的连接后，首先发送 ping 指令，如果 master 没有返回异常，而是返回 pong，则说明 master 可用。

如果 Redis 设置了密码，slave 会发送 auth $masterauth 指令，进行鉴权。当鉴权完毕，从库就通过 replconf 发送自己的端口及 IP 给 master。

接下来，slave 继续通过 replconf 发送 capa eof capa psync2 进行复制版本校验。如果 master 校验成功。从库接下来就通过 psync 将自己的复制 id、复制偏移发送给 master，正式开始准备数据同步。

主库接收到从库发来的 psync 指令后，则开始判断可以进行数据同步的方式。

前面讲到，Redis 当前保存了复制 id，replid 和 replid2。如果从库发来的复制 id，与 master 的复制 id（即 replid 和 replid2）相同，并且复制偏移在复制缓冲积压中，则可以进行增量同步。master 发送 continue 响应，并返回 master 的 replid。slave 将 master 的 replid 替换为自己的 replid，并将之前的复制 id 设置为 replid2。之后，master 则可继续发送，复制偏移位置 之后的指令，给 slave，完成数据同步。

如果主库发现从库传来的复制 id 和自己的 replid、replid2 都不同，或者复制偏移不在复制积压缓冲中，则判定需要进行全量复制。master 发送 fullresync 响应，附带 replid 及复制偏移。然后， master 根据需要构建 rdb，并将 rdb 及复制缓冲发送给 slave。

对于增量复制，slave 接下来就等待接受 master 传来的复制缓冲及新增的写指令，进行数据同步。

而对于全量同步，slave 会首先进行，嵌套复制的清理工作，比如 slave 当前还有嵌套的 子slave，则该 slave 会关闭嵌套 子slave 的所有连接，并清理自己的复制积压缓冲。

然后，slave 会构建临时 rdb 文件，并从 master 连接中读取 rdb 的实际数据，写入 rdb 中。在写 rdb 文件时，每写 8M，就会做一个 fsync操作， 刷新文件缓冲。当接受 rdb 完毕则将 rdb 临时文件改名为 rdb 的真正名字。

接下来，slave 会首先清空老数据，即删除本地所有 DB 中的数据，并暂时停止从 master 继续接受数据。然后，slave 就开始全力加载 rdb 恢复数据，将数据从 rdb 加载到内存。

在 rdb 加载完毕后，slave 重新利用与 master 的连接 socket，创建与 master 连接的 client，并在此注册读事件，可以开始接受 master 的写指令了。

此时，slave 还会将 master 的 replid 和复制偏移设为自己的复制 id 和复制偏移 offset，并将自己的 replid2 清空，因为，slave 的所有嵌套 子slave 接下来也需要进行全量复制。

最后，slave 就会打开 aof 文件，在接受 master 的写指令后，执行完毕并写入到自己的 aof 中。

相比之前的 sync，psync2 优化很明显。在短时间断开连接、slave 重启、切主等多种场景，只要延迟不太久，复制偏移仍然在复制积压缓冲，均可进行增量同步。

master 不用构建并发送巨大的 rdb，可以大大减轻 master 的负荷和网络带宽的开销。同时，slave 可以通过轻量的增量复制，实现数据同步，快速恢复服务，减少系统抖动。

但是，psync 依然严重依赖于复制缓冲积压，太大会占用过多内存，太小会导致频繁的全量复制。而且，由于内存限制，即便设置相对较大的复制缓冲区，在 slave 断开连接较久时，仍然很容易被复制缓冲积压冲刷，从而导致全量复制。

# 哨兵模式

自动选举出master结点。

哨兵模式是一种特殊的进程，它会独立运行监控其他的redis服务器。原理是**哨兵通过发送命令，等待Redis服务器响应，从而监控运行的多个Redis实例。**

![image-20210513202700725](https://gitee.com/mw515031/image/raw/master/image/image-20210513202700725.png)

这里的哨兵有两个作用

- 通过发送命令，让Redis服务器返回监控其运行状态，包括主服务器和从服务器。
- 当哨兵监测到master宕机，会自动将slave切换成master，然后通过**发布订阅模式**通知其他的从服务器，修改配置文件，让它们切换主机。

然而一个哨兵进程对Redis服务器进行监控，可能会出现问题，为此，我们可以使用多个哨兵进行监控。各个哨兵之间还会进行监控，这样就形成了多哨兵模式。

![image-20210513203212790](https://gitee.com/mw515031/image/raw/master/image/image-20210513203212790.png)

用文字描述一下**故障切换（failover）**的过程。假设主服务器宕机，哨兵1先检测到这个结果，系统并不会马上进行failover过程，仅仅是哨兵1主观的认为主服务器不可用，这个现象成为**主观下线**。当后面的哨兵也检测到主服务器不可用，并且数量达到一定值时，那么哨兵之间就会进行一次投票，投票的结果由一个哨兵发起，进行failover操作。切换成功后，就会通过发布订阅模式，让各个哨兵把自己监控的从服务器实现切换主机，这个过程称为**客观下线**。这样对于客户端而言，一切都是透明的。

## 哨兵配置

编写哨兵配置文件sentinel.conf

运行一个 Sentinel 所需的最少配置如下所示：

```bash
sentinel monitor mymaster 127.0.0.1 6379 2	#只写这一条也能正常使用
sentinel down-after-milliseconds mymaster 60000
sentinel failover-timeout mymaster 180000
sentinel parallel-syncs mymaster 1
```

第一行配置指示 Sentinel 去监视一个名为 mymaster（任意） 的主服务器， 这个主服务器的 IP 地址为 127.0.0.1 ， 端口号为 6379 ， 而将这个主服务器判断为失效至少需要 2 个 Sentinel 同意 （只要同意 Sentinel 的数量不达标，自动故障迁移就不会执行）

这里只需要配置主机即可，哨兵会自动扫描它的从机并且始终监视这些服务器。即使该主机宕机后已经选举出新的主机，那么当原主机又恢复时，哨兵也会自动这个原主机变成新主机的slave。

不过要注意， 无论你设置要多少个 Sentinel 同意才能判断一个服务器失效， 一个 Sentinel 都需要获得系统中多数（majority） Sentinel 的支持， 才能发起一次自动故障迁移， 并预留一个给定的配置纪元 （configuration Epoch ，一个配置纪元就是一个新主服务器配置的版本号）。

换句话说， 在只有少数（minority） Sentinel 进程正常运作的情况下， Sentinel 是不能执行自动故障迁移的。（比如需要两个sentinel同意才会判断失效，但集群里只有一个sentinel）

**启动命令：**`redis-sentinel redis-conf/sentinel.conf`，启动时要指定配置文件

## 示例：一主二从选举中==replid==和==replid2==的变化

![leetcode 画布](https://gitee.com/mw515031/image/raw/master/image/leetcode%20%E7%94%BB%E5%B8%83.jpg)

# Redis高可用问题

## 缓存穿透（查不到）

访问一个缓存和数据库都不存在的 key，此时会直接打到数据库上，并且查不到数据，没法写缓存，所以下一次同样会打到数据库上。

此时，缓存起不到作用，请求每次都会走到数据库，流量大时数据库可能会被打挂。此时缓存就好像被“穿透”了一样，起不到任何作用。

**解决方案：**

1、**接口校验。**在正常业务流程中可能会存在少量访问不存在 key 的情况，但是一般不会出现大量的情况，所以这种场景最大的可能性是遭受了非法攻击。可以在最外层先做一层校验：用户鉴权、数据合法性校验等，例如商品查询中，商品的ID是正整数，则可以直接对非正整数直接过滤等等。

2、**缓存空值**。当访问缓存和DB都没有查询到值时，可以将空值写进缓存，但是设置较短的过期时间，该时间需要根据产品业务特性来设置。

3、**布隆过滤器**。使用布隆过滤器存储所有可能访问的 key，不存在的 key 直接被过滤，存在的 key 则再进一步查询缓存和数据库。

### 布隆过滤器

对所有可能查询的参数以hash形式存储，在控制层先校验，不符合则丢弃，从而避免了对底层存储系统的查询压力。

布隆过滤器的特点是判断不存在的，则一定不存在；判断存在的，大概率存在，但也有小概率不存在。并且这个概率是可控的，我们可以让这个概率变小或者变高，取决于用户本身的需求。

布隆过滤器由一个 bitSet 和 一组 Hash 函数（算法）组成，是一种空间效率极高的概率型算法和数据结构，主要用来判断一个元素是否在集合中存在。

在初始化时，bitSet 的每一位被初始化为0，同时会定义 Hash 函数，例如有3组 Hash 函数：hash1、hash2、hash3。

**写入流程**

当我们要写入一个值时，过程如下，以“jionghui”为例：

1）首先将“jionghui”跟3组 Hash 函数分别计算，得到 bitSet 的下标为：1、7、10。

2）将 bitSet 的这3个下标标记为1。

假设我们还有另外两个值：java 和 diaosi，按上面的流程跟 3组 Hash 函数分别计算，结果如下：

java：Hash 函数计算 bitSet 下标为：1、7、11

diaosi：Hash 函数计算 bitSet 下标为：4、10、11

![image-20210514101008560](https://gitee.com/mw515031/image/raw/master/image/image-20210514101008560.png)

**查询流程**

当我们要查询一个值时，过程如下，同样以“jionghui”为例：：

1）首先将“jionghui”跟3组 Hash 函数分别计算，得到 bitSet 的下标为：1、7、10。

2）查看 bitSet 的这3个下标是否都为1，如果这3个下标不都为1，则说明该值必然不存在，如果这3个下标都为1，则只能说明可能存在，并不能说明一定存在。

其实上图的例子已经说明了这个问题了，当我们只有值“jionghui”和“diaosi”时，bitSet 下标为1的有：1、4、7、10、11。

当我们又加入值“java”时，bitSet 下标为1的还是这5个，所以当 bitSet 下标为1的为：1、4、7、10、11 时，我们无法判断值“java”存不存在。

其根本原因是，不同的值在跟 Hash 函数计算后，可能会得到相同的下标，所以某个值的标记位，可能会被其他值给标上了。

这也是为啥布隆过滤器只能判断某个值可能存在，无法判断必然存在的原因。但是反过来，如果该值根据 Hash 函数计算的标记位没有全部都为1，那么则说明必然不存在，这个是肯定的。
降低这种误判率的思路也比较简单：

1）一个是加大 bitSet 的长度，这样不同的值出现“冲突”的概率就降低了，从而误判率也降低。

2）提升 Hash 函数的个数，Hash 函数越多，每个值对应的 bit 越多，从而误判率也降低。

### HashMap 和 布隆过滤器

估计有同学看了上面的例子，会觉得使用 HashMap 也能实现。

确实，当数据量不大时，HashMap 实现起来一点问题都没有，而且还没有误判率，简直完美，还要个鸡儿布隆过滤器。

不过，当数据量上去后，布隆过滤器的空间优势就会开始体现，特别是要存储的 key 占用空间越大，布隆过滤器的优势越明显。

Guava 中的 BloomFilter 在默认情况下，误判率接近3%，大概要使用5个 Hash 函数。

也就是说一个 key 最多占用空间就是 5 bit，而且当多个 key 填充同一个 bit 时，会进一步降低使用空间。

布隆过滤器占用多少空间，主要取决于 Hash 函数的个数，跟 key 本身的大小无关，这使得其在空间的优势非常大。

## 缓存击穿（量太大，缓存过期）

某一个热点 key，在缓存过期的一瞬间，同时有大量的请求打进来，由于此时缓存过期了，所以请求最终都会走到数据库，造成瞬时数据库请求量大、压力骤增，甚至可能打垮数据库。

解决方案：

1、**加互斥锁**。在并发的多个请求中，只有第一个请求线程能拿到锁并执行数据库查询操作，其他的线程拿不到锁就阻塞等着，等到第一个线程将数据写入缓存后，直接走缓存。



关于互斥锁的选择，网上看到的大部分文章都是选择 Redis 分布式锁（可以参考我之前的文章：面试必问的分布式锁，你懂了吗？），因为这个可以保证只有一个请求会走到数据库，这是一种思路。

但是其实仔细想想的话，这边其实没有必要保证只有一个请求走到数据库，只要保证走到数据库的请求能大大降低即可，所以还有另一个思路是 JVM 锁。

JVM 锁保证了在单台服务器上只有一个请求走到数据库，通常来说已经足够保证数据库的压力大大降低，同时在性能上比分布式锁更好。

需要注意的是，无论是使用“分布式锁”，还是“JVM 锁”，加锁时要按 key 维度去加锁。

我看网上很多文章都是使用一个“固定的 key”加锁，这样会导致不同的 key 之间也会互相阻塞，造成性能严重损耗。

使用 redis 分布式锁的伪代码，仅供参考：

```java
public Object getData(String key) throws InterruptedException {
    Object value = redis.get(key);
    // 缓存值过期
    if (value == null) {
        // lockRedis：专门用于加锁的redis；
        // "empty"：加锁的值随便设置都可以
        if (lockRedis.set(key, "empty", "PX", lockExpire, "NX")) {
            try {
                // 查询数据库，并写到缓存，让其他线程可以直接走缓存
                value = getDataFromDb(key);
                redis.set(key, value, "PX", expire);
            } catch (Exception e) {
                // 异常处理
            } finally {
                // 释放锁
                lockRedis.delete(key);
            }
        } else {
            // sleep50ms后，进行重试
            Thread.sleep(50);
            return getData(key);
        }
    }
    return value;
}
```

2、**热点数据不过期**。直接将缓存设置为不过期，然后由定时任务去异步加载数据，更新缓存。

这种方式适用于比较极端的场景，例如流量特别特别大的场景，使用时需要考虑业务能接受数据不一致的时间，还有就是异常情况的处理，不要到时候缓存刷新不上，一直是脏数据，那就凉了。

## 缓存雪崩

大量的热点 key 设置了相同的过期时间，导在缓存在同一时刻全部失效，造成瞬时数据库请求量大、压力骤增，引起雪崩，甚至导致数据库被打挂。

缓存雪崩其实有点像“升级版的缓存击穿”，缓存击穿是一个热点 key，缓存雪崩是一组热点 key。

解决方案：

1、**过期时间打散**。既然是大量缓存集中失效，那最容易想到就是让他们不集中生效。可以给缓存的过期时间时加上一个随机值时间，使得每个 key 的过期时间分布开来，不会集中在同一时刻失效。

2、**热点数据不过期**。该方式和缓存击穿一样，也是要着重考虑刷新的时间间隔和数据异常如何处理的情况。

3、**加互斥锁**。该方式和缓存击穿一样，按 key 维度加锁，对于同一个 key，只允许一个线程去计算，其他线程原地阻塞等待第一个线程的计算结果，然后直接走缓存即可。

# 分布式锁

作者：阿里技术
链接：https://www.zhihu.com/question/300767410/answer/1749442787
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。



如果在一个分布式系统中，我们从数据库中读取一个数据，然后修改保存，这种情况很容易遇到并发问题。因为读取和更新保存不是一个原子操作，在并发时就会导致数据的不正确。这种场景其实并不少见，比如电商秒杀活动，库存数量的更新就会遇到。如果是单机应用，直接使用本地锁就可以避免。如果是分布式应用，本地锁派不上用场，这时就需要引入分布式锁来解决。

由此可见分布式锁的目的其实很简单，就是**为了保证多台服务器在执行某一段代码时保证只有一台服务器执行**。

为了保证分布式锁的可用性，至少要确保锁的实现要同时满足以下几点：

- 互斥性。在任何时刻，保证只有一个客户端持有锁。
- 不能出现死锁。如果在一个客户端持有锁的期间，这个客户端崩溃了，也要保证后续的其他客户端可以上锁。
- 保证上锁和解锁都是同一个客户端。

一般来说，实现分布式锁的方式有以下几种：

- 使用MySQL，基于唯一索引。
- 使用ZooKeeper，基于临时有序节点。
- **使用Redis，基于setnx命令**。

本篇文章主要讲解Redis的实现方式。

## 实现思路

Redis实现分布式锁主要利用Redis的`setnx`命令。`setnx`是`SET if not exists`(如果不存在，则 SET)的简写。

```bash
127.0.0.1:6379> setnx lock value1 #在键lock不存在的情况下，将键key的值设置为value1
(integer) 1
127.0.0.1:6379> setnx lock value2 #试图覆盖lock的值，返回0表示失败
(integer) 0
127.0.0.1:6379> get lock #获取lock的值，验证没有被覆盖
"value1"
127.0.0.1:6379> del lock #删除lock的值，删除成功
(integer) 1
127.0.0.1:6379> setnx lock value2 #再使用setnx命令设置，返回0表示成功
(integer) 1
127.0.0.1:6379> get lock #获取lock的值，验证设置成功
"value2"
```

上面这几个命令就是最基本的用来完成分布式锁的命令。

加锁：使用`setnx key value`命令，如果key不存在，设置value(加锁成功)。如果已经存在lock(也就是有客户端持有锁了)，则设置失败(加锁失败)。

解锁：使用`del`命令，通过删除键值释放锁。释放锁之后，其他客户端可以通过`setnx`命令进行加锁。

key的值可以根据业务设置，比如是用户中心使用的，可以命令为`USER_REDIS_LOCK`，value可以使用uuid保证唯一，用于标识加锁的客户端。保证加锁和解锁都是同一个客户端。

那么接下来就可以写一段很简单的加锁代码：

```java
private static Jedis jedis = new Jedis("127.0.0.1");

private static final Long SUCCESS = 1L;

/**
  * 加锁
  */
public boolean tryLock(String key, String requestId) {
    //使用setnx命令。
    //不存在则保存返回1，加锁成功。如果已经存在则返回0，加锁失败。
    return SUCCESS.equals(jedis.setnx(key, requestId));
}

//删除key的lua脚本，先比较requestId是否相等，相等则删除
private static final String DEL_SCRIPT = "if redis.call('get', KEYS[1]) == ARGV[1] then return redis.call('del', KEYS[1]) else return 0 end";

/**
  * 解锁
  */
public boolean unLock(String key, String requestId) {
    //删除成功表示解锁成功
    Long result = (Long) jedis.eval(DEL_SCRIPT, Collections.singletonList(key), Collections.singletonList(requestId));
    return SUCCESS.equals(result);
}
```

![image-20210514145131765](https://gitee.com/mw515031/image/raw/master/image/image-20210514145131765.png)

## 问题一

这仅仅满足上述的第一个条件和第三个条件，保证上锁和解锁都是同一个客户端，也保证只有一个客户端持有锁。

但是第二点没法保证，因为如果一个客户端持有锁的期间突然崩溃了，就会导致无法解锁，最后导致出现死锁的现象。

![image-20210514145151633](https://gitee.com/mw515031/image/raw/master/image/image-20210514145151633.png)

所以要有个超时的机制，在设置key的值时，需要加上有效时间，如果有效时间过期了，就会自动失效，就不会出现死锁。然后加锁的代码就会变成这样。

```java
public boolean tryLock(String key, String requestId, int expireTime) {
    //使用jedis的api，保证原子性
    //NX 不存在则操作 EX 设置有效期，单位是秒
    String result = jedis.set(key, requestId, "NX", "EX", expireTime);
    //返回OK则表示加锁成功
    return "OK".equals(result);
}
```

![image-20210514145221251](https://gitee.com/mw515031/image/raw/master/image/image-20210514145221251.png)

但是聪明的同学肯定会问，有效时间设置多长，假如我的业务操作比有效时间长，我的业务代码还没执行完就自动给我解锁了，不就完蛋了吗。

这个问题就有点棘手了，在网上也有很多讨论，第一种解决方法就是靠程序员自己去把握，预估一下业务代码需要执行的时间，然后设置有效期时间比执行时间长一些，保证不会因为自动解锁影响到客户端业务代码的执行。

但是这并不是万全之策，比如网络抖动这种情况是无法预测的，也有可能导致业务代码执行的时间变长，所以并不安全。

有一种方法比较靠谱一点，就是给锁续期。在Redisson框架实现分布式锁的思路，就使用watchDog机制实现锁的续期。当加锁成功后，同时开启守护线程，默认有效期是30秒，每隔10秒就会给锁续期到30秒，只要持有锁的客户端没有宕机，就能保证一直持有锁，直到业务代码执行完毕由客户端自己解锁，如果宕机了自然就在有效期失效后自动解锁。

![image-20210514145243172](https://gitee.com/mw515031/image/raw/master/image/image-20210514145243172.png)

作者：阿里技术
链接：https://www.zhihu.com/question/300767410/answer/1749442787
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。



## 问题二

但是聪明的同学可能又会问，你这个锁只能加一次，不可重入。可重入锁意思是在外层使用锁之后，内层仍然可以使用，那么可重入锁的实现思路又是怎么样的呢？

在Redisson实现可重入锁的思路，使用Redis的哈希表存储可重入次数，当加锁成功后，使用`hset`命令，value(重入次数)则是1。

```lua
"if (redis.call('exists', KEYS[1]) == 0) then " +
"redis.call('hset', KEYS[1], ARGV[2], 1); " +
"redis.call('pexpire', KEYS[1], ARGV[1]); " +
"return nil; " +
"end; "
```

如果同一个客户端再次加锁成功，则使用`hincrby`自增加一。

```lua
"if (redis.call('hexists', KEYS[1], ARGV[2]) == 1) then " +
"redis.call('hincrby', KEYS[1], ARGV[2], 1); " +
"redis.call('pexpire', KEYS[1], ARGV[1]); " +
"return nil; " +
"end; " +
"return redis.call('pttl', KEYS[1]);"
```

![image-20210514145323207](https://gitee.com/mw515031/image/raw/master/image/image-20210514145323207.png)

解锁时，先判断可重复次数是否大于0，大于0则减一，否则删除键值，释放锁资源。

```java
protected RFuture<Boolean> unlockInnerAsync(long threadId) {
    return commandExecutor.evalWriteAsync(getName(), LongCodec.INSTANCE, RedisCommands.EVAL_BOOLEAN,
"if (redis.call('hexists', KEYS[1], ARGV[3]) == 0) then " +
"return nil;" +
"end; " +
"local counter = redis.call('hincrby', KEYS[1], ARGV[3], -1); " +
"if (counter > 0) then " +
"redis.call('pexpire', KEYS[1], ARGV[2]); " +
"return 0; " +
"else " +
"redis.call('del', KEYS[1]); " +
"redis.call('publish', KEYS[2], ARGV[1]); " +
"return 1; "+
"end; " +
"return nil;",
Arrays.<Object>asList(getName(), getChannelName()), LockPubSub.UNLOCK_MESSAGE, internalLockLeaseTime, getLockName(threadId));
}
```

![image-20210514145351816](https://gitee.com/mw515031/image/raw/master/image/image-20210514145351816.png)

为了保证操作原子性，加锁和解锁操作都是使用lua脚本执行。

## 问题三

上面的加锁方法是加锁后立即返回加锁结果，如果加锁失败的情况下，总不可能一直轮询尝试加锁，直到加锁成功为止，这样太过耗费性能。所以需要利用发布订阅的机制进行优化。

步骤如下：

当加锁失败后，订阅锁释放的消息，自身进入阻塞状态。

当持有锁的客户端释放锁的时候，发布锁释放的消息。

当进入阻塞等待的其他客户端收到锁释放的消息后，解除阻塞等待状态，再次尝试加锁。

![image-20210514145425231](https://gitee.com/mw515031/image/raw/master/image/image-20210514145425231.png)

## 总结

以上的实现思路仅仅考虑在单机版Redis上，如果是集群版Redis需要考虑的问题还要再多一点。Redis由于他的高性能读写能力，所以在并发高的场景下使用Redis分布式锁会多一点。

问题一，二，三其实就是redis分布式锁不断改良发展的过程，第一个问题是设置有效期防止死锁，并且引入守护线程给锁续期，第二个问题是支持可重入锁，第三个问题是加锁失败后阻塞等待，等锁释放后再次尝试加锁。Redisson框架解决这三个问题的思路也非常值得学习。



作者：阿里技术
链接：https://www.zhihu.com/question/300767410/answer/1749442787
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。







































