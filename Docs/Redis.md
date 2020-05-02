## Redis

#### 1.Redis是什么?

Redis是一个开放源代码（BSD许可）的内存中数据结构存储，可用作数据库，缓存和消息代理，是一个基于键值对的NoSQl数据库。

#### 2.Redis特性?

- 速度快
- 基于键值对的数据结构服务器
- 丰富的功能、丰富的数据结构
- 简单稳定
- 客户端语言多
- 持久化
- 主从复制
- 高可以 & 分布式

#### 3.Redis合适的应用场景？

- 缓存
- 排行榜
- 计数器
- 分布式会话
- 分布式锁
- 社交网络
- 最新列表
- 消息系统

#### 4.除了Redis你还知道哪些NoSQL数据库？

MongoDB、MemcacheDB、Cassandra、CouchDB、Hypertable、Leveldb。

#### 5.Redis和Memcache区别？

支持的存储类型不同，memcached只支持简单的k/v结构。redis支持更多类型的存储结构类型(详见问题6)。

memcached数据不可恢复，redis则可以把数据持久化到磁盘上。

新版本的redis直接自己构建了VM 机制 ，一般的系统调用系统函数的话，会浪费一定的时间去移动和请求。 

redis当物理内存用完时，可以将很久没用到的value交换到磁盘。

#### 6.Redis的有几种数据类型？

基础：字符串（String）、哈希（hash)、列表（list)、集合(set)、有序集合(zset)。

还有HyperLogLog、流、地理坐标等。

#### 7.Redis有哪些高级功能？

消息队列、自动过期删除、事务、数据持久化、分布式锁、附近的人、慢查询分析、Sentinel 和集群等多项功能。

#### 8.安装过Redis吗,简单说下步骤？

 1.下载Redis指定版本源码安装包压缩到当前目录。

1. 解压缩Redis源码安装包。

2. 建立一个redis目录软链接，指向解压包。

3. 进入redis目录

4. 编译

5. 安装

   对于使用docker的童靴来说就比较容易了。

   docker pull redis

#### 9.redis几个比较主要的可执行文件？分别是？

![image-20200425221430652](https://gitee.com/yizhibuerdai/Imagetools/raw/master/images/image-20200425221430652.png)

#### 10.启动Redis的几种方式？

1.默认配置 :   

./redis-server 

2.运行启动: redis-server 加上要修改配置名和值（可以是多对），没有配置的将使用默认配置。

例如: redis-server ———port 7359

3.指定配置文件启动:

./redis-server /opt/redis/redis.conf

#### 11.Redis配置需要自己写？如何配置？

redis目录下有一个redis.conf的模板配置。所以只需要复制模板配置然后修改即可。

一般来说大部分生产环境都会用指定配置文件的方式启动redis。

#### 12.Redis客户端命令执行的方式？

1.交互方式: 

```
redis-cli -h 127.0.0.1 -p 6379
```

连接到redis后，后面执行的命令就可以通过交互方式实现了。

2.命令行方式：

```
redis-cli -h 127.0.0.1 -p 6379 get value
```

#### 13.如何停止redis服务？

Kill -9 pid (粗暴，请不要使用,数据不仅不会持久化，还会造成缓存区等资源不能被优雅关闭)

可以用redis 的shutdown 命令，可以选择是否在关闭前持久化数据。

```
redis-cli shutdown nosave|save
```

#### 14.如何查看当前键是否存在？

exists key

#### 15.如何删除数据？

del key

#### 16.redis为什么快？单线程？

- redis使用了单线程架构和I/O多路复用模型模型。
- 纯内存访问。
- 由于是单线程避免了线程上下文切换带来的资源消耗。

#### 17.字符串最大不能超过多少？

512MB

#### 18.redis默认分多少个数据库？

16 

#### 19.redis持久化的几种方式？

RDB、AOF、混合持久化。

#### 20.RDB持久化?

RDB（Redis DataBase)持久化是把当前进程数据生成快照保存到硬盘的过程。

Tips:是以二进制的方式写入磁盘。

#### 21.RDB的持久化是如何触发的？

手动触发: 

save: 阻塞当前Redis服务器，直到RDB过程完成为止，如果数据比较大的话，会造成长时间的阻塞，

线上不建议。

bgsave:redis进程执行 fork操作创作子进程，持久化由子进程负责，完成后自动结束，阻塞只发生在

