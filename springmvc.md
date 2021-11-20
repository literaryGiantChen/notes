## 线程本地化

RequestContext.getCurrentContext().getRequest(); 是使用threadLocal,从上游走到下游的，方便对象共享。

框架底层是借助 ThreadLocal 从当前线程上获取事先绑定的 Request 对象