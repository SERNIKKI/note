## 基本命令
>redis是一个key-value的键值对内存数据库。对于命令中的key大小写敏感

| 命令 | 含义 |
| :--- | :--- |
| `DEL` | 删除key，`DEL key1 key2` |
| `EXISTS` | 检查key是否存在，`EXISTS key` |
| `EXPIRE` | 设置更新到期时间，到期后自动清除，单位为**秒**，设置为-1表示**永不过期**。`EXPIRE key`|
| `PERSIST`| 移除过期时间，key永久保存。*其实就是设置过期时间为-1*|
| `PTTL` | 以**毫秒**为单位返回key剩余的过期时间|
| `TTL` | 以**秒**为单位返回key剩余的生存时间|
| `EXPIREAT` | 设置key的过期时间，不过设置的是**时间戳**|
|`PEXPIRE`|以**毫秒**为单位设置key的过期时间|
|`KEYS pattern`|查找匹配给定模式pattern的所有的key。<br/>`KEYS *`:匹配数据库中所有的key。<br/>`KEYS h?llo`:匹配hello，hallo，hxllo等。<br/>`KEYS h*llo`:匹配hllo，heeeeello等。<br/>`KEYS h[ae]llo`:只匹配hello和hallo。<br/>特殊符号用`\`隔开|
|`MOVE key db`|把指定的key移动到数据库db中。**(默认有16个数据库)**|
|`SELECT index`|切换数据库，**默认有16个数据库，编号从0开始**|
|`RANDOMKEY`|随机获取一个key|
|`RENAME KEY newkey`|修改key的名字。**如果newkey已经存在，则删除newkey**|
|`RENAMENX key newkey`|仅当newkey不存在时，将key改名为newkey|
|`TYPE key`|返回key的类型|


## 五大数据类型

### String(字符串类型)
>可以设置字符串和数字

| 命令 | 含义 |
| :--- | :--- |
|`GET`|获取一个值。`GET key`|
|`MGET`|获取多个值。`GET key1 key2 ...`|
|`SET`|新增、修改一个值。`SET key value`|
|`MSET`|设置多个值。`MSET key1 value1 key2 value2 ...`|
|`GETSET`|设置一个值，然后返回之前的值 `GETSET key value`|
|`GETRANGE`|根据范围获取值。<br/>比如一个value是 `hello`<br/>正数下标从0开始递增，负数下标在结尾处从-1开始向前递减<br/>`GETRANGE key 0 3` --> hell<br/>`GETRANGE key -5 -3` --> hel<br/>`GETRANGE key 0 -1` --> hello|
|`SETEX key seconds value`|设置一个key，并规定它的过期时间，单位为**秒**|
|`PSETEX key milliseconds value`|设置一个key，并规定它的过期时间，单位为**毫秒**|
|`SETNX`|只有key不存在的时候设置value（新增一个）|
|`MSETNX`|同时设置一个或多个key-value对，当且仅当所有的key都不存在才会成功。具有**原子性**|
|`APPEND key value`|追加一个value值到key末尾|
|`STRLEN key`|获取key的value值长度|
|`INCR key`|将key中的数字值增加1|
|`INCRBY key increment`|将key所储存的值增加给定的数量|
|`INCRBYFLOAT key increment`|增加一定的浮点数|
|`DECR key`|将key存储的数字减一|
|`DECRBY key decrement`|减去给定的数值|

### List(列表类型)
>可以从头部或者尾部插入(取出)。可以做队列，栈

| 命令 | 含义 |
| :--- | :--- |
|`LPUSH key value[value ...]`|将一个或多个value值插入到key的表头，**插入的时候是在表头插入，相当于入栈**<br/>如果key不存在，则会创建一个空列表并执行`LPUSH`操作<br/>如果key存在但不是列表的时候，会返回一个错误|
|`RPUSH key value[value ...]`|将一个或多个value插入到key的表尾。<br/>如果key不存在，则创建以key命名的列表，并执行`RPUSH`操作|
|`LPUSHX key value`|将值value插入到列表key的表头，**当且仅当key存在并且是一个列表**|
|`RPUSHX key value`|将值value插入到列表key的表尾，**当且仅当key存在并且是一个列表**|
|`LSET key index value`|通过指定**下标**设置值，该下表必须**存在**。相当于**更新**操作|
|`LPOP key`|移除并返回列表key的头元素|
|`RPOP key`|移除并返回列表key的尾元素|
|`BLPOP key[key ...] timeout`|移除并获取列表的第一个元素，如果列表没有元素会**阻塞列表**直到**等待超时**或**发现可弹出的元素**为止|
|`BRPOP key[key ...] timeout`|移除并获取列表的最后一个元素，如果列表没有元素会**阻塞列表**直到**等待超时**或**发现可弹出元素**为止|
|`RPOPLPUSH source destnation`|移除列表的最后一个元素，并将该元素添加到另一个列表并返回|
|`BRPOPLPUSH source destnation timeout`|移除列表的最后一个元素，并将该元素添加到另一个列表并返回;<br/>如果列表没有元素会**阻塞列表**直到**等待超时**或**发现可弹出的元素**为止|
|`LINDEX key index`|返回列表key中下表为index的元素|
|`LINSERT key BEFFORE\|AFTER pivot value`|将值value插入到列表key的pivot元素之前或之后;<br/>当pivot不存在于列表key时，不执行任何操作。存在多个pivot时只操作**第一个**;<br/>当key不存在时，key被视为空列表，不执行任何操作;<br/>如果key不是列表类型，返回一个错误|
|`LLEN key`|返回列表中元素数量，即**长度**|
|`LREM key count value`|移除前count个value的值|
|`LRANGE key start stop`|查看指定范围的value，可以用于**分页**|
|`LTRIM key start stop`|对一个列表进行**修剪(trim)**，让列表只保留指定区间内的元素，不在指定区间之内的元素都将被删除|

### Set(集合类型)
>Set是String类型的无序集合。集合成员是**唯一**的，不能重复。

| 命令 | 含义 |
| :--- | :--- |
|`SADD key member[member ...]`|将一个或多个member元素加入到集合key中，已经存在于集合的元素会被忽略;<br/>加入key不存在，则创建一个只包含member元素成员的集合|
|`SCARD key`|返回集合key的基数(集合中元素的数量)|
|`SISMEMBER key member`|判断member元素是否为集合key的成员。返回1表示存在，0表示不存在|
|`SMEMBERS key`|返回集合key中的所有成员。不存在的key视为空集合|
|`SMOVE source destination member`|将member元素从source集合移动到destination集合。**`SMOVEE`是原子性操作**<br/>如果source集合不存在或不包含指定的member元素，则`SMOVE`命令不执行任何操作，仅返回0。否则，member元素从source集合中被移除，并添加到destination集合中。<br/>当destination集合包含member元素时，`SMOVE`命令只是简单地将source集合中的member元素**删除**。<br/>当source或destination不是集合类型的时候，返回一个错误。|
|`SPOP key`|将集合中一个随机的元素移除并返回|
|`SRANDMEMBER key[count]`|如果执行时只有key参数，那么返回集合中一个随机的元素<br/>从Redis2.6版本开始。`SRANDMEMBER`命令接受可选的count参数<br/>count为正数且小于集合基数:命令返回一个包含count个元素的数组，数组中的元素各不相同<br/>count为正数且大于等于集合基数:返回整个集合<br/>count为负数:命令返回一个数组，数组中的元素可能会重复出现多次，数组长度为count的绝对值<br/>该操作和`SPOP`命令相似，但是`SRANDMEMBER`**仅仅返回随机的元素而不对集合进行任何改动**|
|`SREM key member[member ...]`|移除集合key中一个或多个member元素，不存在的member元素会被忽略<br/>当key不是集合类型的时候，返回一个错误|
|`SDIFF key[key ...]`|返回一个集合的全部成员，该集合是所有给定集合之间的**差集**|
|`SDIFFSTORE destination key[key ...]`|这个命令的作用和`SDIFF`类似，但它将结果保存在destination集合，而不是简单的返回结果集。<br/>如果destination集合已经存在，则将其**覆盖**。<br/>destination可以是**key本身**，若destination不存在，则新建一个|
|`SINTER key[key ...]`|返回一个集合的全部成员，该集合是所有给定集合的**交集**。<br/>不存在的key被视为**空集**。<br/>当给定的集合中有一个空集的时候，结果也为**空集**|
|`SINTERSTORE destination key[key ...]`|这个命令的作用和`SINTER`类似，但它将结果保存在destination集合，而不是简单的返回结果集。<br/>如果destination集合已经存在，则将其**覆盖**。<br/>destination可以是**key本身**，若destination不存在，则新建一个|
|`SUNION key[key ...]`|返回一个集合的全部成员，该集合是所有给定集合的**并集**。<br/>不存在的key被视为空集|
|`SUNIONSTORE destination key[key ...]`|这个命令的作用和`SUNION`类似，但它将结果保存在destination集合，而不是简单的返回结果集。<br/>如果destination集合已经存在，则将其**覆盖**。<br/>destination可以是**key本身**，若destination不存在，则新建一个|

>作用
* 共同关注
* 共同爱好
* 二度好友
* 推荐好友([六度分隔理论](https://baike.baidu.com/item/%E5%85%AD%E5%BA%A6%E5%88%86%E9%9A%94%E7%90%86%E8%AE%BA/1086996?fr=aladdin))
* ......
### Hash(哈希类型)
>hash可以存储对象，字典等。hash里面是一个string类型的key-value映射集合，每一个都叫做**域**

| 命令 | 含义 |
| :--- | :--- |
|`HSET key field value`|将哈希表key中的域field的值设为value。<br/>如果key不存在，则创建一个新的hash表并进行`HSET`操作。<br/>如果域field已经存在于hash表中，则旧值将会被覆盖|
|`HMSET key field value [field value ...]`|同时将多个field-value(域-值)对设置到哈希表key中。<br/>**此命令会覆盖旧的field域**|
|`HGET key field`|返回哈希表key中给定域field的值|
|`HMGET key field[field ...]`|返回哈希表中，一个或多个给定域的值。<br/>如果给定的域不存在于哈希表，则返回一个nil值。<br/>不存在的key被当作空哈希表处理，返回为nil|
|`HGETALL key`|返回哈希表key中所有的域和值。**返回的长度为哈希表大小的2倍**|
|`HEXISTS key field`|查看哈希表中，给定域field是否存在。<br/>若存在则返回1，否则返回0|
|`HDEL key field[field ...]`|删除一个或多个域|
|`HKEYS key`|返回哈希表key中所有的域|
|`HLEN key`|返回哈希表key中域的数量|
|`HINCRBY key field increment`|为哈希表key中的域field的值加上增量increment。<br/>增量可以为**负数**。<br/>如果key不存在，则创建一个新的哈希表key并执行`HINCRBY`命令。<br/>如果域field不存在，那么在执行命令前，域的值被初始化为0|
|`HINCRBYFLOAT key field increment`|为哈希表 key 中的域 field 加上浮点数增量 increment 。|

### Zset(有序集合类型)
>Redis有序集合和集合一样也是string类型元素的集合，且不允许重复的成员。<br/>不同的是每个元素都会关联一个double类型的分数(score)。redis通过分数来为集合中的成员进行从大到小的排序。<br/>有序集合的成员是唯一的，但分数(score)可以重复。

| 命令 | 含义 |
| :--- | :--- |
|`ZADD key score member [[score member] [score member] ...]`|将一个或多个member元素及其score值加入到有序集key中。<br/>如果某个member已经是有序集的成员，则更新该member;并重新插入该元素，来保证member在**正确的位置**。<br/>score值可以是整数值或双精度浮点数。<br/>如果key不存在，则创建一个空的有序集并执行`ZADD`操作。<br/>当key存在但不是有序集的时候，返回一个错误。|
|`ZCARD key`|返回有序集key的个数|
|`ZSCORE key member`|返回有序集key中，成员member的score的值。<br/>如果member不在有序集中或key不存在，返回nil|
|`ZCOUNT key min max`|返回有序集key中，score值在min和max之间(默认包括等于min或max的值)的成员数量。|
|`ZINCRBY key increment member`|为有序集key的成员member的score值加上增量increment。<br/>当key不存在，或member不是key的成员时，该命令等同于`ZADD key increment member`<br/>当key不是有序集类型时，返回错误。|
|`ZRANGE key start stop[WITHSCORES]`|**可以用来分页**，返回有序集key中，指定区间内的成员。<br/>成员的位置按score值递增来排序<br/>**具有相同的score值的成员按字典序(lexicographical order)来排列。|
|`ZREVRANGE key start stop[WITHSCORE]`|**可以用来分页**，返回有序集key中，指定区间内的成员。<br/>其中成员的位置按score值递减来排列。<br/>具有相同score值的成员按照字典序的逆序(reverse lexicographical order)排列。|
|`ZRANGEBYSCORE key min max [WITHSCORES] [LIMIT offset count]`|返回有序集key中，所有score值介于min和max之间(包括等于min或max)的成员。<br/>有序集成员按score值递增排序。<br/>具有相同score值的成员按照字典序(lexicographical order)来排列。<br/>* 可选的`LIMIT`参数指定返回结果的数量及区间(*就像SQL中的`SELECT LIMIT offset,count`*),**当offset很大时，定位offset的操作可能需要遍历整个有序集，此过程最坏复杂度为`O(N)`时间。**<br/>* 可选的`WITHSCORES`参数决定时只返回有序集的成员还是将有序集成员和score值一起返回。<br/>* **该选项自Redis2.0版本可以用**|
|`ZREVRANGEBYSCORE key max min [WITHSCORES] [LIMIT offset count]`|返回有序集key中，score值位于max和min之间(默认包括max或min)的所有成员。<br/>有序集成员按照score值递减排列。<br/>具有相同score值的成员按字典序的逆序(reverse lexicographical order )排列。|
|`ZRANK key member`|返回有序集key中成员member的排名。<br/>**有序集成员按score值递增顺序排列**。<br/>排名以0为底，也就是说，score值最小的成员排名为0。|
|`ZREVRANK key member`|返回有序集key中成员member的排名。<br/>**有序集成员按score值递减顺序排列**。<br/>排名以0为底，也就是说，score值最大的成员排名为0。|
|`ZLEXCOUNT key min max`|计算有序集合中指定字典区间内成员数量。|
|`ZREM key member [member ...]`|移除有序集key中的一个或多个成员，不存在的成员将被忽略。<br/>当key存在但不是一个有序集的时候，返回一个错误。|
|`ZREMRANGEBYRANK key start stop`|移除有序集key中，指定排名(rank)区间内的所有成员。<br/>区间分别以下标参数start和stop指出，包含start和stop在内。<br/>下标参数start和stop都以0为底，也就是说，以0表示有序集第一个成员，1表示有序集第二个成员，以此类推。<br/>也可以用负数下标，以-1表示最后一个成员，-2表示倒数第二个成员，以此类推。|
|`ZREMRANGEBYSCORE key min max`|移除有序集合中，所有score值介于min和max之间(包括等于min或max)的成员。<br/>在版本2.1.6开始，score值等于min或max也可以不包括在内。|
|`ZINTERSTORE destination numkeys key [key ...] [WEIGHTS weight [weight ...]] [AGGREGATE SUM\|MIN\|MAX]`|计算给定的一个或多个有序集的**交集**，其中key的数量必须以`numkeys`参数指定，并将该交集的结果集存储到destination。<br/>默认情况下，结果集中某个成员的score值是所有给定集下该成员score值之和。|
|`ZUNIONSTORE destination numkeys key [key ...] [WEIGHTS weight [weight ...]] [AGGREGATE SUM\|MIN\|MAX]`|计算给定的一个或多个有序集的**并集**，其中给定key的数量必须以numkeys参数指定，并将该并集的结果存储到destination。<br/>默认情况下，结果集中某个成员的score值是所有给定集下该成员score值之和。<br/>* `WEIGHTS`<br/>使用`WEIGHTS`选项，你可以为每个给定有序集分别指定一个乘法因子(multiplication factor),每个给定有序集的所有score值在传递给聚合函数(aggregation function)之前都要先乘以该有序集的因子。<br/>如果没有指定`WEIGHTS`选项，乘法因子默认设置为**1**。<br/>* `AGGREGATE`<br/>使用`AGGREGATE`选项，你可以指定并集的结果集的聚合方式。<br/>默认使用参数`SUM`，可以将指定并集中某个成员的score值之和作为结果集中该成员的score值;<br/>使用参数`MIN`,可以将所有集合中某个成员的最小score值作为结果集中该成员的score值;<br/>使用参数`MAX`,可以将所有集合中某个成员的最大score值作为结果集中该成员的score值。|

>小知识：+inf表示正无穷；-inf表示负无穷

#### 非默认情况下的min，max区间如何设置

|命令|效果|
|:---|:---|
|`ZRANGEBYSCORE key min max`|`min<=score<=max`|
|`ZRANGEBYSCORE key (min max`|`min<score<=max`|
|`ZRANGEBYSCORE key min (max`|`min<=score<max`|
|`ZRANGEBYSCORE key (min (max`|`min<score<max`|

## 特殊数据类型

### Geospatial(地理位置)
>底层使用的是Zset命令。

| 命令 | 含义 |时间复杂度|
| :--- | :--- | :---: |
|`GEOADD key longitude latitude member [longitude latitude member ...]`|将指定的地理空间位置(纬度、经度、名称)添加到指定的key中。<br/>有效的经度从 **-180**度到**180**度。<br/>有效的维度从 **-85.05112878**度到**85.05112878**度。|O(log(N))|
|`GEODIST key member1 member2[uniit]`|返回两个给定位置之间的距离。<br/>如果两个位置之间的其中一个不存在，则命令返回空值。<br/>指定单位参数unit为:<br/>* m 表示单位为米。<br/> * km 表示单位为千米。<br/> * mi 表示单位为英里。<br/> * ft表示单位为英尺。<br/>**默认使用米作为单位**|O(log(N))|
|`GEOHASH key member [member ...]`|返回一个或多个位置元素的Geohash表示。<br/>该命令返回11个字符的Geohash字符串。|O(log(N))|
|`GEOPOS key member [member ...]`|从key中返回所有给定位置的元素(经度和维度)|O(log(N))|
|`GEORADIUS key longitude latitude radius m\|km\|ft\|mi [WITHCOORD][WITHDIST][WITHHASH][COUNT count]`|以给定的**经纬度**为中心，返回键包含的位置元素中，与中心距离不超过给定最大距离的所有位置元素。<br/>范围可以使用以下其中一个单位:<br/>* m 表示单位为米。<br/>* km表示单位为千米。<br/>* mi 表示单位为英里。<br/>* ft 表示单位为英尺。<br/>在给定以下选项时，命令会返回额外的信息:<br/>* `WITHDISI`:在返回位置元素的同时，将位置元素与中心之间的距离一并返回。单位与命令指定单位相同。<br/>* `WITHCOORD`:将位置元素的经度和纬度也一并返回。<br/>* `WITHHASH`:以52位有符号整数的形式，返回位置元素经过原始geohash编码的有序集合分值。<br/>默认返回未排序的位置元素，通过以下参数指定排序方式:<br/>* `ASC`:根据中心距离，按照从近到远的方式返回位置元素。<br/>* `DESC`:根据中心位置，按照从远到近的方式返回位置元素。<br/>使用`COUNT`选项获取前N个匹配元素。|O(N+log(M))|
|`GEORADIUSBYMEMBER key longitude latitude radius m\|km\|ft\|mi [WITHCOORD][WITHDIST][WITHHASH][COUNT count]`|以给定的**位置元素**为中心，返回键包含的位置元素中，与中心距离不超过给定最大距离的所有位置元素。<br/>参考`GEORADIUS`命令。|O(N+log(M))|


### Hyperloglog(基数统计)
>`hyperloglog`是用来做基数统计的算法，优点是在输入元素的数量或者体积非常大的时候，计算基数所需要的空间总是**固定的**，并且**非常小**。每个`hyperloglog`键只需要花费12kb内存，就可以计算接近2^64个不同元素的基数。

>**`hyperloglog`只会根据输入元素来计算基数，而不会存储输入元素本身，所以不能返回输入的各个元素**。

| 命令 | 含义 |
| :--- | :--- |
|`PFADD key element [element ...]`|添加指定元素到Hyperloglog中|
|`PFCOUNT key [key ...]`|返回给定Hyperloglog的基数**估算值**。|
|`PFMERGE destkey sourcekey [sourcekey ...]`|将多个Hyperloglog合并为一个Hyperloglog|


### Bitmap(位图)
>`BitMap`即位图，是byte数组，用二进制表示，只有0和1两个数字。

| 命令 | 含义 |
| :--- | :--- |
|`getbit key offset`|对key所存储的字符串值，获取指定的偏移量上的位|
|`setbit key offset value`|对key所存储的字符串值，设置或清除指定偏移量上的位。<br/>返回值为该位在setbit之前的值。<br/>value只能取0或者1.<br/>offset从0开始，即使原位图只能10位，offset可以取1000。|
|`bitcount key [start end]`|获取位图指定范围内位值为1的个数。**如果不指定start与end，则取所有**|
|`bitop op destkey key [key ...]`|做多个BitMap的and(交集)、or(并集)、not(非集)、xor(异或)操作并将结果保存在destkey中。|
|`bitpos key tartgetBit [start end]`|计算位图指定范围内第一个偏移量对应的值等于tartgetBit的位置。<br/>找不到则返回-1.<br/>start与end没有设置，则取全部。<br/>tartgetBit只能取0或者1。|

#### 应用场景
>统计每日用户登录数、打开天数等。如果独立用户很多，使用BitMap明显更有优势，能节省大量内存。但如果独立用户较少，则还是继续使用set存储，BitMap会产生多余的存储开销。

* type = string，BitMap是string类型，最大512MB。
* setbit时的偏移量，可能有较大耗时。
* 位图不是绝对好。

## 事务
>redis事务可以一次执行多个命令，并且带有三个重要的特性：
* 批量操作在发送`exec`命令前被放入队列缓存。
* 收到`exec`命令之后进入事务执行，**事务中任意命令执行失败，其余的命令依然被执行**。
* 在事务执行过程，其他客户端提交的命令请求不会被插入到事务执行命令序列中。

>一个事务从开始到执行会经历以下三个阶段:
* 开始事务
* 命令入队
* 执行事务

>单个redis命令的执行**是原子性的**。但是redis没有在事务上增加任何维持原子性的机制，所以redis事务的执行**并不是原子性的**，中间某条指令的失败不会导致前面已经做的指令回滚，也不会导致后面的指令不执行。

### 主要API
| 命令 | 含义 |
| :--- | :--- |
|`discard`|取消事务，放弃执行事务块内的所有命令。|
|`exec`|执行所有事务块内的命令。<br/>如果某条命令的指令错误，则会返回一个错误，事务块内的所有命令**都不会执行**。<br/>如果是**运行时异常**，则会继续执行。|
|`multi`|标记一个事务块的开始。|
|`unwatch`|取消`watch`命令对所有key的监视。|
|`watch key [key ...]`|监视一个或多个key，如果在事务执行之前这个(或这些)key被其他命令所改动，那么事务将会被打断。|

### 性质
* 一次性
* 顺序性
* 排他性

### 监控
>悲观锁
* 很悲观，认为什么时候都会出问题，无论做什么都会加锁。
>乐观锁
* 很乐观，认为什么时候都不会出问题，所以不会上锁。更新数据的时候会去判断一下，在此期间是否有人修改过这个数据。