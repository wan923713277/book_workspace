## - 什么是redis
redis是一种C语言编写的可远程的内存型数据库，不仅性能强劲，而且还具有复制,持久化的特性以及为解决问题而生的独一无二的数据模型。通常用作应用级的缓存工具来使用。

redis具有五种数据类型

-  String
-  List
-  Set
-  Hash
-  Zset

### String常用命令
String类型的value值可以存String，Int和Float类型

String的整型和浮点型的基本操作：

命令 | 举例用法 | 解释 
---|---|---
INCR | INCR key-name | 给键为key-name存储的值加上1
DECR | DDCR key-name | 给键为key-name存储的值减少1
INCRBY | INCRBY key-name 5 | 给键为key-name存储的值加上5
DECRBY | DDCRBY key-name 5 | 给键为key-name存储的值减少5
INCRBYFLOAT | INCRBYFLOAT key-name 2.1| 给键为key-name存储的值加上2.1

String的字符类型操作：

命令 | 举例用法 | 解释 
---|---|---
APPEND | APPEND key-name abc | 给键为key-name存储的值末尾添加abc字符串
GETRANGE | GETRANGE key-name 1 10| 获取key-name存储的值的由偏移量1到10的子串
SETRANGE | SETRANGE key-name 10 abc | 给key-name的value值从第十位开始由字符串'abc'替换，长度不够会自动扩容
GETBIT | GETRANGE key-name 1 10| 把key-name的value值当成二进制，获取偏移量从1到10的子二进制字符串
SETBIT | SETBIT key-name 10 010101 | 把key-name的value值当成二进制，并将从第十位开始替换为010101，长度不够会自动扩容
BITCOUNT | BITCOUNT key-name [start end] |  把key-name的value值当成二进制，统计值为1的数量（可以添加偏移量限制统计子串）
BITTOP | BITTOP operation dest-key key-name [key-name,...] | 将数个字符串进行操作（operation可选值：AND,OR,XOR,NOT(与，或，异或，非)）

## 列表常用命令


命令 | 举例用法 | 解释 
---|---|---
RPUSH | RPUSH key-name value [value ...] | 将一个或多个值推入列表的右端
LPUSH | LPUSH key-name value [value ...] | 将一个或多个值推入列表的左端
RPOP | RPOP key-name | 移除并返回列表最右端的元素
LPOP | LPOP key-name | 移除并返回列表最左端的元素
LINDEX | LINDEX key-name offset | 返回列表从左开始偏移量为offset的元素(单个元素)
LRANGE | LRANGE key-name start end | 返回列表从左开始偏移量为start到end的所有元素
LTRIM | LTRIM key-name start end | 对列表裁剪，start 到 end （包括start和end）
LLEN | LTRIM key-name | 获取列表的长度

阻塞式和列表之间移动命令：
命令 | 举例用法 | 解释 
---|---|---
BLPOP | BLPOP key-name [key-name ...] timeout | 从一个非空列表中弹出位于最右端的元素，直到timeout之内一直阻塞
LPOP | LPOP key-name [key-name ...] timeout | 从一个非空列表中弹出位于最左端的元素，直到timeout之内一直阻塞
RPOPLPUSH | RPOPLPUSH source-key dest-key | 从source-key右端推出，推出后从左侧dest-key列表
BRPOPLPUSH | BRPOPLPUSH source-key dest-key timeout | 上述的阻塞模式

**阻塞模式的说明：以客户端A输入了blpop命令为例，该会阻塞等待timeout秒，直到客户端B为key-name键中push了值进该列表，则A客户端获取B客户端刚插入的值，并结束，如果timeout秒之内，key-name一直没有别的客户端插入新值，则A客户端返回null**

## 集合常用命令
**Set 无序的集合，一个键中可以存入多个value值，并且是去重的，无序状态**

**List为有序集合，同样是一个键中可以存入多个value值，但是是不去重,且有序的状态（插入的顺序，）**

命令：
命令 | 举例用法 | 解释 
---|---|---
SADD | SADD key-name item [item ...] | 将一个或多个元素存入集合，并返回添加成功的个数
SREM | SREM key-name item [item ...] | 从一个集合里面移除一个或多个元素，并返回被移除元素的数量
SISMEMBER | SISMEMBER key-name item | 检查该元素是否存在于该key-name集合中
SCARD | SCARD key-name | 返回该集合元素的数量
SMEMBERS | SMEMBERS key-name | 返回该集合包含的所有元素
SRANDMEMBER | SRANDMEMBER key-name count | 随机返回一个或多个元素，count为正返回不重复，为负重复
SPOP | SPOP key-name [count] | 随机的返回一个或多个元素
SMOVE | SMOVE source-key dest-key item | 从source-key移除元素item，并添加到dest-key中，移除成功返回1

redis支持集合的运算

