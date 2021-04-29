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

## Java操作Redis

### Jedis
>使用Jedis可以使用Java来操作redis数据库

#### 使用Jedis远程连接

1.  修改服务器提供商的安全组，开启6379端口
2.  修改服务器防火墙规则，开放6379端口
```shell
    systemctl start firewalld
    systemctl status firewalld
    firewalld-cmd --zone=public --add-port=6379/tcp --permanent
```
3.  修改redis配置文件
* protected修改为no:`protected-mode no`
* 注释掉bind
* 设置密码:`requirepass xxxx`
4.  在pom文件中导入依赖
```xml
<!-- https://mvnrepository.com/artifact/redis.clients/jedis -->
        <dependency>
            <groupId>redis.clients</groupId>
            <artifactId>jedis</artifactId>
            <version>3.3.0</version>
        </dependency>
```
5.  在Java中连接
```java
    public void test(){
        //1、创建jedis对象，连接redis数据库
        Jedis jedis = new Jedis("115.29.200.235",6379,false);
        jedis.auth("sernikki000602");//设置密码
        //2、使用redis中的命令操作数据库
        System.out.println(jedis.ping());//测试ping命令是否连接成功
        /*
         * 在此操作redis数据库
         */
        //3、关闭连接
        jedis.close();
    }
```

### SpringBoot整合Redis
>在SpringBoot2.x版本之后，底层不再使用`Jedis`而改用`lettuce`。

#### Jedis和Lettuce的区别

