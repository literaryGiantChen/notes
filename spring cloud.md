## 核心

springcloud基于HTTP协议，和Dubbo最本质的区别，Dubbo的核心是基于RPC

![RPC结构图](\截图\RPC结构图.jpg)

中间件：springcloud提供的微服务架构整体管理的一站式解决方案

## 组件

​		Eureka注册中心

​		Ribbon客户端负载均衡

​		Feign远程接口的声明式调用

​		Hystrix服务的熔断、降级、监控

​		Zuul网关，类似于nginx一样，配置80端口，通过网关访问关联的服务接口

### 组件之间的关联

![springcloud组件之间的关联](\截图\springcloud组件之间的关联.jpg)



## 环境搭建

springboot和springclous版本对应：https://start.spring.io/actuator/info

```
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
  </properties>

  <dependencyManagement>
    <dependencies> <!-- 导入 SpringCloud 需要使用的依赖信息 -->
      <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-dependencies</artifactId>
        <version>Hoxton.SR12</version>
        <type>pom</type> <!-- import 依赖范围表示将 spring-cloud-dependencies 包中的依赖信息导入 -->
        <scope>import</scope>
      </dependency> <!-- 导入 SpringBoot 需要使用的依赖信息 -->
      <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-dependencies</artifactId>
        <version>2.3.12.RELEASE</version>
        <type>pom</type>
        <scope>import</scope>
      </dependency>
    </dependencies>
  </dependencyManagement>

  <build>
    <finalName>pro42-spring-cloud</finalName>
    <plugins>
      <plugin>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-maven-plugin</artifactId>
        <configuration>
          <fork>true</fork>
          <addResources>true</addResources>
        </configuration>
      </plugin>
    </plugins>
  </build>

```



## CAP

c：Consistency强一致性

a：Availability可用性

p：Partition tolerance分区容错性

cap理论关注粒度是数据，而不是整体系统设计的策略



​	最多只能同时较好的满足两个。
​	CAP理论的核心是：一个分布式系统不可能同时很好的满足一致性，可用性和分区容错性这三个需求，
因此，根据 CAP 原理将 NoSQL 数据库分成了满足 CA 原则、满足 CP 原则和满足 AP 原则三 大类：
​		CA - 单点集群，满足一致性，可用性的系统，通常在可扩展性上不太强大。
​		CP - 满足一致性，分区容忍必的系统，通常性能不是特别高。
​		AP - 满足可用性，分区容忍性的系统，通常可能对一致性要求低一些。