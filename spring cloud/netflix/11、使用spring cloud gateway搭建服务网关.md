# 使用spring cloud gateway基本使用

## 基础环境

+ spring cloud Greenwich.SR3 版本

+ JDK 1.8

+ spring cloud eureka

+ spring cloud gateway


## 路由

### maven依赖

```maven
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-gateway</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
```

### application.yml配置

```yml
spring:
  application:
    name: cloud.gateway
  cloud:
      gateway:
        discovery:
          locator:
            enabled: true
eureka:
  client:
    service-url:
     defaultZoon: http://localhost:8761/eureka
server:
  port: 9090

logging:
  level:
    org.springframework.cloud.gateway: debug
```

Spring Boot启动类不用添加相关`Enable*`注解即可启动。

我们根据服务名称从网关端口访问：`http://fancy-pc:9090/SALESDELIVERY.MICROSERVICE/edi/v1/salesdelivery`

我们发现返回的结果与直接访问salesdelivery服务的结果一致，这是因为将`spging.cloud.gateway.discovery.locator.enabled`设置为了`true`。`Spring Cloud GateWay`会默认为我们创建一个`routes`配置:

```yml
routes:
  - id: CompositeDiscoveryClient_SALESDELIVERY.MICROSERVICE
    uri: lb://SALESDELIVERY.MICROSERVICE
    order: 0
    predicates:
      - Path=/SALESDELIVERY.MICROSERVICE/**
    filters:
      - RewritePath=/SALESDELIVERY.MICROSERVICE/(?<segment>.*), /$\{segment}

```

***注意***： serviceId会默认为大写，与Eureka中的applicationName一致，所以url中的serviceId名称必须全部大写。


### 自定义路由规则

在配置文件中添加如下配置：
```yml
routes:
    - id: service_delivery
    uri: lb://SALESDELIVERY.MICROSERVICE
    order: 0
    predicates:
        - Path=/delivery/**
    filters:
        - StripPrefix=1
        - AddResponseHeader=X-Response-Default-Foo, Default-Bar
```

此时，再调用`http://fancy-pc:9090/delivery/edi/v1/salesdelivery`发现返回结果与直接访问`salesdelivery`服务一致，同时在返回的`header`中也包含了`X-Response-Default-Foo`标签。

## 过滤器

### 生命周期

|名称|执行时间|
|--|---|
|pre|请求被执行前调用|
|post|请求被执行后调用|

### 自定义局部过滤器

实现`GatewayFilter`和`Ordered`，其中`GatewayFilter`的方法`Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain)`实现相关逻辑；而`Ordered`的`getOrder()`用于定义`filter`的优先级。


### 自定义全局过滤器


### 自定义过滤器工厂