* `Jedis`底层采用的是**直连**，在多个线程连接的情况下，是**不安全**的。想要避免直连，则需要使用`Jedis pool`连接池。**更像[BIO](https://www.cnblogs.com/zedosu/p/6666984.html)模式**
* `Lettuce`使用`netty`，是异步请求，实例可以在多个线程中共享，**不存在线程不安全的情况**。可以减少线程数据。**更像[NIO](https://www.cnblogs.com/zedosu/p/6666984.html)模式**

#### 在SpringBoot中连接

```java
    //获取connection连接
    RedisConnection connection = redisTemplate.getConnectionFactory().getConnection();
    //清空数据库
    connection.flushDb();
    //操作数据库
    redisTemplate.opsForValue().set("v1",s);
    System.out.println(redisTemplate.opsForValue().get("v1"));
    //关闭连接
    connection.close();
```

#### 序列化
>在Springboot中，默认的序列化是`JdkSerializationRedisSerializer`，**故会产生乱码的情况**。
```java
    //源码中关于序列化的设置
    static RedisSerializer<Object> java(@Nullable ClassLoader classLoader) {
		return new JdkSerializationRedisSerializer(classLoader);
	}
```
**向数据库中传入对象**
* 使用`new ObjectMapper().writeValueAsString()`方法将对象转为json字符串
```java
    Pet pet = new Pet("番茄", "2");
    //将pet对象转为json格式的字符串
    String jsonPet = new ObjectMapper().writeValueAsString(pet);
    redisTemplate.opsForValue().set("pet",jsonPet);
```
* 直接传入对象，将会报**对象没有序列化**的错误。需要对象实现**序列化**才能传值成功。
```java
    public class Pet implements Serializable {
        private String name;
        private String age;
    }
```

**自定义redisTemplate**
```java
    @Configuration
    public class RedisConfig {
        @Bean
        @SuppressWarnings("all")
        public RedisTemplate<String,Object> redisTemplate(RedisConnectionFactory redisConnectionFactory){
            RedisTemplate<String, Object> template = new RedisTemplate<>();
            template.setConnectionFactory(redisConnectionFactory);
            //序列化配置
            Jackson2JsonRedisSerializer serializer = new Jackson2JsonRedisSerializer(Object.class);
            //使用ObjectMapper进行转义
            ObjectMapper mapper = new ObjectMapper();
            mapper.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
            mapper.activateDefaultTyping(LaissezFaireSubTypeValidator.instance, ObjectMapper.DefaultTyping.NON_FINAL);
            serializer.setObjectMapper(mapper);
            StringRedisSerializer stringRedisSerializer = new StringRedisSerializer();
            //key采用string的序列化方式
            template.setKeySerializer(stringRedisSerializer);
            //hash采用string的序列化方式
            template.setHashKeySerializer(stringRedisSerializer);
            //value采用jackson
            template.setValueSerializer(serializer);
            template.setHashValueSerializer(serializer);
            template.afterPropertiesSet();
            return template;
        }
    }
```

#### Redis工具类

参考[RedisUtils](/redis/redisUtils.md·)

## Redis.conf
>redis的配置文件
* redis配置文件对大小写**不敏感**
* redis可以包含其他conf
```bash
# include /path/to/local.conf
# include /path/to/other.conf
```
* 网络
```bash
bind 127.0.0.1 -::1 # 绑定ip地址
protected-mode no # 是否为保护模式，默认为yes
port 6379 # 端口设置，默认为6379
```
* 通用GENERAL
```bash
daemonize yes # 是否以守护进程开启(可以在后台运行)
pidfile /var/run/redis_6379.pid # 如果以守护进程开启，就需要指定一个pid文件
loglevel notice # 日志，级别分别有:debug、verbose、notice、warning
logfile "" # 默认的日志文件名，如果没有写则为控制台输出
databases 16 # 默认的数据库数量
always-show-logo no # 是否显示logo。默认为no
set-proc-title yes
proc-title-template "{title} {listen-addr} {server-mode}"
```
* 快照SNAPSHOTTING
>持久化:在规定的时间内，执行了多少次操作，则会持久化到文件。.rbd,.aof
>redis是基于内存的数据库，如果没有持久化，数据断电即失
```bash
# 在3600s内至少修改了1次key，则进行持久化操作
save 3600 1 
# 在300s内至少修改了100次key，则执行持久化操作
save 300 100
# 在60s内至少修改了10000次key，则执行持久化操作
save 60 10000

stop-writes-on-bgsave-error yes # 如果持久化出错，是否还需要继续工作。默认为yes
rdbcompression yes # 是否压缩rdb文件。会需要消耗一些cpu资源
rdbchecksum yes # 是否校验rdb文件，如果出错会进行修复。
dbfilename dump.rdb # rdb文件的名字
rdb-del-sync-files no
dir ./ # rdb文件存放的目录，默认为当前目录下
```
* 主从复制REPLICATION
* 关键点追踪KEYS TRACKING
* 安全SECURITY
```bash
acllog-max-len 128
requirepass xxxxx # 密码设置
rename-command CONFIG b840fc02d524045429941cc15f59e41cb7be6c52 # 把危险的命令修改为其他名称，这样用户不能使用，而内部工具可以使用
rename-command CONFIG "" # 设置为一个空的值，可以禁止一个命令
```
* 进程限制CLIENTS
* 内存设置MEMORY MANAGEMENT

## Redis持久化
>redis是内存数据库，如果不将内存中的数据库状态保存到磁盘，那么一旦服务器进程退出，服务器中的数据库状态就会丢失。所以redis提供了持久化的功能

### RDB机制
>RDB其实就是把数据以**快照**的形式保存在磁盘上。(快照:把当前时刻的数据拍成一张图片保存下来)
>RDB持久化是指在指定的时间间隔将内存中的**数据集快照写入磁盘**。也是**默认**的持久化方式，这种方式就是将内存中数据以快照方式写入到二进制文件中，默认的文件名为`dump.rdb`。
>对于RDB来说，提供了三种机制：sava、bgsave、自动化

#### sava触发方式

该命令会**阻塞**当前的redis服务器，执行`save`命令期间，redis不能处理其他命令，直到**RDB过程完成**为止。具体流程为:

执行完成时如果存在旧的RDB文件，就把新的替换掉旧的。*客户端可能为几万或者几十万，不能使用这种方式*

#### bgsave触发方式

执行该命令时，Redis会在后台**异步**进行快照操作，*快照的同时还可以响应客户端请求*。具体流程为:
具体操作是Redis进程执行fork操作创建子进程，RDB持久化过程**由子进程负责**，完成后自动结束。阻塞只发生在**fork阶段**，一般时间很短。*基本上Redis内部所有的RDB操作都是采用`bgsave`命令*。

#### 自动触发

自动触发是由配置文件完成的。在redis.conf配置文件中：
| 配置 | 默认值 | 含义 |
| :---: | :---: | :--- |
|`save m n`||表示m秒内如果至少有n个key的值变化，自动触发`bgsave`|
|`stop-writes-on-bgsave-error`|yes|当启用了RDB之后最后一次后台保存数据失败，Redis是否停止接受数据。<br/>这会让用户意识到数据没有正确的持久化到磁盘上。|
|`rdbcompression`|yes|对于存储到磁盘中的快照，设置是否进行压缩存储。|
|`rdbchecksum`|yes|存储快照后，使redis使用**CRC64算法**来进行数据校验，但这样做会**增加大约10%的性能消耗**。|
|`dbfilename`||设置快照的文件名，默认为`dump.rdb`|
|`dir`||设置快照文件的存储路劲|

#### save与bgsave的对比
| 命令 | save | bgsave |
| :--- | :--- | :--- |
|IO类型|同步|异步|
|是否阻塞|是|是(fork的时候)|
|复杂度|O(n)|O(n)|
|优点|不会消耗额外内存|不阻塞客户端命令|
|缺点|阻塞客户端命令|需要fork,消耗内存|

#### RDB的优势和劣势

##### 优势

1.  RDB文件紧凑，全量备份，非常适合用于进行备份和灾难恢复。
2.  生成RDB文件的时候，redis主进程会fork()一个子进程来处理所有保存工作，主进程不需要进行任何磁盘IO操作
3.  RDB在恢复大数据集时的速度比AOF的恢复速度快。

##### 劣势

RDB快照是一次全量备份，存储的是内存数据的二进制序列化形式，存储上非常紧凑。当进行快照持久化的时候，会开启一个子进程专门负责快照的持久化，子进程会拥有父进程的全部内存数据，父进程修改内存子进程不会反应出来，所以**在快照持久化期间修改的数据不会保存**，可能会**丢失数据**。

### AOF机制
>redis会将每一条**写命令**都通过write函数追加到文件中。相当于**写日志**。

#### 持久化原理
>每当有一个写命令时，就直接保存在AOF文件中。

#### 文件重写原理
AOF的方式由于写命令的不断写入，会导致持久化文件会越来越大。为了压缩持久化文件。redis提供了`bgrewriteaof`命令。将内存中的数据以命令的方式保存到临时文件中，同时fork出一条新进程来将文件重写。
>重写AOF文件的操作，并没有读取旧的AOF文件，而是将整个内存中的数据库内容用命令的方式重写了一个新的AOF文件，和快照类似。

#### 三种触发机制

* 每修改同步`always`：同步持久化，每次发生数据改变会立即记录到磁盘，性能比较差但是数据完整性好。
* 每秒同步`everysec`：异步操作，每秒记录，如果一秒宕机，有数据丢失。
* 不同`no`：从不同同步

| 命令 | always | everysec | no |
| :--- | :--- | :--- | :--- |
|优点|不丢失数据|每秒一次fsync|不用管|
|缺点|IO开销大|丢1秒数据|不可控|

#### 优缺点

##### 优点

* AOF可以更好的保存数据不丢失，一般AOF会每隔一秒，通过一个后台线程执行一次`fsync`操作，最多丢失1秒数据。
* AOF日志文件没有任何磁盘寻址的开销，写入性能非常高，文件不容易损坏。
* AOF日志文件即使过大的时候，出现后台的重写操作，也不会影响客户端的读写。
* AOF日志文件的命令通过非常可读的方式进行记录，很适合做**灾难性的误删除的紧急恢复**。

##### 缺点

* 对于同一份数据，AOF日志文件通常比RDB文件数据快照文件更大
* AOF开启后，支持的写QPS会比RDB支持的写QPS低，因为AOF一般会配置成每秒fsync一次日志文件。

### RDB和AOF

| 命令 | RDB | AOF |
| :--- | :--- | :--- |
|启动优先级|低|高|
|体积|小|大|
|恢复速度|快|慢|
|数据安全性|丢数据|根据策略不同|
|轻重|重|轻|
>一般两者结合一起使用

## Redis订阅
>用于消息发布，例如公众号订阅等

| 命令 | 含义 |
| :--- | :--- |
|`psubscribe pattern [pattern ...]`|订阅一个或多个符合给定模式的频道。|
|`pubsub subcommand [argument [argument...]]`|查看订阅与发布系统状态|
|`publish channel message`|发送信息到指定频道|
|`punsubscribe pattern [pattern ...]`|退订所有给定模式下的频道|
|`subscribe channel [channel ...]`|订阅给定的一个或多个频道|
|`unsubscribe channel [channel ...]`|退订给定的频道|

## 主从复制

### 集群
>所谓集群，就是通过添加服务器的数量，提供相同的服务，从而让服务器达到一个稳定、高效的状态。

* redis集群中，每一个redis称之为一个节点。
* redis集群中，有两种类型的节点：主节点(master)、从节点(slave)。
* redis集群，是基于redis**主从复制**实现。

### 主从复制

**主从复制中，有多个redis节点。其中，有且仅有一个为主节点master，从节点slave可以有多个**。
>只要网络正常，master会一直将自己的数据更新同步给slave，保持主从同步。

#### 特点

* 主节点master**可读、可写**。
* 从节点slave**只读**。(reda-only)

主从模型可以提高读写能力，在一定程度上缓解了写的能力。因为能写仍然只有master一个节点，可以将读的操作全部移交到从节点上，变相提高了写能力。

## 哨兵模式
>redis的Sentinel系统用于管理多个redis服务器(instance)，该系统执行以下三个任务:
* **监控(Monitoring)**:Sentinel会不断的检查主服务器和从服务器是否运作正常。
* **提醒(Notification)**:当被监控的某个Redis服务器出现问题时，Sentinel可以通过API向管理员或者其他应用程序发送通知。
* **自动故障迁移(Automatic failover)**:当一个主服务器不能正常工作时，Sentinel会开始一次**自动故障迁移**操作，它会进行**选举**，将其中一个从服务器升级为新的主服务器，并让失效主服务器的其他从服务器改为复制新的主服务器；当客户端试图连接失效的主服务器时，集群也会向客户端返回新主服务器地址，使得集群可以使用新服务器替代失效的服务器。

### 监控
* Sentinel可以监控**任意多个**Master和该Master的Slaves(即**多个主从模式**)
* 同一个哨兵下的、不同主从模型，彼此之间**相互独立**。
* Sentinel会**不断地检查**Master和Slaves是否正常

### 自动故障切换
>监控同一个Master的Sentinel会**自动连接**，组成一个**分布式**的Sentinel网络，**互相通信并交换彼此关于被监视服务器的信息**。
这样可以防止其中一个Sentinel挂掉，从而无法实现自动故障切换。

#### 故障切换过程

* 投票(半数原则):当任何一个Sentinel发现被检控的Master下线时，会通知其他的Sentinel开会，投票确定该Master是否下线(半数以上，**所以Sentinel通常配奇数个**)
* 选举:当Sentinel确定Master下线后，会在所有Slaves中，选举一个新的节点，升级为Master节点。其他Slaves节点，转为该节点的从节点
* 原Master重新上线:当原Master重新上线后，自动转化为当前Master节点的从节点。

## 缓存穿透与雪崩
>在生产环境中，会因为很多原因造成访问请求绕过了缓存，都需要访问数据库持久层，虽然对redis缓存服务器不会造成影响，但是数据库的负载就会增大，使缓存的作用降低。

### 缓存穿透
>缓存穿透是指查询一个**根本不存在**的数据，缓存层和持久层都不会命中。在日常工作中出于容错的考虑，如果从持久层查不到数据则不写入缓存层，缓存穿透将导致不存在的数据每次请求都要到持久层查询，失去了缓存保护后端持久的意义。

缓存穿透问题可能会使后端存储**负载加大**，由于很多后端持久层不具备高并发性，甚至有可能造成后端存储宕机。通常可以在程序中**统计总调用数**、**缓存层命中数**、如果同一个key的缓存命中率很低，可能就是出现了缓存穿透问题。

造成缓存穿透的基本原因有两个：
* 自身业务代码或者数据出现问题(例如:set和get的key值不一致)
* 一些恶意攻击、爬虫等造成大量空命中(爬取线上商城商品数据，超大循环递增商品的ID)

**解决方案**
1.  缓存空对象
>缓存空对象是指在持久层没有命中的情况下，设置key为null
缓存空对象有两个问题:
* value为null不代表不占用内存空间，空值做了缓存，意味着缓存层中存了更多的键，需要更多的内存空间。比较有效的方法是针对这类数据**设置一个较短的过期时间**，让其自动剔除。
* 缓存层和持久层的数据会有一段时间窗口的不一致，可能对业务有一定影响。例如过期时间设置为5分钟，如果此时存储层添加了这个数据，那此段时间就会出现缓存层和存储层数据不一致，此时可以利用**消息系统**或其他方式清除掉缓存层中的空对象。
2.  布隆过滤器拦截
在访问缓存层和存储层之前，将存在的key用布隆过滤器提前保存起来，做第一层拦截，当收到一个对key请求时先用布隆过滤器验证key是否存在，如果存在再进入缓存层、存储层。可以使用**bitmap**做布隆过滤器。这种方法适用于**数据命中不高、数据相对固定、实时性低**的应用场景，代码维护较为复杂，但是缓存空间占用少。
布隆过滤器实际上是一个很长的二进制向量和一系列随即映射函数。布隆过滤器可以用于检索一个元素是否在一个集合中。它的优点是**空间效率**和**查询时间**都远远**超过**一般的算法，缺点是**有一定的误识别率和删除困难**。
**算法描述:**
* 初始状态时，BoolmFilter是一个长度为m的位数组，每一位都置为0。
* 添加元素x时，x使用k个hash函数得到k个hash值，对m取余，对应的bit位设置为1。
* 判断y是否属于这个集合，对y使用k个哈希函数得到k个哈希值，对m取余，所有对应的位置都是1，则认为y属于该集合(由于**哈希冲突**，可能存在误判)，否则就认为y不属于该集合。可以通过增加哈希函数和增加二进制数组的长度来降低错报率。
**错报原因**
一个key映射数组上多位，一位会被多个key使用，也就是多对多的关系。如果一个key映射的所有值为1，就判断为存在。但是可能会出现key1和key2同时映射到下标为100的位，key1不存在，key2存在，这种情况下会发生错报率。
**方案对比**

| 方法 | 适用场景 | 维护成本 |
| :--- | :--- | :--- |
|缓存空对象|* 数据命中不高<br/>* 数据频繁变化实时性高|* 代码维护简单<br/>* 需要过多的缓存空间<br/>* 数据不一致|
|布隆过滤器|* 数据命中不高<br/>* 数据相对固定实时性低|* 代码维护复杂<br/>* 缓存空间占用少|

### 缓存雪崩
>由于缓存层承载着大量请求，有效的保护了存储层，但是如果缓存层由于某些原因不可用(宕机)或者大量缓存由于超时时间相同在同一时间段失效(大批key失效/热点数据失效)，大量请求直接到达存储层，存储层压力过大导致系统雪崩。

**解决方案**
* 可以把缓存层设计成**高可用**的，即使个别节点、个别机器、甚至机房宕机掉，依然可以提供服务。利用sentinel(哨兵)或cluster实现。
* 采用多级缓存，本地进程作为一级缓存，redis作为二级缓存，不同级别的缓存设置的超时时间不同，即使某级缓存过期了，也有其他级别缓存兜底
* 缓存的过期时间用随机值，尽量让不同的key的过期时间不同(例如:定时任务新建大批量key，设置的过期时间相同)

### 缓存击穿
系统在存在以下两个问题时，易发生缓存击穿：
* 当前key是一个热点key(例如一个秒杀活动)，并发量非常大。
* 重建缓存不能在短时间内完成，可能是一个复杂计算，例如复杂的sql，多次IO、多个依赖等。
在缓存失效的瞬间，有大量线程来重建缓存，造成后端负载加大，甚至可能会让应用崩溃。
**解决方案**
1.  分布式互斥锁
只允许一个线程重建缓存，其他线程等待重建缓存的线程执行完，重新从缓存获取数据即可。
2.  永不过期
* 从缓存层来看，确实没有设置过期时间，所以不会出现热点key过期后产生的问题，也就是"物理"不过期。
* 从功能层面来看，为每个value设置一个逻辑过期时间，当发现超过逻辑过期时间后，会使用单独的线程去更新缓存。
**两种方案对比**
* 分布式互斥锁：思路比较简单，但是存在一定的隐患，如果在查询数据库和重建缓存(key失效后进行了大量的计算)时间过长，也可能会存在死锁和线程池阻塞的风险，高并发情况下吞吐量大大降低，但是这种方法能够较好的降低后端存储负载，并在一致性上做的比较好。
* 永不过期：由于没有设置真正的过期时间，实际上已经不存在热点key产生的一系列危害，但是会存在数据不一致的情况，同时代码复杂度会增大。