fork阶段，一半时间很短。

自动触发：

save xsecends n:

表示在x秒内，至少有n个键发生变化，就会触发RDB持久化。也就是说满足了条件就会触发持久化。

flushall :

主从同步触发

#### 22.RDB的优点？

- rdb是一个紧凑的二进制文件，代表Redis在某个时间点上的数据快照。
- 适合于备份，全量复制的场景，对于灾难恢复非常有用。
- Redis加载RDB恢复数据的速度远快于AOF方式。

#### 23.RDB的缺点？

- RDB没法做到实时的持久化。中途意外终止，会丢失一段时间内的数据。
- RDB需要fork()创建子进程，属于重量级操作，可能导致Redis卡顿若干秒。

#### 24.如何禁用持久化？

一般来说生成环境不会用到，了解一下也有好处的。

```
config set save ""
```

#### 25.AOF持久化？

AOF(append only file)为了解决rdb不能实时持久化的问题，aof来搞定。以独立的日志方式记录把每次命令记录到aof文件中。

#### 26.如何查询AOF是否开启?

```
config get appendonly
```

#### 27.如何开启AOF?

命令行方式： 实时生效，但重启后失效。

```
config set appendonly
```

配置文件：需要重启生效，重启后依然生效。

```
appendonly yes
```

#### 28.AOF工作流程？

1.所有写入命令追加到aof_buf缓冲区。

2.AOF缓冲区根据对应的策略向硬盘做同步操作。

3.随着AOF文件越来越大，需要定期对AOF文件进行重写，达到压缩的目的。

4.当redis服务器重启时，可以加载AOF文件进行数据恢复。

#### 29.为什么AOF要先把命令追加到缓存区(aof_buf)中？

Redis使用单线程响应命令，如果每次写入文件命令都直接追加到硬盘，性能就会取决于硬盘的负载。如果使用缓冲区，redis提供多种缓冲区策略，在性能和安全性方面做出平衡。

#### 30.AOF持久化如何触发的？

自动触发：满足设置的策略和满足重写触发。

策略：(在配置文件中配置)

![image-20200427101221526](https://gitee.com/yizhibuerdai/Imagetools/raw/master/images/image-20200427101221526.png

手动触发：（执行命令）

```
bgrewriteaof 
```

#### 31.AOF优点？

- AOF提供了3种保存策略：每秒保存、跟系统策略、每次操作保存。实时性比较高，一般来说会选择每秒保存，因此意外发生时顶多失去一秒的数据。
- 文件追加写形式，所以文件很少有损坏问题，如最后意外发生少写数据，可通过redis-check-aof工具修复。
- AOF由于是文本形式，直接采用协议格式，避免二次处理开销，另外对于修改也比较灵活。

#### 32.AOF缺点？

- AOF文件要比RDB文件大。
- AOF冷备没RDB迅速。
- 由于执行频率比较高，所以负载高时，性能没有RDB好。

#### 33.混合持久化？优缺点？

一般来说我们的线上都会采取混合持久化。redis4.0以后添加了新的混合持久化方式。

优点：

- 在快速加载的同时，避免了丢失过更多的数据。

缺点：

- 由于混合了两种格式，所以可读性差。
- 兼容性，需要4.0以后才支持。

#### 34.Redis的Java客户端官方推荐？实际选择？

官方推荐的有3种：Jedis、Redisson和lettuce。

一般来说用的比较多的有:Jedis|Redisson。

Jedis：更轻量、简介、不支持读写分离需要我们来实现，文档比较少。API提供了比较全面的Redis命令的支持。

Redisson：基于Netty实现，性能高，支持异步请求。提供了很多分布式相关操作服务。高级功能能比较多，文档也比较丰富，但实用上复杂度也相对高。和Jedis相比，功能较为简单，不支持字符串操作，不支持排序、事务、管道、分区等Redis特性。

#### 35.Redis事务？

事务提供了一种将多个命令请求打包，一次性、按顺序的执行多个命令的机制。并且在事务执行期间，服务器不会中断事务而改去执行其他客户端命令请求，它会

#### 36.Redis事务开始到结束的几个阶段？

- 开启事务
- 命令入队
- 执行事务/放弃事务

#### 37.Redis中key的过期操作？

​    设置key的生存时间为n秒

```
expire key nseconds
```

​    设置key的生存时间为nmilliseconds

```
pxpire key milliseconds
```

​	设置过期时间为timestamp所指定的秒数时间戳

```
expireat key timespamp
```

  设置过期时间为timestamp毫秒级时间戳

```
pexpireat key millisecondsTimestamp
```

#### 38.Redis过期键删除策略?

定时删除：在设置的过期时间同时，创建一个定时器在键的过期时间来临时，立即执行队键的操作删除。

惰性删除：放任过期键不管，但每次从键空间中获取键时，都检查取得的键是否过期，如果过期就删除，如果没有就返回该键。

定期删除：每隔一段时间执行一次删除过期键操作，并通过先吃删除操作执行的时长和频率来减少删除操作对cpu时间的影响。

#### 39.Pipeline是什么？为什么要它？

命令批处理技术，对命令进行组装，然后一次性执行多个命令。

可以有效的节省RTT(Round Trip Time 往返时间)。

经过测试验证：

- pipeline执行速度一般比逐条执行快。
- 客户端和服务的网络延越大，pipeline效果越明显。

#### 40.如何获取当前最大内存？如何动态设置？

获取最大内存:

```
config get maxmemory
```

设置最大内存：

命令设置:

```
config set maxmemory 1GB
```

#### 41.Redis内存溢出控制？

当Redis所用内存达到maxmemory上限时，会出发相应的溢出策略。

#### 42.Redis内存溢出策略？

1.noeviction(默认策略):拒绝所有写入操作并返回客户端错误信息（error) OOM command not allowed     when used memory,只响应读操作。

