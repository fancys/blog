# spring cloud 集成Feign组件实现服务消费

 上一章我们通过Eureka实现了服务的注册与发现，这章我们将讲解如何通过Fegin去消费注册的服务。

我们提供了salesorder服务，并在注册中心注册成功，即在注册中心我们有了salesorder的服务提供者，现在我们需要提供服务消费者调用salesorder服务。

下面以salesdelivery服务调用salesorder服务为例。

 ## 改造salesorder服务
 1、client项目中添加依赖：

 ```maven
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-openfeign</artifactId>
    </dependency>
 ```
 由于高版本中没有相关注解，还需在父项目中引入对应版本依赖：

 ```maven
 <dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-dependencies</artifactId>
            <version>${spring-cloud.version}</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-openfeign</artifactId>
            <version>2.1.2.RELEASE</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
 ```

 2、在client模块中添加`SalesOrderServiceClient`接口

 ```java
 @FeignClient("salesorder.microservice")
 public interface SalesOrderServiceClient {

    @GetMapping("edi/v1/salesorder")
    SalesOrderOutput fetchSalesOrder();
 }
 ```
 ***Mapping中的值为方法对应的Controller类的Mapping+方法的Mapping***

 对client模块进行`mvn clean install`,将client模块打包到本地仓库。

 ## salesdelivery项目调用salesorder服务
 1、新建salesdelivery项目，项目结构参考salesorder项目。

 在server模块中引用salesorder.client模块

 ```maven
 <dependency>
    <groupId>com.avatech.edi</groupId>
    <artifactId>salesorder.client</artifactId>
    <version>0.0.1-SNAPSHOT</version>
 </dependency>
 ```
 在Application类加上注解`@EnableFeignClients(basePackages = "com.avatech.edi.salesorder.client")`

2、调用salesorder服务

添加client声明，即可本地方法调用。
```java
    @Autowired
    SalesOrderServiceClient salesOrderServiceClient;

    @GetMapping("salesdelivery")
    public SalesOrderOutput getSalesOrder(){
        SalesOrderOutput orderOutput = salesOrderServiceClient.fetchSalesOrder();
        return orderOutput;
    }
```

启动项目后，我们调用salesdelivery的服务，发现返回结果与直接调用salesorder的服务结果一致。

## 总结
通过上面示例，我们发现使用Feign组件实现服务消费功能主要有以下几个步骤：

+ 服务提供者
    + 添加Fegin依赖。注意高版本的版本号，且版本号要与spring cloud的版本对应。

    + 添加接口，添加Feign注解

    + 声明服务方法，添加Mapping。注意Mapping的值与Controller中的Mapping值和方法的Mapping值

    + 打包项目

+ 服务消费者
    + 添加项目依赖

    + Autowired调用服务





