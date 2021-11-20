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