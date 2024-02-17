# 初识redis

**口诀**

```css
lpush+lpop=Stack（栈）
lpush+rpop=Queue（队列）
lpsh+ltrim=Capped Collection（有限集合）
lpush+brpop=Message Queue（消息队列）
```

***Redis的8个重要特性***

1. **速度快**

   + Redis的所有数据都是存放在内存中的
   + 使用C语言实现
   + 使用单线程架构，预防了多线程可能产生的竞争问题

2. **基于键值对的数据结构服务器**

   几乎所有的编程语言都提供了类似字典的功能，类似于这种组织数据的方式叫作基于键值的方式

   Redis中的值不仅可以是字符串，而且还可以是具体的数据结构

   Redis的全称是REmote Dictionary Server(远程字典服务器)

3. **丰富功能**

   + 提供了键过期功能，可以用来实现缓存
   + 提供发布订阅功能，可以用来实现消息系统
   + 支持Lua脚本功能，可以利用Lua创造出新的Redis命令
   + 提供简单的事务功能，能在一定程度上保证事务特性
   + 提供了流水线功能，这样客户端能将一批命令一次性传到Redis，减少了网络的开销

4. **简单稳定**

   Redis的源码很少，早期版本的代码只有2万行左右，3.0版本以后由于添加了集群特性，代码增至5万行左右

   Redis使用单线程模型，不仅使Redis服务端处理模型变得简单，而且也使客户端开发变得简单

5. **客户端语言多**

   Redis提供了简单的TCP通信协议，很多编程语言可以很方便地接入到 Redis

6. **持久化**

   Redis提供了两种持久化方式：RDB和AOF，即可以用两种策略将内存的数据保存到硬盘中

7. **主从复制**

   Redis提供了复制功能，实现了多个相同数据的Redis副本，复制功能是分布式Redis的基础

8. **高可用和分布式**

   Redis2.8版本提供了高可用实现Redis Sentinel，它能够保证Redis节点的故障发现和故障自动转移

   Redis3.0版本提供了分布式实现Redis Cluster，它是Redis真正的分布式实现，提供了高可用、读写和容量的扩展性

***redis的作用***

1. **缓存**

   缓存机制几乎在所有的大型网站都有使用，合理地使用缓存不仅可以加快数据的访问速度，而且能够有效地降低后端数据源的压力

2. **排行榜系统**

   Redis提供了列表和有序集合数据结构，合理地使用这些数据结构可以很方便地构建各种排行榜系统

3. **计数器应用**

   为了保证数据的实时性，每一次播放和浏览都要做加1的操作，如果并发量很大对于传统关系型数据的性能是一种挑战。Redis天然支持计数 功能而且计数的性能也非常好，是计数器系统的重要选择

4. **社交网络**

   由于社交网站访问量通常比较大，而且传统的关系型数据不太适合保存这种类型的数据，Redis提供的数据结构可以相对比较容易地实现这些功 能

5. **消息队列系统**

   Redis提供了发布订阅功能和阻塞队列的功能，虽然和专业的消息队列比还不够足够强大，但是对于一般的消息队列功能基本可以满足

***用好redis的建议***

1. **切勿当作黑盒使用，开发与运维同样重要**

   很多线上的故障和问题都是由于完全把Redis当做黑盒造成的

   在很多公司内只有专职的关系型数据库DBA，并没有NoSQL的相关运维人员，也就是说开发者很有可能会自己运维Redis

2. **阅读源码**

   通过阅读优秀的源码，不仅能够加深我们对于Redis的理解，而且还能提高自身的编码水平， 甚至可以对Redis做定制化，也就是说可以修改Redis的源码来满足自身的需求

**Redis可执行文件说明**

```shell
redis-server #启动redis
redis-cli #redis命令行客户端
redis-benchmark #redis基准测试工具
redis-check-aof #redisAOF持久化文件检测和修复丁且
redis-check-dump #redisRDB持久化文件检测和修复工具
redis-sentine1 #启动redisSentinel
```

**启动redis**

1. **默认配置**

   ```shell
   #使用Redis的默认配置来启动
   #找到redis-server所在目录，redis7在src目录下
   ./redis-server
   ```

2. **运行配置**

   ```shell
   #redis-server加上要修改配置名和值（可以是多对）
   #例如如果要用6380作为端口启动Redis，那么可以执行
   redis-server --port 6380
   ```

   虽然运行配置可以自定义配置，但如果需要修改的配置较多或者希望将配置保存到文件中，不建议使用

3. **配置文件启动**

   将配置写到指定文件里，Redis建议使用配置文件来启动， 因为直接启动无法自定义配置

   ```shell
   redis-server redis.conf文件路径
   ```

**Redis命令行客户端**

```SHELL
#交互方式
redis-cli -h host -p port
#命令方式
redis-cli -h host -p port command
可以直接得到命令的返回结果
```

没有-h参数，默认连接127.0.0.1；没有-p参数，默认6379端口

**停止Redis服务**

```shell
redis-cli shutdown
#可选参数表示是否在关闭redis前，生成持久化文件
redis-cli shutdown nosave|save
```

Redis关闭的过程：断开与客户端的连接、持久化文件生成

可以通过kill进程号的方式关闭掉Redis，但是不要粗暴地使用kill-9强制杀死Redis服务，会造成AOF和复制丢失数据的情况

# API理解与使用

## 预备知识

**全局命令**

```sql
#查看所有键
key*

#查看键总数
dbsize
dbsize命令在计算键总数时是直接获取Redis内置的键总数变量，dbsize命令的时间复杂度是O（1），而keys命令会遍历所有键，时间复杂度是O（n）

#检查键是否存在
exists key
键存在返回1，不存在返回0

#删除键
del key [key...]
del a b c #del支持删除多个键
del是一个通用命令，无论值是什么数据结构类型，del命令都可以将其
删除
返回值为成功删除键的个数

#设置键过期时间
redis支持对键添加过期时间，当超过过期时间后，会自动删除键
>set Hello world
ok
>expire Hello 10 #键hello设置了10秒过期时间
>ttl Hello
8
可以通过ttl命令观察键的剩余过期时间，有3种返回值：
>=0的整数：键的剩余过期时间
-1：键未设置过期时间
-2：键不存在

#查看键的数据结构类型
type key
>set a
>type a
string
>rpush mylist A B C D
>type mylist
list
>type b
none #如果键不存在返回none
```

**数据结构和内部编码**

redis对外的数据结构：

string字符串、hash哈希、list列表、set集合、zset有序集合

