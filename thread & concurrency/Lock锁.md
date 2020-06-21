## Lock锁



### JUC之Lock锁

​		在java并发API（java.util.concurrent）中有一系列显示锁，这些锁都由`Lock`接口规定，可用于替代`sychronized`这种隐示锁的高级功能。

​		`Lock`提供了一种无条件的、可轮询的、定时的及可中断的锁获取操作。

Lock锁主要实现有：

+ ReentrantLock 

+ ReentrantReadWriteLock 


#### ReentrantLock

​		与`synchronized`一样，`ReentrantLock`保证了互斥性和内存可见性，同时也提供了可重入的加锁语义。

`Lock`的标准使用：

```java
Lock lock = new ReentrantLock();
lock.lock()
try{
  //TODO
}finally{
  lock.unlock();
}
```

​		`Lock`锁必须显示的加锁和释放锁，尤其需要注意的是，必须要在finally中释放锁，因为在程序的执行控制离开被保护的代码块时，不会自动清除锁。

+ 轮询锁、定时锁 

  在`Lock`锁中，`tryLock`提供了一种可轮询、可定时获取锁机制：

  1、如果锁没有被其他线程占有，那么该线程立即获得锁；

  2、如果锁被其他线程占有，若指定了等待时间，那么在等待时间内会一直尝试获取锁，在此期间如果锁被其他线程释放，那么立即获得锁。

  ```java
  boolean tryLock()
  boolean tryLock(long timeout, TimeUnit unit)
  ```

+ 可中断锁

  Lock提供了一个种可中断机制，即线程在等待锁资源时候，会对`interrupt`进行响应，停止等待锁资源释放。

  ```java
  private void doAcquireInterruptibly(int arg)
    throws InterruptedException {
    final Node node = addWaiter(Node.EXCLUSIVE);
    boolean failed = true;
    try {
      for (;;) {
        final Node p = node.predecessor();
        if (p == head && tryAcquire(arg)) {
          setHead(node);
          p.next = null; // help GC
          failed = false;
          return;
        }
        if (shouldParkAfterFailedAcquire(p, node) &&
            parkAndCheckInterrupt())
          throw new InterruptedException();
      }
    } finally {
      if (failed)
        cancelAcquire(node);
    }
  }
  
  private final boolean parkAndCheckInterrupt() {
    LockSupport.park(this);
    return Thread.interrupted();
  }
  ```

  

+ 公平锁与非公平锁

  `ReentrantLock`提供了两个构造方法，默认构造方法提供非公平锁，带参构造可指定锁的公平性。

  ```java
  public ReentrantLock() {
     sync = new NonfairSync();
  }  
  public ReentrantLock(boolean fair) {
    sync = fair ? new FairSync() : new NonfairSync();
  }
  ```



#### ReentrantReadWriteLock

​		`ReentrantLock`实现了一种标准的互斥锁，每次最多只能有一个线程持有锁资源，但对于读多写少的操作来说，这种”强硬“的加锁规则也限制了并发性。如果资源在无写入更改的情况下，允许多个线程同时读取，那么在一定程度上可以提高并发性。`ReentrantReadWriteLock`提供了读写访问的一对锁，只要写锁没有被占用，允许并发地访问被保护的资源。

下图实现了一个简单的`Map`,并用ReadWriteLock来包装，能在多个读线程中被安全的共享。

```java
public class ReadWriteMap<K,V>{
    private final Map<K,V> map;
    private final ReadWriteLock lock = new ReentrantReadWriteLock();
    private final Lock readLock = lock.readLock();
    private final Lock writeLock = lock.writeLock();
        
    public ReadWriteMap(Map<K,V> map){
        this.map = map;
    }
    
    public V put(K key,V value){
        writeLock.lock();
        try{
            return map.put(key,value);
        }finally {
            writeLock.unlock();
        }
    }
    
    public V get(Object key){
        readLock.lock();
        try{
            return map.get(key);
        }finally {
            readLock.unlock();
        }
    }
}
```



#### StampedLock

​		`JDK1.8`中提供了一种新的锁：`StampedLock`，它同样支持读写锁。

​		1）与`ReentrantReadWriteLock`不同的是，`StampedLock`锁没有重入特性，`StampedLock`的`readLock()`和`writeLock()`方法会返回一个long类型标记，这个标记可用来释放锁或检查锁是否有效。

```java
 long writeLock();
 long readLock();
 boolean validate(long stamp);
 void unlockWrite(long stamp);
 void unlockRead(long stamp);
 void unlock(long stamp);
```

​		2）与此同时，`StampedLock`还支持一种`乐观锁`特性。

```java
public long tryOptimisticRead() {
  long s;
  return (((s = state) & WBIT) == 0L) ? (s & SBITS) : 0L;
}
```

​		`tryOptimisticRead`并不会发生“阻塞”，如果当前线程获取到乐观读锁后，有其他线程取得写锁，那么其他线程将无需等待乐观读锁释放锁而直接拿到写锁。此时，乐观读锁处于无效状态，所以如果使用乐观读锁，那么需要注意使用`validate(stamp)`判断锁是否有效。

​		3）`StampedLock`还提供了一种锁转化机制，可以将读锁转化为写锁，避免再次加锁、解锁操作，进一步提高性能。

```java
long stamp = lock.readLock();
try {
  if (count == 0) {
    stamp = lock.tryConvertToWriteLock(stamp);
    if (stamp == 0L) {
      System.out.println("Could not convert to write lock");
      stamp = lock.writeLock();
    }
    //TODO
  }
} finally {
  lock.unlock(stamp);
}
```







