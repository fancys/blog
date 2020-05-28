## Spring Cloud Alibaba实现服务注册与发现

### 运行环境

+ jdk1.8 +
+ maven 3.0+
+ spring cloud alibaba 2.1.0.RELEASE
+ spring boot 2.1.8.RELEASE
+ spring cloud Greenwich.SR3

### Maven依赖

```maven
 <dependency>
 	<groupId>com.alibaba.cloud</groupId>
 	<artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
 </dependency>

<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-alibaba-dependencies</artifactId>
            <version>2.1.0.RELEASE</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

### 配置文件

```
nacos:
  server: 192.168.3.239:8848
spring:
  application:
    name: auth-server
  cloud:
    nacos:
      discovery:
        server-addr: ${nacos.server}
        namespace: 5f971a79-6fc8-43a4-aa08-ad7ccea3d0ec
```



### 注意点

1. discovery.server-addr配置格式`ip:port`（如果端口为8848，则可省略），不能带有`http://`或者`https://`。

因为在`NacosDiscoveryProperties`中对`server-addr`属性做了如下判断：

```java
if (this.serverAddr.lastIndexOf("/") != -1) {
    this.serverAddr = this.serverAddr.substring(0, this.serverAddr.length() - 1);
}
```

2. 