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
|`RPUSH key value[value ...]`|将一个或多个value插入到key的表尾。<br/>如果key不存在，则创建以key命名的列表，并执行RPUSH操作|
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

### Hash(哈希类型)

### Zset(有序集合类型)
