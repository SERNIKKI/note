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

### Set(集合类型)

### Hash(哈希类型)

### Zset(有序集合类型)
