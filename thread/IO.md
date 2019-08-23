# IO模型



## JAVA集中IO模型

BIO是一种同步阻塞IO。 客户端发起请求，服务端立即处理，new Socket()阻塞了线程

NIO是一种同步非阻塞IO。客户端发起请求，在单线程中存储，轮询请求状态，

AIO是一种异步非阻塞IO。客户端发起请求，服务端调用系统内核，处理完成后返回消息至客户端。



## UNIX 5中IO模型



#### 1、问答模式



### BIO阻塞原因

客户端发起请求，服务端立即处理，new SocketServer()阻塞了线程



### NIO优化

***Channel***

多路复用

ServerSocketChanel：服务连接和数据传输

***Selector***



***Buffer***

数据缓冲区





延伸：handler思想