1. volatile-lru:根据LRU算法删除设置了超时属性（expire)的键，直到腾出足够空间为止。如果没有可删除的键对象，回退到noeviction策略。
2. allkeys-lru:根据LRU算法删除键，不管数据有没有设置超时属性， 直到腾出足够空间为止。
3. allkeys-random:随机删除所有键，直到腾出足够空间为止。
4. volatile-random:随机删除过期键，直到腾出足够空间为止。
5. volatile-tth根据键值对象的ttl属性，删除最近将要过期数据。如果没有，回退到noeviction策略。

#### 43.Redis高可用方案？

Redis Sentinel(哨兵)能自动完成故障发现和转移。

#### 44.Redis集群方案？

Twemproxy、Redis Cluster、Codis。

#### 45.Redis Cluster槽范围？

0~16383

#### 46.Redis锁实现思路?

setnx (set if not exists),如果创建成功则表示获取到锁。

setnx lock true 创建锁

del lock 释放锁

如果中途崩溃，无法释放锁？

此时需要考虑到超时时间的问题。比如 :expire lock 300

由于命令是非原子的，所以还是会死锁,如何解决？

Redis 支持 set 并设置超时时间的功能。

比如: set lock true  ex 30 nx

#### 47.什么是布隆过滤器？

是1970年由布隆提出的。它实际上是一个很长的二进制向量和一系列随机映射函数。布隆过滤器可以用于检索一个元素是否在一个集合中。它的优点是空间效率和查询时间都比一般的算法要好的多，缺点是有一定的误识别率和删除困难。

Tips:当判断一定存在时，可能会误判，当判断不存在时，就一定不存在。

#### 48.什么是缓存穿透？处理问题？

缓存穿透：缓存层不命中，存储层不命中。

处理方式1:缓存空对象，不过此时会占用更多内存空间，所以根据大家业务特性去设置超时时间来控制内存占用的问题。

处理方式2:布隆过滤器。

#### 49.什么是缓存预热？

就是系统上线后，提前将相关数据加载到缓存系统，避免用户先查库，然后在缓存。

#### 50.什么是缓存雪崩？处理问题？

缓存雪崩：由于缓存层承载着大量请求，有效的保护了存储层，但如果存储层由于某些原因不能提供服务，存储层调用暴增，造成存储层宕机。

处理：

- 保证缓存层服务高可用性。
- 对缓存系统做实时监控，报警等。
- 依赖隔离组件为后端限流并降级。
- 做好持久化，以便数据的快速恢复。

参考：

- 《Redis深度历险:核心原理和应用实践》
- 《Redis开发与运维》
- 《Redis设计与实现》
- https://redis.io/
- 百度百科

![WechatIMG360](https://gitee.com/yizhibuerdai/Imagetools/raw/master/images/common1.png)