## 过滤器 和 拦截器

过滤器是在javaweb才有的，而拦截器在spring中管理的，但是可以将过滤器交给spring管理

1.实现原理不同：过滤器 是基于函数回调的，拦截器 则是基于Java的反射机制（动态代理）实现的

2.使用范围不同：过滤器 实现的是 javax.servlet.Filter 接口，而这个接口是在Servlet规范中定义的，也就是说过滤器Filter 的使用要依赖于Tomcat等容器，导致它只能在web程序中使用。而拦截器(Interceptor) 它是一个Spring组件，并由Spring容器管理，并不依赖Tomcat等容器，是可以单独使用的。不仅能应用在web程序中，也可以用于Application、Swing等程序中。

3.触发时机不同：过滤器Filter是在请求进入容器后，但在进入servlet之前进行预处理，请求结束是在servlet处理完以后。

拦截器 Interceptor 是在请求进入servlet后，在进入Controller之前进行预处理的，Controller 中渲染了对应的视图之后请求结束。

4.拦截的请求范围不同：执行顺序 ：Filter 处理中 -> Interceptor 前置 -> 我是controller ->Interceptor 处理中 -> Interceptor 处理后

过滤器Filter执行了两次，拦截器Interceptor只执行了一次。这是因为过滤器几乎可以对所有进入容器的请求起作用，而拦截器只会对Controller中请求或访问static目录下的资源请求起作用。

5.注入Bean不同

6.控制执行顺序不同

https://www.zhihu.com/question/30212464/answer/1786967139



## 如何注册filter

1.web.xml

2.@webFilter

3.FilterRegisterBean + Filter

javaweb的filter的配置会加载在server 的context中

FilterRegisterBean 继承的父类，实现了ServletContextInitializer

![springboot-FilterRegistrationBean](..\notes\截图\springboot-FilterRegistrationBean.jpg)

filter的以下方法去往servlet里面context添加

![springboot-filter](..\notes\截图\springboot-filter.jpg)



## filter计模式

责任链模式



## servlet



## http

1.无状态：默认是单例模式，不会去影响上一次或者下一次的请求，只会处理当前请求的参数

2.传输类型多：	

3.易修改			 

4.可编程