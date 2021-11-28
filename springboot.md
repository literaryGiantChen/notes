## 是什么

​	Spring Boot是由Pivotal团队提供的全新框架，其设计目的是用来简化新Spring应用的初始搭建以及开发过程。

​	在Spring框架这个大家族中，产生了很多衍生框架，比如 Spring、SpringMvc框架等，Spring的核心内容在于控制反转(IOC)和依赖注入(DI),所谓控制反转并非是一种技术，而是一种思想，在操作方面是指在spring配置文件中创建<bean>，依赖注入即为由spring容器为应用程序的某个对象提供资源，比如 引用对象、常量数据等。

​	SpringBoot是一个框架，一种全新的编程规范，他的产生简化了框架的使用，所谓简化是指简化了Spring众多框架中所需的大量且繁琐的配置文件，所以 SpringBoot是一个服务于框架的框架，服务范围是简化配置文件。

## 特点

​	让文件配置变的相当简单、让应用部署变的简单（SpringBoot内置服务器，并装备启动类代码），可以快速开启一个Web容器进行开发。



## 核心功能

1、 可独立运行的Spring项目：Spring Boot可以以jar包的形式独立运行。

2、 内嵌的Servlet容器：Spring Boot可以选择内嵌Tomcat、Jetty或者Undertow，无须以war包形式部署项目。

3、 简化的Maven配置：Spring提供推荐的基础 POM 文件来简化Maven 配置。

4、 自动配置Spring：Spring Boot会根据项目依赖来自动配置Spring 框架，极大地减少项目要使用的配置。

5、 提供生产就绪型功能：提供可以直接在生产环境中使用的功能，如性能指标、应用信息和应用健康检查。

6、 无代码生成和xml配置：Spring Boot不生成代码。完全不需要任何xml配置即可实现Spring的所有配置。



## 优势

简化配置

产品级独立运行

强化的场景启动器



## 相关注解

@Conditional：

​	一个类在满足特定条件时才加入IOC容器中

@Configuration：

​	表示当前类是一个配置文件类



## 工作原理

![springboot工作原理](\截图\springboot工作原理.jpg)



## thymeleaf

官方文档：https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html

```
<html xmlns:th="http://www.thymeleaf.org" lang="en">
```

${session.名} 访问session域中的数据

${application.名} 访问application域



解析URL地址

```
<p th:text="@{/aa/bb}">test</p>
```



直接执行表达式

```
<p th:text="@{/aa/bb}">[[${test}]]</p>
or
<p th:text="@{/aa/bb}">[(${test})]</p>
```

区别在于：[[]]会将内容中的html标签转义，而[()]会将内容中的html标签解析



条件判断

```
<p th:if="${not #strings.isEmpty(reqAttrName)}">test</p>
```



循环

```
<div th:each="arr : ${list}">
    <p th:text="${arr}"></p>
</div>
or
<div>
    <p th:text="${arr}" th:each="arr : ${list}"></p>
</div>
```

第一种写法会对div及子元素循环、第二种写法会只对p元素循环



![springboot-thymeleaf-包含其他模板](\截图\springboot-thymeleaf-包含其他模板.jpg)