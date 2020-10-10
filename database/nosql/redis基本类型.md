## redis基本类型

redis存储的是二进制安全流信息，并非存储的字符流。但在redis使用过程中，对数据封装了五种常用的数据类型。

+ String
    
    String类型中可以存储字符类型、数值类型、bitmaps。

    基本操作指令：
    
    + set 新增/更新key值
    + get 获取key的值
    
    参数说明：
    
    + NX：新建的时候没有该key才成功，否则返回nil
    + XX：更新的时候存在该key才成功，否则返回nil
    
    其他指令：
    
    + `help @String` 可以查看String类型的相关操作指令
    
    + 对于数值类型，可以执行`INCR`指令
    + `STRELEN`获取字符长度，但注意编码格式
    + `APPEND`,`LAPPEND`,`RAAPEND`可以追加字符串
    + `getset`获取key的值，同时新建/更新key的值
    + `MSETNX`原子操作，保证多个key同时成功
    + `setbit` 对string类型数据进行二进制位操作。
    
+ hash
    
    可以存储类似map结构的数据，一般可用于存储点赞、收藏、详情页信息。

    + hmset
    + hget
    + hmget
    + Hgetall
    
+ list
    
    可以存储类比编程语言中的栈、队列、数组、阻塞/单播队列（FIFO）类型，可以快速获取头部数据和尾部数据。

    基本命令：
    
    + lpush、rpush
    + lpop、rpop
    
    其他常用名称：
    
    + lrange取出list中指定范围的数据
    + lset设置list中指定位置的数据
    + lrem移除list中指定个数的指定元素
    + blpop等待指定时间内从list取出信息
    
    
    
+ set
    
    可以用于存储一组无序、无重复的集合信息，并提供了一些集合操作和随机取数的功能。

    + sadd 添加set信息
    + srem 移除set中元素
    + sinter 取两个set的交集
    + sinterstore 取两个set的交集，并存储到新的set 
    + sunion 取两个set的并集
    + sunionstore 取出两个set的交集，并存储到新的set
    + sdiff 取出两个set的差集
    + srandmember 随机取出set中指定个数的信息
    
+ sorted set
    
    可以存储有序、去重的集合，与set类型不同，sort set中还包含了分值信息。
    
    + zadd
    + zrange
    + Zrevrange
    + Zrangebyscore
    + Score
    
    sorted set类型通过skip list实现增删改查时排序的速度。