![](https://img2024.cnblogs.com/blog/3357226/202402/3357226-20240204224820950-809200940.png)

每种数据结构都有自己底层的内部编码实现，而且是多种实现， 这样Redis会在合适的场景选择合适的内部编码

```sql
#查看内部编码
object encoding key
>object encoding Hello
"embstr"
```

![](https://img2024.cnblogs.com/blog/3357226/202402/3357226-20240204224930231-133135633.png)

Redis这样设计有两个好处：

1. 可以改进内部编码，而对外的数据结构和命令没有影响，这样一旦开发出更优秀的内部编码，无需改动外部数据结构和命令
2. 多种内部编码实现可以在不同场景下发挥各自的优势，例如`ziplist`比较节省内存，但是在列表元素比较多的情况下，性能会有所下降，这时候Redis会根据配置选项将列表类型的内部实现转换为`linkedlist`

**单线程架构**

```sql
#client1设置一个字符串键值对
>set hello world
#client2对counter做自增操作
>incr counter
#client3对counter做自增操作
>incr counter
```

每次客户端调用都经历了发送命令、执行命令、返回结果三个过程

Redis是单线程来处理命令的，所以一条命令从客户端达到服务端不会立刻被执行，所有命令都会进入一个队列中，然后逐个被执行。所以上面3个客户端命令的执行顺序是不确定的，但是可以确定不会有两条命令被同时执行，所以两条`incr`命令无论怎么执行最终结果都是2，不会产生并发问题

## 字符串

键都是字符串类型，其他几种数据结构都是在字符串类型基础上构建的

字符串类型的值实际可以是字符串（简单的字符串、复杂的字符串例如JSON、XML）、数字 （整数、浮点数）、二进制（图片、音频、视频），但是值最大不能超过512MB

​														***字符串数据结构***

![](https://img2024.cnblogs.com/blog/3357226/202402/3357226-20240205124203748-785325249.png)

**设置值**

```sql
set key value [ex seconds] [px milliseconds] [nx|xx]
ex seconds：设置秒级过期时间
px milliseconds：设置毫秒级过期时间
nx：键不存在才设置，用于添加
xx：键存在才设置，用于更新
>set Hello
OK #返回OK表示设置成功，0为失败

setex key seconds value #setex命令作用和ex相同
setnx key value #setnx命令和nx相同

#批量设置值
mset key value [key value ...]
```

**获取值**

```sql
get key #如果获取的键不存在则返回nil

#批量获取值
mget key [key ...] #结果是按照传入键的顺序返回
```

**计数**

```sql
incr key #自增
值不是整数，返回错误
值是整数，返回自增后结果
键不存在，按照值为0自增，返回1

decr key #自减

incrby key increment #自增指定数字

decrby key decrement #自减指定数字

incrbyfloat key increment #自增指定浮点数
```

**其他命令**

```sql
#追加值
append key value

#字符串长度
strlen key 
注意每个中文占3字节，所以1个汉字返回3长度

#设置并返回原值
getset key value
getset和set一样会设置值，但不同的是，它同时回显键原来的值

#设置指定位置的字符
setrange key offeset value
>set name wwj
ok
>setrange name 0 x
(integer) 3
>get name
xwj

#获取部分字符串
getrange key start end
分别是开始和结束的偏移量，偏移量从0开始计算
>getrange name 1 2
wj

```

**内部编码**

```css
int：8bytes长整型
embstr：<=39bytes字符串
raw：>39bytes字符串
```

Redis会根据当前值的类型和长度决定使用哪种内部编码实现

```sql
object encoding key #查看键编码
```

**使用场景**

1. ***缓存功能***

   Redis作为缓存层，MySQL作为存储层，绝大部分请求的数据都是从Redis中获取。由于Redis具有支撑高并发的特性，所以缓存通常能起到加速读写和降低后端压力的作用

   ![](https://img2024.cnblogs.com/blog/3357226/202402/3357226-20240205131804937-2129925420.png)

   设计合理的键名，有利于防止键冲突和项目的可维护性：

   ```sql
   #比较推荐的方式是使用“业务名：对象 名：id：[属性]”作为键名（也可以不是分号）
   例如MySQL的数据库名为vs，用户表名为user，那么对应的键可以用
   "vs：user：1"或"vs：user：1： name"
   
   #如果键名较长，例如“user：{uid}：friends：messages：{mid}”
   可以在能描述键含义的前提下适当减少键的长度：
   u：{uid}：fr：m： {mid}
   从而减少由于键过长的内存浪费
   ```

2. ***计数***

   许多应用都会使用Redis作为计数的基础工具，它可以实现快速计数、 查询缓存的功能，同时数据可以异步落地到其他数据源

3. ***共享session***

   可以使用Redis将用户的Session进行集中管理，在这种模式下只要保证Redis是高可用和扩展性的，每次用户更新或者查询登录信息都直接从Redis中集中获取

4. ***限速***

   例如为了短信接口不被频繁访问，会限制用户每分钟获取验证码的频率，例如一分钟不能超过5次

## 哈希

在Redis中，哈希类型是指键值本身又是一个键值对结构

![](https://img2024.cnblogs.com/blog/3357226/202402/3357226-20240205132641773-1935611071.png)

哈希类型中的映射关系叫作field-value，这里的value是指field对应的值，不是键对应的值

```sql
#设置值
hset key field value
>hset user:1 name mike
(integer) 1 #成功返回1，反之返回0

hestnx key field value #如果不存在才执行，成功返回1，反之返回0
它们的关系就像set和setnx命令一样，只不过作用域由键变为field

#获取值
hget key field
>hget user:1 name
mike #如果键或field不存在返回nil

#删除field
hdel key field [field ...] #返回成功删除field的个数

#查看field个数
hlen key
hset user:1 name tom
(integer) 1
>hset user:1 age 23
(integer) 1
>hset user:1 city tianjin
(integer) 1
>hlen user:1
(integer) 3

#批量设置field-value
hmset key field value [field value ...] 
#批量获取field-value
hmget key field [field ...] 
>hmset user:1 name mike age 12 city xian
OK
>hmget user:1 name age city
mike
12
xian

#查看field是否存在
hexists key field
>hexists user:1 name
(integer) 1 #包含name域，返回1，不包含返回0

#获取所有field
hkeys key #返回键的所有field

#获取所有value
hvals key #返回键所有field的所有value

#获取所有的field-value
hgetall key

#获取指定value
hmget key field [field ...]
>hmget user:1 name city
mike
xian
```

在使用`hgetall`时，如果哈希元素个数比较多，会存在阻塞Redis的可能。 如果开发人员只需要获取部分`field`，可以使用`hmget`，如果一定要获取全部`field-value`，可以使用`hscan`命令，该命令会渐进式遍历哈希类型

```sql
hincrby key field increment #自增指定值
hincrbyfloat key field increment #自增指定浮点数值
#就像incrby和incrbyfloat命令一样，但是它们的作用域是filed
```

**`hash`没有递减，可以将自增操作值设为负值即可实现减**

```sql
#计算value的字符串长度
hstrlen key field
>hstrlen user:1 name
(integer) 4 #name是mike
```

**内部编码**

```css
ziplist(压缩列表)
/*当哈希类型元素个数小于hash-max-ziplist-entries配置（默认512个）
同时所有值都小于hash-max-ziplist-value配置（默认64字节）时
redis会使用ziplist作为哈希的内部实现*/
field<512 && all value <64bytes
ziplist使用更加紧凑的结构实现多个元素的连续存储，更节省内存

hashtable(哈希表)
/*当哈希类型无法满足ziplist的条件时，Redis会使用hashtable作为哈希的内部实现*/
因为此时ziplist的读写效率会下降，而hashtable的读写时间复杂度为O(1)

```

**使用场景**

相比于使用字符串序列化缓存用户信息，哈希类型变得更加直观，并且在更新操作上会更加便捷。可以将每个用户的id定义为键后缀，多对`field-value`对应每个用户的属性

但哈希类型和关系型数据库有两点不同：

1. 哈希类型是稀疏的，而关系型数据库是完全结构化的，例如哈希类型 每个键可以有不同的`field`，而关系型数据库一旦添加新的列，所有行都要为其设置值（即使为`NULL`）
2. 关系型数据库可以做复杂的关系查询，而Redis去模拟关系型复杂查询 开发困难，维护成本高

**缓存用户信息的三种方法**

```sql
#原生字符串类型：每个属性一个键
set user:1:name tom
set user:1:age 23
set user:1:city beijing
#优点：简单直观，每个属性都支持更新操作
#缺点：占用过多的键，内存占用量较大，同时用户信息内聚性比较差，最rubbish的方法

#序列化字符串类型：将用户信息序列化后用一个键保存
set user:1 serialize(userInfo)
#优点：简化编程，合理使用序列化可以提高内存的使用效率
#缺点：序列化和反序列化有一定的开销，同时每次更新属性都需要把全部数据取出进行反序列化，更新后再序列化到Redis中

#哈希类型：每个用户属性使用一对field-value，但只用一个键保存
hmset user:1 name tom age 23 city beijing
#优点：简单直观，合理使用可以减少内存空间的使用
#缺点：要控制哈希在ziplist和hashtable两种内部编码的转换，hashtable会消耗更多内存
```

## 列表

`list`是用来存储多个有序的字符串，列表中的每个字符串称为元素element，一个列表最多存储2^32^-1个element

在redis中可以对列表两端插入push和弹出pop，还可以获取指定范围的元素列 表、获取指定索引下标的元素等。列表是一种比较灵活的数据结构，它可以充当栈和队列的角色

列表类型有两个特点：

1. 列表中的元素是有序的，索引从0开始
2. 列表中的元素可以是重复的

***添加操作***

```sql
#左push
lpush key value [value ...]
#右push
rpush key value [value ...]

#向某个元素前或后push
linsert key [before|after] pivot value
从列表中找到==pivot的元素，在其前或后插入value
```

***查找操作***

```sql
#获取指定范围内的元素列表
lrange key start end
end选项包含自身
索引下标从左到右是0~N-1，从右到左是-1~-N
>lrange list 1 3 #获取2~4个元素
>lrange list -1 -3 #获取最后3个元素，-1为最后一个元素

#获取列表指定索引下标的元素
lindex key index

#获取列表长度
llen key
```

***删除操作***

```sql
#左pop
lpop key 
#右pop
rpop key

#删除指定元素
lrem key count value
lrem从列表中找到==value的元素进行删除
count>0，从左到右，删除count个元素
count<0，从右到左，删除count个元素
count=0，删除所有

#保留指定范围内元素
ltrim key start end
保留start和end指定的元素范围，若超出范围则取最接近范围的值
>ltrim list 1 3 #只保留2~4元素
```

***修改操作***

```sql
#修改指定下标的元素
lset key index value
>lset list 3 Lua #将第2个元素设为Lua
```

***阻塞操作***

```sql
blpop key [key ...] timeout
brpop key [key ...] timeout
#分别是lpop和rpop的阻塞版本，除了弹出方向不同，使用方法基本相同
key[key ...]：多个列表的键
timeout：阻塞时间(unit:second)

#列表为空
>brpop list:test 3 #客户端等到3秒后返回
>brpop list:test 0 #客户端一直阻塞下去
如果期间添加数据，客户端立即返回
#列表不为空，客户端会立即返回

#如果是多个键，brpop会从左至右遍历键，一旦有一个能弹出元素，客户端立即返回
>brpop list:1 list:2 list:3 0 #client1使用brpop弹出元素
..阻塞..
#client2分别向list：2和list：3插入元素
>lpush list:2 element2
(integer) 1
> lpush list:3 element3
(integer) 1
那么client1会立即返回list：2中的element2，因为list：2最先有可以弹出的元素

#如果多个客户端对同一个键执行brpop，那么最先执行brpop命令的客户端可以获取到弹出的值
brpop list:test 0 #client1
brpop list:test 0 #client2

lpush list:test element #client3向list：test插入元素
(integer) 1
那么客户端1最会获取到元素，因为客户端1最先执行brpop，而客户端2继续阻塞
```

**内部编码**

```css
ziplist（压缩列表）
/*当列表的元素个数小于list-max-ziplist-entries配置（默认512个），同时列表中每个元素的值都小于list-max-ziplist-value配置时（默认64字节），Redis会选用ziplist来作为列表的内部实现来减少内存的使用*/
element<512 && all element value<64bytes

linkedlist（链表）
/*当列表类型无法满足ziplist的条件时，Redis会使用linkedlist作为列表的内部实现*/
```

**使用场景**

1. **消息队列**

   Redis的lpush+brpop命令组合即可实现阻塞队列，生产 者客户端使用lrpush从列表左侧插入元素，多个消费者客户端使用brpop命令阻塞式的“抢”列表尾部的元素，多个客户端保证了消费的负载均衡和高可用 性

2. **文章列表**

   分页展示文章列表。此时可以考虑使用列表，因为列表不但是有序的，同时支持按照索引范围获取元素

## 集合

`set`类型也是用来保存多个的字符串类型，但和列表类型不一样的是，集合中不允许有重复元素，并且集合中的元素是无序的，不能通过索引下标获取元素

一个集合最多可以存储2^32^ -1个元素，Redis除了支持集合内的增删改查，同时还支持多个集合取交集、并集、差集

**集合内操作**

```sql
#添加元素
sadd key element [element ...] #返回添加成功的元素个数

#删除元素
srem key element [element] #返回删除成功的元素个数

#计算元素个数
scard key #不会遍历集合所有元素，而是直接用Redis内部的变量

#判断元素是否在集合中
sismember key element #在集合内返回1，否则返回0

#从集合随机返回指定个数元素
srandmember key [count] #不指定个数默认为1

#从集合随机弹出指定个数元素
spop key [count] #不指定个数默认为1

#获取所有元素，返回结果是无序的
smembers key
```

`smembers`和`lrange`、`hgetall`都属于比较重的命令，如果元素过多存在阻塞Redis的可能性，这时候可以使用`sscan`来完成

**集合间操作**

```sql
#添加两个集合
>sadd user:1:follow it music his sports
(integer) 4
> sadd user:2:follow it news ent sports
(integer) 4

#求多个集合的交集
sinter key [key ...]
>sinter user:1:follow user:2:follow
1) "sports"
2) "it"

#求多个集合的并集
suinon key [key ...]

#求多个集合的差集
sdiff key [key ...]
9>sdiff user:1:follow user:2:follow
1) "music"
2) "his"

#保存交集结果到destination
sinterstore destination key [key ...]
#保存并集结果到destination
suionstore destination key [key ...]
#保存差集结果到destination
sdiffstore destination key [key ...]
如果destination集合已经存在，它将被覆盖
```

**内部编码**

```css
intset（整数集合）
/*当集合中的元素都是整数且元素个数小于set-maxintset-entries配置（默认512个）时，Redis会选用intset来作为集合的内部实现，从而减少内存的使用*/
all element==integer && element<512

hashtable（哈希表）
/*当集合类型无法满足intset的条件时，Redis会使用hashtable作为集合的内部实现*/
```

**使用场景**

集合类型比较典型的使用场景是标签，例如一个用户可能对娱乐、体育比较感兴趣，另一个用户可能对历史、新闻比较感兴趣，这些兴趣点就是标签

有了这些数据就可以得到喜欢同一个标签的人，以及用户的共同喜好的标签，这些数据对于用户体验以及增强用户黏度很重要

集合类型的应用场景通常为以下几种：

```css
sadd=Tagging（标签）
spop/srandmember=Random item（生成随机数，比如抽奖）
sadd+sinter=Social Graph（社交需求）
```

## 有序集合

保留了集合不能有重复成员的特性， 但有序集合中的元素可以排序

它和列表使用索引下标作为排序依据不同的是，它给每个元素设置一个分数score作为排序的依据

有序集合中的元素不能重复，但是score可以重复

![](https://img2024.cnblogs.com/blog/3357226/202402/3357226-20240205194911990-1491415735.png)

**集合内**

```sql
#添加成员
zadd key [option] score member [score member ...] #返回成功添加成员的个数
有4个选项：
nx：member不存在才可以设置，用于添加
xx：member必须存在才可以设置，用于更新
ch：返回此次操作后，元素和分数发生变化的个数
incr：对score自增
/*有序集合相比集合提供了排序字段，但是也产生了代价，zadd的时间复杂度为O(log(n))，sadd的时间复杂度为O(1)*/

#查看成员个数
zcard key

#查看某个成员的分数
zscore key member

#查看成员排名
zrank key member #分数从低到高返回排名
zrevrank key member #分数从高到低返回排名

#删除成员
zrem key member [member ...] #返回成功删除的个数

#增加成员的分数
zincrby key increment member

#返回指定排名范围的成员
zrange key start end [withscores] #从低到高返回
zrevrange key start end [withscores] #从高到低返回
加上withscores选项会同时返回成员的分数
zrevrange user:ranking 0 2 withscores #返回第1~3名成员和分数

#返回指定分数范围的成员
#从低到高返回
zrangebyscore key min max [withscores] [limit offset count]
#从高到低返回
zrevrangebyscore key max min [withscores] [limit offset count]
[limit offset count]选项可以限制输出的起始位置和个数
>zrevrange user:ranking 0 5 limit 0 2
#返回分数在0~5之间的前3个成员

#min和max还支持开区间（小括号）和闭区间（中括号）
#-inf和+inf分别代表无限小和无限大
>zrangebyscore user:ranking (200 +inf withscores
#返回分数在200以上的成员和分数      
                             
#返回指定分数范围成员个数
zcount key min max

#删除指定排名内的升序元素
zremrangebyrank key start end
>zremrangebyrank user:ranking 0 2 #删除第1到3名成员

#删除指定分数范围的成员
zremrangebyscore key min max #返回成功删除的个数
>zremrangebyscore user:ranking (250 +inf
#将250分以上的成员全部删除                
```

**集合间操作**

```sql
#交集
zinterstore destination numkeys key [key ...] [weights weight [weight ...]]
[aggregate sum|min|max]
destination：交集计算结果保存到这个键
numkeys：需要做交集计算键的个数
key[key...]：需要做交集计算的键
weights weight[weight...]：每个键的权重，在做交集计算时，每个键中的每个member会将自己分数乘以这个权重，每个键的权重默认是1
aggregate sum|min|max：计算成员交集后，分值可以按照sum（和）、
min（最小值）、max（最大值）做汇总，默认值是sum

>zinterstore user:ranking:1_inter_2 2 user:ranking:1
user:ranking:2 weights 1 0.5 aggregate max
#对user：ranking：1和user：ranking：2做交集

#并集
zunionstore destination numkeys key [key ...] [weights weight [weight ...]] [aggregate sum|min|max]
#该命令的所有参数和zinterstore是一致的，只不过是做并集计算
```

**权重影响最终有序集合中元素的排序和分数**

**内部编码**

```css
ziplist（压缩列表）
/*当有序集合的元素个数小于zset-max-ziplist-entries配置（默认128个），同时每个元素的值都小于zset-max-ziplist-value配置（默认64字节）时，Redis会用ziplist来作为有序集合的内部实现*/
element<128 && all element value<64bytes
ziplist可以有效减少内存的使用

skiplist（跳跃表）
/*当ziplist条件不满足时，有序集合会使用skiplist作为内部实现，因为此时ziplist的读写效率会下降*/
```

**使用场景**

有序集合比较典型的使用场景就是排行榜系统

## 键管理

### 单个键管理

```sql
#键重命名
rename key newkey
如果在rename之前，新名的键已存在，那么它的值将被覆盖
>mset a1 a a2 b
ok
>rename a1 a2
OK
>get a1
(nil)
>get a2
"a"
#Redis提供了renamenx命令，确保只有newKey不存在时候才重命名
renamenx key newkey
由于重命名键期间会执行del命令删除旧的键，如果键对应的值比较大，会存在阻塞Redis的可能性

#随机返回一个键
randomkey
```

**键过期**

```SQL
#键在seconds秒后过期
expire key seconds 
>expire Hello 10 #设置10秒过期时间

#键在秒级时间戳timestamp后过期
expireat key timestamp
>expireat hello 1469980800 #设置1469980800秒过期时间

#键在milliseconds毫秒后过期
pexpire key milliseconds

#键在毫秒级时间戳timestamp后过期
pexpireat key milliseconds-timestamp

无论是使用过期时间还是时间戳，秒级还是毫秒级，在Redis内部最终使用的都是pexpireat

ttl命令和pttl都可以查询键的剩余过期时间，但是pttl精度更高可以达到
毫秒级别，有三个返回结果：
>=0：键剩余的过期时间（ttl是秒，pttl是毫秒）
-1：键没有设置过期时间
-2：键不存在
```

+ 如果`expire key`的键不存在，返回结果为0
+ 如果过期时间为负值，键会立即被删除
+ `persist`命令可以将键的过期时间清除
+ 对于字符串类型键，执行`set`命令会去掉过期时间
+ Redis不支持二级数据结构（例如哈希、列表）内部元素的过期功 能，例如不能对列表类型的一个元素做过期时间设置
+ `setex`命令作为`set+expire`的组合，不但是原子执行，同时减少了一次 网络通讯的时间

**迁移键**

```sql
#同一redis不同数据库之间迁移
move key db
move命令用于在Redis内部进行数据迁移

#不同redis之间迁移
#在源Redis上，dump命令会将键值序列化，格式采用的是RDB格式
dump key
#在目标Redis上，restore命令将上面序列化的值进行复原，其中ttl参数代表过期时间，如果ttl=0代表没有过期时间
restore key ttl value
整个迁移过程并非原子性的，而是通过客户端分步完成的

#不同redis之间迁移   
migrate host port key|"" destination-db timeout [copy] [replace] [key ...]
host：目标Redis服务器的IP地址或主机名
port：目标Redis服务器的端口号
key：需要迁移的键名
""：表示要迁移多个键
destination-db：目标Redis数据库的索引号，从0开始
timeout：迁移操作的超时时间，单位毫秒
copy：可选参数，表示在迁移过程中保留原键
REPLACE：可选参数，表示如果目标Redis数据库中已经存在相同的键，则覆盖该键
key ...：指定要迁移的键的列表

整个过程是原子执行的，不需要在多个Redis实例上开启客户端的，只需要在源Redis上执行migrate命令即可
migrate命令的数据传输直接在源Redis和目标Redis上完成
目标Redis完成命令后会发送OK给源Redis，源Redis接收后会根据migrate对应的选项来决定是否在源Redis上删除对应的键
```

### 遍历键

***全量遍历键***

```sql
keys pattern
#keys命令是支持模式匹配的
*匹配任意字符
?匹配一个字符
[]匹配部分字符，[1,3]匹配1，3，[1-3]匹配1到3的任意数字
\x用来做转义，匹配星号、问号需要进行转义
```

如果Redis包含了大量的键，执行`keys`命令很可能会造成Redis阻塞

***渐进式遍历***

使用渐进式遍历可以有效防止阻塞

因为增量式命令仅仅使用游标来记录迭代状态， 所以这些命令带有以下缺点：

+ 从集合中遍历出的元素可能会重复
+ 遍历期间添加或删除集合数据时，这些数据也可能会被返回

```sql
scan cursor [match pattern] [count number]
cursor：一个游标，告诉Redis从哪里开始迭代，每次scan遍历完都会返回当前游标的值，直到游标值为0，表示遍历结束
match pattern：指定一个模式，只有匹配该模式的键才会被返回
count number：指定每次迭代返回的键的最大数量，但只是提示redis，结果不一定
```

除了`scan`以外，Redis提供了面向哈希类型、集合类型、有序集合的扫描遍历命令，解决诸如`hgetall`、`smembers`、`zrange`可能产生的阻塞问题，对 应的命令分别是`hscan`、`sscan`、`zscan`

```sql
#遍历哈希
hscan key cursor [match pattern] [count number]

#遍历集合
sscan key cursor [match pattern] [count number]

#遍历有序集合
zscan key cursor [match pattern] [count number]
```

### 数据库管理

```sql
#切换数据库
select dbIndex
Redis默认配置中是有16个数据库，0号数据库和15号数据库之间的数据没有任何关联，甚至可以存在相同的键
但多数据库功能已逐渐弱化：
#Redis是单线程的。如果使用多个数据库，那么这些数据库仍然是使用一个CPU，彼此之间还是会受到影响的
#多数据库的使用方式，会让调试和运维不同业务的数据库变的困难，假如有一个慢查询存在，依然会影响其他数据库
#部分Redis的客户端根本就不支持这种方式
如果要使用多个数据库功能，完全可以在一台机器上部署多个Redis实例，彼此用端口来做区分，因为现代计算机或者服务器通常是有多个CPU的。这样既保证了业务之间不会受到影响，又合理地使用了CPU资源

flushdb #只清除当前数据库
flushall #清除所有数据库

#验证密码是否正确
auth password  
#查看服务是否运行
ping
#关闭当前连接
quit
```

# redis附加功能

## 慢查询分析

慢查询日志：系统在命令执行前后计算每条命令的执行时间如果超过预设阀值，就将这条命令的相关信息（例如：发生时间，耗时，命令的详细信息）记录下来

`slowlog-log-slower-than`就是那个预设阀值， 它的单位是微秒（1秒=1000毫秒=1000000微秒），默认值是10000，假如执行了一条很慢的命令（例如keys *），如果它的执行时间超过了10000微秒，那么它将被记录在慢查询日志中

`slowlog-log-slower-than=0`记录所有的命令

`slowlog-log-slowerthan<0`不记录所有的命令

Redis使用了一个列表来存储慢查询日志，`slowlog-max-len`就是列表的最大长度。一个新的命令满足慢查询条件时被插入到这个列表中，当慢查询日志列表已处于其最大长度时，最早插入的一个命令将从列表中移出

Redis中有两种修改配置的方法，一种是修改配置文件，另一种是使用`config set`命令动态修改(在服务器运行期间重写某些配置，无需重启 Redis)

```sql
#所有使用config set设置的配置参数将会立即被Redis加载，并从下一个执行的命令开始生效
#config rewrite命令对启动Redis服务器时所指定的redis.conf配置文件进行改写，使用后才能将配置持久化到本地配置文件

config set slowlog-log-slower-than 20000 #预设阀值
config set slowlog-max-len 1000 #列表最大长度
config rewrite #持久化配置

#获取慢查询日志
slowlog get [n] #n指定查看最近几条记录
每个慢查询日志有4个属性组成：
unique identifier：标识该条慢查询记录的编号
timestamp：以UNIX时间戳形式表示的日志记录时间。
execution time：命令的实际执行时间，单位通常为微秒
command details：执行的具体命令及其参数

#获取慢查询日志列表当前长度
slowlog len

#慢查询日志重置
slowlog reset
```

`slowlog-max-len`配置建议：线上建议调大慢查询列表，记录慢查询时 Redis会对长命令做截断操作，并不会占用大量内存。增大慢查询列表可以 减缓慢查询被剔除的可能，例如线上可设置为1000以上

`slowlog-log-slower-than`配置建议：默认值超过10毫秒判定为慢查询， 需要根据Redis并发量调整该值。由于Redis采用单线程响应命令，对于高流量的场景，如果命令执行时间在1毫秒以上，那么Redis最多可支撑OPS不到1000。因此对于高OPS场景的Redis建议设置为1毫秒

慢查询只记录命令执行时间，并不包括命令排队和网络传输时间。因此客户端执行命令的时间会大于命令实际执行时间。因为命令执行排队机制，慢查询会导致其他命令级联阻塞

当客户端出现请求超时，需要检查该时间点是否有对应的慢查询，从而分析出是否为慢查询导致的命令级联阻塞

由于慢查询日志是一个先进先出的队列，也就是说如果慢查询比较多的情况下，可能会丢失部分慢查询命令，为了防止这种情况发生，可以定期执行`slowlog get`命令将慢查询日志持久化到其他存储中

## redis shell

***redis-cli***

```sql
#命令执行n次
-r n
#./redis-cli -r 3 ping
执行3次ping命令

#每隔n秒执行1次命令，必须和-r选项一起使用
#不支持毫秒为单位，想以每隔10毫秒执行一次，可以用-i 0.01
-i n
#redis-cli -r 3 -i 2 ping
每隔2秒执行1次，共执行3次

#把当前客户端模拟成当前Redis节点的从节点，可以用来获取当前Redis节点的更新操作
--slave
#redis-cli --slave
client1使用--slave，同步已完成
client2执行一些更新操作
client1就会收到redis节点的更新操作

#请求Redis实例生成并发送RDB持久化文件，保存在本地。可使用它做持久化文件的定期备份
--rdb

#将命令封装成Redis通信协议定义的数据格式，批量发送给Redis执行
--pipe

#获取每个数据类型最大的bigkeys，同时给出每个类型键的个数和平均大小
--bigkeys
最好在slave节点执行，--bigkeys也是扫描数据，会造成其他线程阻塞
使用-i参数，降低扫描的执行速度

#执行指定Lua脚本
--eval

#检测客户端到目标Redis的网络延迟
--latency
--latency-history #分时段显示，使用-i选项控制间隔时间
--latency-dist #统计图表的形式显示延迟统计信息

#实时获取Redis的重要统计信息
--stat
```

***pipeline***

Redis客户端执行一条命令分为如下四个过程：

发送命令->命令排队->命令执行->返回结果

发送命令到返回结果的结果称为Round Trip Time（RTT，往返时间），批量操作命令可以有效地节约RTT，但大部分命令不支持批量操作

流水线Pipeline机制能改善这个问题，它将一组Redis命令进行组装，通过一次RTT传输给Redis，再将这组Redis命令的执行结果按顺序返回给客户端

+ 原生批量命令是原子的，Pipeline是非原子的
+ 原生批量命令是一个命令对应多个key，Pipeline支持多个命令
+ 原生批量命令是Redis服务端支持实现的，而Pipeline需要服务端和客户端的共同实现

Pipeline虽然好用，但一次组装Pipeline数据量过大，一方面会增加客户端的等待时间，另一方面会造成一定的网络阻塞，可以将一次包含大量命令的Pipeline拆分成多次较小的Pipeline来完成

***redis-server***

`redis-server`除了启动Redis外，还有一个`--test-memory`选项

`redis-server- -test-memory`可以用来检测当前操作系统能否稳定地分配指定容量的内存给Redis，通过这种检测可以有效避免因为内存问题造成Redis崩溃，整个内存检测的时间较长

***redis-benchmark***

`redis-benchmark`可以为Redis做基准性能测试

```sql
#客户端并发数量，默认50
-c number

#客户端请求数量，默认100000
-n number
#./redis-benchmark -c 10 -n 20
20个client同时请求redis，一共执行20次，redis-benchmark会对各类数据结构的命令进行测试，并给出性能指标

#每个请求pipeline的数据量
-p number

#代表客户端是否使用keepalive，1为使用，0为不使用，默认值为1
-k number

#将结果按照csv格式输出，便于后续处理，如导出到Excel等
--csv
```

## 事务与Lua

事务表示一组动作，要么全部执行，要么全部不执行，将一组需要一起执行的命令放到`multi`和`exec`两个命令之间。`multi`命令代表事务开始，`exec`命令代表事务结束，它们之间的命令是原子顺序执行的

命令的执行顺序是原子性的，即命令的执行不会被其他客户端的命令插入或中断。Redis通过提供单线程模型来保证命令的原子顺序

```sql
>multi #事务开始
>sadd user:a:follow user:b #a关注b
>sadd user:b:fans user:a #b增加粉丝a
>exec #事务结束

discard #停止事务的执行

如果事务中的命令语法错误，会造成整个事务无法执行
redis不支持回滚功能

有些应用场景需要在事务之前确保事务中的key没有被其他客户端修改过，才执行事务，否则不执行
#给key值加1，假设原值为1，如果2个客户端同时增加值，最终结果为2而不是3，redis提供watch命令来解决这类问题
>set num Hello
>watch num
>multi
>append num Lua
QUEUED#排队
>exec
客户端2：
>append num redis
(integer)8
客户端1：
>exec
>get num
"Helloredis"
#如果有多个客户端同时增加值，事务会执行失败
```

**在redis中使用Lua**

在Redis中执行Lua脚本有两种方法：`eval`和`evalsha`

```sql
eval 脚本内容 key个数 key列表 参数列表
```

```sql
>eval 'return "hello " .. KEYS[1] .. ARGV[1]' 1 redis world
"hello redisworld"
```

如果Lua脚本较长，可以使用`redis-cli--eval`直接执行文件

`eval`命令和`--eval`参数本质是一样的，客户端如果想执行Lua脚本，首先在客户端编写好Lua脚本代码，然后把脚本作为字符串发送给服务端，服务端会将执行结果返回给客户端

Redis还提供了`evalsha`命令来执行Lua脚本

执行Lua脚本需要将其加载到Redis服务端，得到该脚本的SHA1校验和， `evalsha`命令使用SHA1作为参数可以直接执行对应Lua脚本，避免每次发送Lua脚本的开销。这样客户端就不需要每次执行脚本内容，而脚本也会常驻在服务端，脚本功能得到了复用

`script load`命令可以将脚本内容加载到Redis服务器的脚本缓存中，并返回该脚本的SHA1校验和，这个SHA1校验和可以在后续的`evalsha`命令中使用，以执行已经加载的脚本，这样无需每次都发送完整的脚本内容

```shell
redis-cli script load <script>
#<script>不能是文件路径，只能是脚本内容，可以用变量来存储
```

```sql
evalsha 脚本SHA1值 key个数 key列表 参数列表
```

**Lua的Redis API**

是Lua使用`redis.call`函数实现对redis的访问

```lua
redis.call("set", "hello", "world")
redis.call("get", "hello")
```

在redis的执行效果：

```sql
eval 'return redis.call("get", KEYS[1])' 1 hello
#获取hello键数组[1]的值
```

Lua还可以使用`redis.pcall`函数实现对Redis的调用，`redis.call`和 `redis.pcall`的不同在于，如果`redis.call`执行失败，那么脚本执行结束会直接返回错误，而`redis.pcall`会忽略错误继续执行脚本

Lua可以使用`redis.log`函数将Lua脚本的日志输出到Redis的日志文件中， 但是一定要控制日志级别

+ Lua脚本在Redis中是原子执行的，执行过程中间不会插入其他命令
+ Lua脚本可以帮助开发和运维人员创造出自己定制的命令，并可以将这些命令常驻在Redis内存中，实现复用的效果
+ Lua脚本可以将多条命令一次性打包，有效地减少网络开销

设置一个列表记录热门用户的id：

```sql
>rpush user:list 1 2 3 4 5
```

设置一个字符串类型的键表示用户的热度

```shell
>mset 2 20 3 30 4 40 5 50
```

将列表内所有的键对应热度做加1操作，并且保证是原子执行， 此功能可以利用Lua脚本来实现：
```lua
--取出列表所有元素，赋值给list
local list = redis.call("lrange", KEYS[1], 0, -1)
--记录incr的总次数
local count = 0
--遍历list中所有元素，最后返回count
for index, key in ipairs(list)
do
    redis.call("incr", key)
    count = count + 1
end
return count
```

执行脚本：

```sql
redis-cli --eval D:\visualstudio2022\Lua\redis.lua user:list
```

执行后所有用户的热度自增1：

```sql
>mget 1 2 3 4 5
1) "11"
2) "21"
3) "31"
4) "41"
5) "51"
```

**Redis如何管理Lua脚本**

```sql
script load
#用于将Lua脚本加载到Redis内存中

scripts exists sha1 [sha1 …]
#用于判断sha1是否已经加载到Redis内存中，返回sha1被加载到redis内存的个数
script flush
#用于清除Redis内存已经加载的所有Lua脚本

script kill
#用于杀掉正在执行的Lua脚本
```

如果Lua脚本比较耗时，甚至Lua脚本存在问题，那么此时Lua脚本的执行会阻塞Redis，直到脚本执行完毕或 者外部进行干预将其结束

redis的配置文件内提供了一个`lua-time-limit`参数，默认是5秒，它是Lua脚本的超时时间，但这个超时时间仅仅是当Lua脚本时间超过`lua-time-limit`后，向其他命令调用发送BUSY的信号，但是并不会停止掉服务端和客户端的脚本执行

所以当达到`lua-time-limit`值之后，其他客户端在执行正常的命令时，将 会收到`Busy Redis is busy running a script`错误，并且提示使用`script kill`或者 `shutdown nosave`命令来杀掉这个busy的脚本

> 但如果当前Lua脚本正在执行写操作，那么`script kill`将不会生效，只能等待脚本执行结束或使用`shutdown save`停掉Redis服务，Lua脚本虽然好用，但是使用不当破坏性也是难以想象的

## Bitmaps

**数据结构模型**

合理地使用位能够有效地提高内存使用率和开发效率。Redis提供了`Bitmaps`这个数据结构可以实现对位的操作

+ `Bitmaps`本身不是一种数据结构，实际上它就是字符串，但是它可以对字符串的位进行操作
+ `Bitmaps`单独提供了一套命令，所以在Redis中使用`Bitmaps`和使用字符串的方法不太相同。可以把`Bitmaps`想象成一个以位为单位的数组，数组的每个单元只能存储0和1，数组的下标在`Bitmaps`中叫做偏移量

**命令**

将访问的用户记做1，没有访问的用户记做0，用偏移量作为用户的id

***设置值***

```sql
setbit key offset value
#uid为0的用户该天访问了网站
setbit unique:user:2022:04:17 0 1
```

设置键的第`offset`个位的值，从0开始

很多应用的用户id以一个指定数字（例如10000）开头，直接将用户id和`Bitmaps`的偏移量对应势必会造成一定的浪费，通常的做法是每次做`setbit`操作时将用户id减去这个指定数字。在第一次初始化`Bitmaps`时，假如偏移量非常大，那么整个初始化过程执行会比较慢，可能会造成Redis的阻塞

***获取值***

```sql
gitbit key offset
#获取id=8的用户是否在2022-04-17这天访问过
>gitbit unique:user:2022:04:17 8
```

**获取`Bitmaps`指定范围值为1的个数**

```sql
bitcount [start][end]
#获取用户id在第1个字节到第3个字节之间的独立访问用户数
>bitcount unique:user:2022:04:17 1 3
```

**Bitmaps间的运算**

`bitop`是一个复合操作，它可以做多个`Bitmaps`的and交集、or并 集、not非、xor异或操作并将结果保存在`destkey`中

```sql
bitop 运算符 destkey key[key....]
#两天内都访问网站的用户数
>bitop and unique:user:and unique:user:2022:04:17 unique:user:2022:04:18

#两天内的任意一天访问过网站的用户数
>bitop or unique:user:or unique:user:2022:04:17 unique:user:2022:04:18
```

`bitpos`用于在二进制字符串中查找第一个设置为指定值的位的位置

```sql
bitpos key targetBit [start] [end]
```

`start`和`end`分别代表起始字节和结束字节

```sql
>bitpos unique:user:2022:04:17 0 0 1
#计算第0个字节到第1个字节之间，第一个值为0的偏移量
```

**Bitmaps分析**

 										set和Bitmaps存储一天活跃用户的对比

![](https://img2024.cnblogs.com/blog/3357226/202402/3357226-20240216121033119-1686948660.png)

但如果网站每天独立访问用户少，还不如集合

![](https://img2024.cnblogs.com/blog/3357226/202402/3357226-20240216121307793-633591850.png)

## HyperLogLog

HyperLogLog并不是一种新的数据结构，实际类型为字符串类型，是一种基数算法，通过HyperLogLog可以利用极小的内存空间完成独立总数的统计

***向HyperLogLog添加元素***

`pfadd`用于向HyperLogLog添加元素，如果添加成功返回1：

```sql
pfadd key element [element …]
```

***计算一个或多个HyperLogLog的独立总数***

```sql
pfcount key [key …]
```

***将多个HyperLogLog的并集赋值给destkey***

```sql
pfmerge destkey sourcekey [sourcekey ...]
#destkey是目标键，用于存储合并后的HyperLogLog
```

HyperLogLog内存占用量小得惊人，但是用如此小空间来估算如此巨大的数据，必然不是100%的正确，其中一定存在误差率。Redis官方给出的数字是0.81%的失误率

因此使用HyperLogLog只为了计算独立总数，不需要获取单条数据

## 发布订阅

redis提供基于发布/订阅模式的消息机制，此种模式下，消息发布者和订阅者不进行直接通信，发布者客户端向指定的频道发布消息，订阅该频道的每个客户端都可以收到该消息

![](https://img2024.cnblogs.com/blog/3357226/202402/3357226-20240216123645271-1627712224.png)

***发布消息***

```sql
publish channel message
```

将`message`发送到指定的`channel`

***订阅消息***

```sql
subscribe channel [channel ...]
```

订阅一个或多个`channel`的`message`

+ 客户端在执行订阅命令之后进入了订阅状态，只能接收`subscribe`、 `psubscribe`、`unsubscribe`、`punsubscribe`的四个命令
+ 新开启的订阅客户端，无法收到该频道之前的消息，因为Redis不会对发布的消息进行持久化

***取消订阅***

```sql
unsubscribe [channel [channel ...]
```

退订客户端指定的`channel`，取消成功后，不会再收到该频道的发布消息

***按照模式订阅和取消订阅***

```sql
psubscribe pattern [pattern...]
punsubscribe pattern [pattern ...]
#订阅以it开头的所有频道
psubscribe it*
```

***查询订阅***

```sql
#查看活跃的频道，活跃频道指当前频道至少有一个订阅者
pubsub channels [pattern]

#查看频道订阅数
pubsub numsub [channel ...]

#查看模式订阅数
pubsub numpat
```

**使用场景**

聊天室、公告牌、服务之间利用消息解耦都可以使用发布订阅模式

![](https://img2024.cnblogs.com/blog/3357226/202402/3357226-20240216143335608-1994228283.png)

```sql
#订阅vedeo:change频道
subscribe vedeo:change
#视频管理系统发布消息到vedeo:change频道
publish video:change "video1,video2,video3"
#视频服务收到消息，对视频消息进行更新
for video in video1,video3,video5
	update {video}
```

## GEO

Redis3.2版本提供了GEO（地理信息定位）功能，支持存储地理位置信息用来实现诸如附近位置

**增加地理位置信息**

```sql
geoadd key longitude latitude member [longitude latitude member ...]
```

`longitude`、`latitude`、`member`分别是该地理位置的经度、纬度、成员，返回结果代表添加成功的个数

如果需要更新地理位置信息，仍然可以使用`geoadd`命令，虽然返回结果为0，geoadd`命令可以同时添加多个地理位置信息

**获取地理位置信息**

```sql
geopos key member [member ...]
```

**获取两个地理位置的距离**

```sql
geodist key member1 member2 [unit]
```

`unit`代表返回结果的单位

+ m：meters
+ km：kilometers
+ mi：miles
+ ft：feet

**获取指定位置范围内的地理信息位置集合**

```sql
georadius key longitude latitude radiusm|km|ft|mi [withcoord] [withdist] [withhash] [COUNT count] [asc|desc] [store key] [storedist key]
georadiusbymember key member radiusm|km|ft|mi [withcoord] [withdist] [withhash] [COUNT count] [asc|desc] [store key] [storedist key]
```

`georadius`和`georadiusbymember`两个命令的作用是一样的，都是以一个地 理位置为中心算出指定半径内的其他地理信息位置

但`georadius`命令需要给出具体的经纬度，而`georadiusbymember`只需给出成员即可

+ `withcoord`：返回结果中包含经纬度
+ `withdist`：返回结果中包含离中心节点位置的距离
+ `withhash`：返回结果中包含`geoha~sh`
+ `COUNT count`：指定返回结果的数量
+ `asc|desc`：返回结果按照离中心节点的距离做升序或者降序
+ `store key`：将返回结果的地理位置信息保存到指定键
+ `storedist key`：将返回结果离中心节点的距离保存到指定键

**获取geohash**

```sql
geohash key member [member ...]
```

Redis使用`geohash`将二维经纬度转换为一维字符串

+ GEO的数据类型为`zset`，Redis将所有地理位置信息的`geohash`存放在`zset` 中
+ 字符串越长，表示的位置更精确
+ 两个字符串越相似，它们之间的距离越近，Redis利用字符串前缀匹配算法实现相关的命令
+ `geohash`编码和经纬度是可以相互转换的

**删除地理位置信息**

```sql
zrem key member
```

GEO没有提供删除成员的命令，但是因为GEO的底层实现是`zset`，所以可以借用`zrem`命令实现对地理位置信息的删除

# 客户端

Redis制定了RESP（REdis Serialization Protocol，Redis序列化协议）实现客户端与服务端的正常交互，这种协议简单高效，既能够被机器解析，又容易被人类识别

**发送命令格式**

```sql
*<参数数量> CRLE
$<参数1字节数量> CRLE
<参数1> CRLE
...
```

实际传输格式没有格式化

**返回结果格式**

+ 状态回复：在RESP中第一个字节为+
+ 错误回复：在RESP中第一个字节为-
+ 整数回复：在RESP中第一个字节为:
+ 字符串回复：在RESP中第一个字节为$
+ 多条字符串回复：在RESP中第一个字节为*

![](https://img2024.cnblogs.com/blog/3357226/202402/3357226-20240216152030225-1136936014.png)

可以在Linux系统中使用`nc`或`telnet`命令查看redis服务端返回的真正结果

```sql
nc 127.0.0.1 6379
set Hello world
+OK
get Hello
$5
world
```

无论是字符串回复还是多条字符串回复，如果有`nil`值，那么会返回`$-1`

有了RESP提供的发送命令和返回结果的协议格式，各种编程语言就可以利用其来实现相应的Redis客户端

## Java客户端Jedis

Jedis属于Java的第三方开发包，在Java中获取第三方开发包通常有两种方式：

+ 直接下载目标版本的Jedis-${version}.jar包加入到项目中
+ 使用集成构建工具，例如maven、gradle等将Jedis目标版本的配置加入到项目中

Jedis的使用方法非常简单，只要下面三行代码就可以实现`get`功能：

```java
//生成一个Jedis对象，这个对象负责和指定Redis实例进行通信
Jedis jedis = new Jedis("127.0.0.1", 6379);
//jedis执行set操作
jedis.set("hello", "world");
//jedis执行get操作, value="world"
String value = jedis.get("hello");
```

初始化Jedis需要两个参数：Redis实例的IP和端口，此外还有一个包含四个参数的构造函数：

```java
Jedis(final String host, final int port, final int 				  connectionTimeout, final int soTimeout)
```

+ host：Redis实例的所在机器的IP
+ port：Redis实例的端口
+ connectionTimeout：客户端连接超时
+ soTimeout：客户端读写超时

在实际项目中比较推荐使用`try catch finally`的形式来进行代码的书写： 

+ 可以在Jedis出现异常的时候（本身是网络操作），将异常进行捕获或者抛出

+ 无论执行成功或者失败，将Jedis连接关闭掉，在开发中关闭不用的连接资源是一种好的习惯



