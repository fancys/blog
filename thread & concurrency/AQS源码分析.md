## AQS源码分析

​		在使用`ReentrantLock`锁的时候，我们在`ReentrantLock`可以发现三个静态类：`Sync`、`NonfairSync`和`FairSync`，而静态抽象类继承了`AbstractQueuedSynchronizer`(AQS)。

<img src="../img/thread/ReentrantLock.jpg" style="zoom:50%;" />

