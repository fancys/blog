# 使用Spring Cloud OAuth 2.0实现服务身份校验

## OAuth 2.0概念

[OAuth2.0](https://tools.ietf.org/html/draft-ietf-oauth-v2-31)

[OAuth2-servers](https://www.oauth.com/oauth2-servers/definitions/)

[RFC 6749-OAuth 2.0授权框架](https://github.com/jeansfish/RFC6749.zh-cn)

### 四种角色

+ 资源拥有着

+ 鉴权认证服务器

+ 资源服务器

+ 客户端


### 四种授权模式

+ 授权码模式

+ 密码模式

+ 客户端模式

+ 简单模式

### 其他概念

+ EndPoint

## Spring Cloud OAuth 2.0



### 授权服务端配置

**AuthorizationServer配置**

继承类`AuthorizationServerConfigurerAdapter`,重写以下方法，实现相应逻辑。

```java
    @Override
    public void configure(AuthorizationServerSecurityConfigurer security) throws Exception {

    }

    @Override
    public void configure(ClientDetailsServiceConfigurer clients) throws Exception {
        //配置客户端认证

    }

    @Override
    public void configure(AuthorizationServerEndpointsConfigurer endpoints) throws Exception {
        //配置token的数据源、自定义的tokenServices等信息

    }

    @Override
    public void configure(AuthorizationServerSecurityConfigurer oauthServer) {
        //允许表单认证，默认为false
        oauthServer.allowFormAuthenticationForClients();
    }
```

**WebSecurityConfigurer配置**

继承`WebSecurityConfigurerAdapter`，重写以下三个方法。

```java
    @Override
    protected void configure(HttpSecurity http) throws Exception {

    }

    @Override
    public void configure(WebSecurity web) {

    }

    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {

    }
```

### 资源服务端配置

**ResourceServerConfigurer配置**


继承`ResourceServerConfigurerAdapter`,重写以下方法：

```java
    @Override
    public void configure(HttpSecurity http) throws Exception {

    }
```





## 参考

[spring-security-oauth](https://github.com/spring-projects/spring-security-oauth)

[芋道 Spring Security OAuth2 入门](http://www.iocoder.cn/Spring-Security/OAuth2-learning/)