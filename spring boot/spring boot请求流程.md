### Spring boot 请求流程



spring boot内嵌tomcat，根据servlet规范，所有请求都被移交到前端控制器（DispatchServlet）的doService(HttpServletRequest request, HttpServletResponse response)处理。

在`spring boot`中`DispatcherServlet`继承了`FrameworkServlet`，并实现了doService方法。

doService方法主要设置了request后，然后调用了doDispatch()方法。



doDispatch()方法的主要逻辑

1. 通过getHandler()获取HandleMapping
2. 获取HandlerAdapter，然后执行handle方法。

```java
doDispath(){
  
  HandlerAdapter ha = getHandlerAdapter(mappedHandler.getHandler());
  ...
  mv = ha.handle(processedRequest, response, mappedHandler.getHandler());
}
```



在执行handle方法时，主要经历以下：

`AbstractHandlerMethodAdapter`

`RequestMappingHandlerAdapter` invokeHandlerMethod()方法

`ServletInvocableHandlerMethod` invokeAndHandle()方法：解析参数，调用对用的controller方法，设置返回值

`HandlerMethodArgumentResolverComposite` getArgumentResolver()方法

`RequestResponseBodyMethodProcessor` resolveArgument()方法

`AbstractMessageConverterMethodProcessor`

`HandlerMethodReturnValueHandler`

`HandlerMethodReturnValueHandlerComposite` handleReturnValue()方法

`RequestResponseBodyMethodProcessor`  handleReturnValue()方法

