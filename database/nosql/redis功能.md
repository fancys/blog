## redis功能



### redis管道



### redis发布/订阅

解决一种实时性的消息查询



### redis事务

+ multi：开启事务

+ exec：执行事务
+ watch：为事务提供Check-And-Set行为，被watch监听的键在`exec`前被更改后，exec会返回nil-reply表示事务失败



### bloom(布隆)过滤器

解决缓存穿透问题，用小容量空间与大量信息进行匹配。



### 配置

+ 最大内存

+ 有效期

  不能随着访问而自动延长有效期

  发生写操作会剔除过期时间，需要业务代码层面补全

  倒计时，定时操作不能延长有效期

+ 回收策略



