### zookeeper数据结构和监听机制



#### zookeeper数据结构

zookeeper采用类似文件系统的层级树状结构进行管理，在zookeeper中，数据信息被保持在一个个数据节点上，被称为Znode

+ Znode是最小的数据单位
+ Znode下可以再创建Znode



#### Zone类型

Zookeeper节点类型可以分为三类

+ 持久化节点
+ 临时性节点
+ 顺序性节点

在实际使用过程中，可以通过组合生成四种节点类型：

***持久节点***

***持久顺序节点***

***临时节点***

***临时顺序节点***



#### Znode的状态信息

***事务ID***：在zookeeper中，事务是指能改变**Zookeeper服务器状态**的操作，一般包括数据节点的创建、删除，数据节点内容的变更操作。对于每一个事务请求，zookeeper都会分配一个全局的事务ID，用**ZXID**表示。每一个ZXID对应一次更新操作，通过ZXID可以识别出Zookeeper处理操作的全局顺序。



获取节点状态信息：get /.nodename

```shell
cZxid 表示节点被创建时的事务ID
ctime 表示节点创建时间
mZxid 表示节点最后一次被修改时的事务ID
mtime 表示节点最后一次被修改的时间
pZxid 表示该节点的子节点列表最后一次被修改的事务ID，只有子节点列表变更才会更新pZid，子节点内容变更不会更新
cversion 表示子节点的版本号
dataVersion 表示内容版本号
aclVersion 表示acl版本
ephemeralOwner 表示创建该临时节点的会话sessionId，如果是持久性节点则为0
dataLength 表示数据长度
numChildren 表示直系子节点数
```



#### 监听机制

Zookeeper使用`watcher`机制实现分布式发布/订阅功能。

`Watcher`机制主要包括：*客户端*、*客户端WatcherManager*、*Zookeeper服务器*

