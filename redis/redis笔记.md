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


### List(列表类型)

### Set(集合类型)

### Hash(哈希类型)

### Zset(有序集合类型)
