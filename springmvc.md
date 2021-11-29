## 线程本地化

RequestContext.getCurrentContext().getRequest(); 是使用threadLocal,从上游走到下游的，方便对象共享。

框架底层是借助 ThreadLocal 从当前线程上获取事先绑定的 Request 对象



## 请求处理的流程

1. 首先有一个统一的处理请求的入口
2. 随后根据请求路径找到对应的映射器
3. 找到处理请求的适配器
4. 拦截器前置处理
5. 真实处理请求（也就是真正的代码）
6. 视图解析器处理
7. 拦截器后置处理



统一的处理入口，对应的SpringMVC下的源码是在DispatcherServlet下的实现的该对象，在初始化就会把映射器、适配器、视图解析器、异常处理器、文件处理器等等初始化掉，至于会初始化那些具体实例，看下DispatcherServlet.properties就知道了，都配置在哪了，所有的请求都会被doService方法处理，里边最主要就是调用doDispatch方法，通过doDispatch方法我们就可以看到整个SpringMVC处理的流程，查找映射器的时候实际就是找到{最佳匹配}的路径，具体方法实现我记得好像是在lookupHandlerMethod方法上，从源码可以看到{查找映射器}实际返回的是HandlerExecutionChan，里边有映射器Handler+拦截器List，前面提到的拦截器前置处理和后置处理就是用的HandlerExecutionChain中的拦截器List，获取得到HandlerExecutionChain后，就会去获取适配器，一般我们获取得到的就是RequestMappingHandlerAdapter，在代码里边可以看到的是，经常用到的@ResponseBody和@Requestbody的解析器，就会在初始化的时候加到参数解析器List中，得到适配器之后，就会执行拦截器前后处理，拦截器前置处理执行完后，就会调用适配器对象实例的hanlde方法执行真正的代码逻辑处理，核心的处理逻辑在invokerAndHandler出口中，会获取得到请求的参数并调用，处理返回值，参数的封装以及处理会被适配器的参数解析器进行处理，具体的处理逻辑取决于HttpMessageConverter的实例对象



DispatcherServlet 入口

DispatcherServlet.properties 会初始化对象

HandlerMapping 映射器

HandlerExecutionChain 映射器最终的实例+拦截器List

HttpRequestHandlerAdapter 适配器

HttpMessageConverter 数据转换