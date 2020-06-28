### JVM调优之常用命令

在对一个系统问题定位的时候，我们通常需要对一些`数据`进行分析，这里的`数据`包括：CPU使用情况、内存占用情况、运行日志、异常堆栈、GC日志、线程快照（threaddump/javacore文件）、堆转储快照（heapdump/hprof文件）等。

#### linux下提供的命令

1. `top`  命令查看各个进程的资源使用情况
2. `top -Hp`  查看指定进程的线程资源使用情况

#### JDK提供的常用命令

| 命令   | 说明                                                         |
| ------ | ------------------------------------------------------------ |
| jps    | JVM Process Status Tool，显示指定系统内所有的HotSpot虚拟机进程 |
| jstat  | JVM Statistics Monitoring Too，用于收集HotSpot虚拟机各方面的运行数据 |
| jinfo  | Configuration Info for Java，显示虚拟机配置信息              |
| jmap   | Memory Map for Java，生成虚拟机的内存转储快照（heapdump文件） |
| jhat   | JVM Heap Dump Browser，用于分析heapdump文件                  |
| jstack | Stack Trace for Java，显示虚拟机的线程快照                   |



1. jps

   Jps -l

   

   

2. jstat

   jstat -gc 4655 500 20 -- 每隔500ms打印进程4655的gc情况，一共打印20次

   

3. jinfo

   jinfo pid

   

4. jmap

   jmap - histo 4655 | head -20，查找有多少对象产生

   jmap -dump:format=b,file=xxx pid ：

   另外也可以在启动程序时候设置jvm参数，当发生OOM时候，自动产出堆转储文件。

   ***eg: java -Xms20M -Xmx20M -XX:+UseParallelGC -XX:+HeapDumpOnOutOfMemoryError demo.jar***

   

5. jhat

   

6. Jstack



#### 其他工具

1. jconsole

2. jvisualVM

3. arthas

   

#### dump文件解读