命令 | 举例用法 | 解释 
---|---|---
SDIFF | SDIFF key-name [key-name ...] | 将数个集合和第一个集合做差集并返回
SDIFFSTORE | SDIFFSTORE dest-key key-name [key-name ...] | 从一个集合里面移除一个或多个元素并存储到dest-key集合中
SINTER | SINTER key-name [key-name ...] | 将数个集合和第一个集合做交集并返回
SINTERSTORE | SINTERSTORE dest-key key-name [key-name ...] | 将数个集合和第一个集合做并集并存储到dest-key集合中
SUNION | SUNION key-name [key-name ...] | 将数个集合和第一个集合做并集并返回
SUNIONSTORE | SUNIONSTORE dest-key key-name [key-name ...] | 将数个集合和第一个集合做并集并存储到dest-key集合中

## 散列
散列及将多个键值对存储在一个redis键里，适合存放一些相关数据在一起

命令 | 举例用法 | 解释 
---|---|---
HGET | HGET key-name key | 获取key-name的key键的值
HSET | HSET key-name key value | 设置key-name的key键的值为value
HMGET | HMGET key-name key [key ...] | 获取key-name散列中一个或多个值
HMSET | HMSET key-name key value [key value ...] | 设置key-names散列中一个或多个键值对
HDEL | HDEL key-name key | 删除散列里一个或者多个键值对
HLEN | HLEN key-name | 返回散列包含的键值对数量

散列的其他高级特性
命令 | 举例用法 | 解释 
---|---|---
HEXISTS | HEXISTS key-name key | 检查key是否在散列key-name里
HKEYS | HKEYS key-name| 返回key-name中所有的键
HVALS | HVALS key-name | 返回key-name中所有的值
HGETALL | HGETALL key-name| 获取散列所有的键值对
HINCRBY | HINCRBY key-name key increment | 将键key的存储的值加上一个整数increment
HINCRBYFLOAT | HINCRBYFLOAT key-name key increment| 将键key的存储的值加上一个浮点数increment

## 有序集合
和散列结构有相近的地方，不过有序集合值存的是score（分值）

命令 | 举例用法 | 解释 
---|---|---
ZADD | ZADD key-name score member [score member ...] | 往key-name的有序集合中插入带有分值的成员
ZREM | ZREM key-name member [member ...] | 移除key-name的成员，返回移除成功的数量
ZCARD | ZCARD key-name | 返回有序集合包含的成员变量
ZINCRBY | ZINCRBY key-name increment member | 将member成员的分值加上一个increment
ZCOUNT | ZCOUNT key-name min max | 返回分值介于min和max之间的成员变量
ZRANK | ZRANK key-name member |  返回成员member在有序集合中的排名
ZSCORE | ZSCORE key-name member | 返回成员变量member的分值
ZRANGE | ZRANGE key-name start stop [WITHSCORE]| 返回有序集合中排名介于start和stop之间的成员（有withscore则会把分值一起带出）

有序集合的范围型数据获取命令和范围型数据删除命令，以及并集和交集命令

命令 | 举例用法 | 解释 
---|---|---
ZREVRANK | ZREVRANK key-name member | 返回有序集合里成员member的降序排名
ZREVRANGE | ZREVRANGE key-name start stop [WITHSCORES] | 返回start-stop排名的所有成员（降序）
ZRANGEBYSCORE | ZRANGEBYSCORE key min max [WITHSCORES] [limit offset count] | 返回分值在min-max 之间的所有成员
ZREVRANGEBYSCORE |ZREVRANGEBYSCORE key min max [WITHSCORES] [limit offset count]| 同上，但是是倒序
ZREMRANGEBYRANK | ZREMRANGEBYSCORE key-name start stop | 移除有序集合中排名介于start-end之间的所有成员，返回移除成功个数
ZREMRANGEBYSCORE | ZREMRANGEBYSCORE key-name min max | 移除有序集合中排名介于start和stop之间的所有成员变量，返回移除成功个数
ZINTERSTORE | ZINTERSTORE destination numkeys key [key …] [WEIGHTS weight [weight …]] [AGGREGATE SUM/MIN/MAX] |交集， 两个集合都存在的元素做sum/min/max运算并存入destination键中，如果有权重weight，则先乘以权重在做运算
ZUNIONSTORE | ZINTERSTORE destination numkeys key [key …] [WEIGHTS weight [weight …]] [AGGREGATE SUM/MIN/MAX] |同上， 但是并集运算

## 发布与订阅

pub/sub 
命令 | 举例用法 | 解释 
---|---|---
subscribe | subscribe channel [channel ...] | 订阅给定的一个或者多个频道
unsubscribe | unsubscribec channel [channel ...] | 退订...，如果没有给定频道，则退订所有频道
publish | publish channel message | 向给定的频道发布一条消息
psubscribe | psubscribe pattern [pattern ...] | 订阅与给定模式相匹配的所有频道
punsubscribe | punsubscribe [pattern [pattern ...]] | 退订给定的模式，如果执行时没有给定任何模式，那么退订所有模式


## 排序
**sort**  
sort source-key [by pattern] [limit offset count] [get poattern [get pattern ...]] [asc|desc] [alpha] [store dest-key]

