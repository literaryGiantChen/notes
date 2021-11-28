## 核心

springcloud基于HTTP协议，和Dubbo最本质的区别，Dubbo的核心是基于RPC

![RPC结构图](\截图\RPC结构图.jpg)

中间件：springcloud提供的微服务架构整体管理的一站式解决方案

## 组件

Spring Cloud Netflix：核心组件，可以对多个Netflix OSS开源套件进行整合，包括以下几个组件：

Eureka：服务治理组件，包含服务注册与发现

Hystrix：容错管理组件，实现了熔断器

Ribbon：客户端负载均衡的服务调用组件

Feign：基于Ribbon和Hystrix的声明式服务调用组件

Zuul：网关组件，提供智能路由、访问过滤等功能

Getway : 网关组件, 对API进行路由，以及提供一些强大的过滤器功能， 例如：熔断、限流、重试等

Archaius：外部化配置组件

Spring Cloud Config：配置管理工具，实现应用配置的外部化存储，支持客户端配置信息刷新、加密/解密配置内容等。

Spring Cloud Bus：事件、消息总线，用于传播集群中的状态变化或事件，以及触发后续的处理

Spring Cloud Stream : 消息驱动

Spring Cloud Sleuth :  分布式请求链路跟踪

Spring Cloud Security：基于spring security的安全工具包，为我们的应用程序添加安全控制

Spring Cloud Consul : 封装了Consul操作，Consul是一个服务发现与配置工具（与Eureka作用类似），与Docker容器可以无缝集成

Spring Cloud Alibaba Nacos : 注册中心、配置中心， Euerka+Config+Bus

Spring Cloud Alibaba Sentinel :  实现熔断与限流

Spring Cloud Alibaba Seata : 处理分布式事务 @GlobalTransactional(name = "fsp-create-order",rollbackFor = Exception.class)



### 组件之间的关联

![springcloud组件之间的关联](\截图\springcloud组件之间的关联.jpg)



## eureka底层原理

https://www.jianshu.com/p/56155d2bde6b

![img](https://upload-images.jianshu.io/upload_images/16877112-f61cd8bcb004989d.png?imageMogr2/auto-orient/strip|imageView2/2/w/888/format/webp)

如上图所示，图中的这个名字叫做**registry**的**CocurrentHashMap**，就是注册表的核心结构。看完之后忍不住先赞叹一下，精妙的设计！

从代码中可以看到，Eureka Server的注册表直接基于**纯内存**，即在内存里维护了一个数据结构。

各个服务的注册、服务下线、服务故障，全部会在内存里维护和更新这个注册表。

各个服务每隔30秒拉取注册表的时候，Eureka Server就是直接提供内存里存储的有变化的注册表数据给他们就可以了。

同样，每隔30秒发起心跳时，也是在这个纯内存的Map数据结构里更新心跳时间。
 一句话概括：维护注册表、拉取注册表、更新心跳时间，全部发生在内存里！这是Eureka Server非常核心的一个点



1、Eureka Server 启动成功，等待服务端注册。在启动过程中如果配置了集群，集群之间定时通过 Replicate 同步注册表，每个 Eureka Server 都存在独立完整的服务注册表信息

2、Eureka Client 启动时根据配置的 Eureka Server 地址去注册中心注册服务

3、Eureka Client 会每 30s 向 Eureka Server 发送一次心跳请求，证明客户端服务正常

4、当 Eureka Server 90s 内没有收到 Eureka Client 的心跳，注册中心则认为该节点失效，会注销该实例

5、单位时间内 Eureka Server 统计到有大量的 Eureka Client 没有上送心跳，则认为可能为网络异常，进入自我保护机制，不再剔除没有上送心跳的客户端

6、当 Eureka Client 心跳请求恢复正常之后，Eureka Server 自动退出自我保护模式

7、Eureka Client 定时全量或者增量从注册中心获取服务注册表，并且将获取到的信息缓存到本地

8、服务调用时，Eureka Client 会先从本地缓存找寻调取的服务。如果获取不到，先从注册中心刷新注册表，再同步到本地缓存

9、Eureka Client 获取到目标服务器信息，发起服务调用

10、Eureka Client 程序关闭时向 Eureka Server 发送取消请求，Eureka Server 将实例从注册表中删除



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



## 负载均衡

负载均衡是云计算的基础组件，是网络流量的入口，其重要性不言而喻。

### 什么是负载均衡呢？

将用户请求或者说流量通过负载均衡器，按照某种负载均衡算法把流量均匀地分散到后端的多个服务器上，接收到请求的服务器可以独立的响应请求，以期望的规则分摊到多个操作单元上进行执行，达到负载分担的目的。并通过它可以实现横向扩展（scale out），将冗余的作用发挥为高可用

### LVS负载均衡

DR 模式、TUN 模式、NAT 模式

### 5 种策略

轮询、加权轮询、最少连接数、最快响应、hash法

![img](https://img-blog.csdnimg.cn/20190510011537906.jpeg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI5Njc3ODY3,size_16,color_FFFFFF,t_70)





## 微服务有什么好处

独立的可扩展性，每个微服务都可以独立进行横向或纵向扩展，根据业务实际增长情况来进行快速扩展；

独立的可升级性，每个微服务都可以独立进行服务升级、更新，不用依赖于其它服务，结合持续集成工具可以进行持续发布，开发人员就可以独立快速完成服务升级发布流程；

易维护性，每个微服务的代码均只专注于完成该单个业务范畴的事情，因此微服务项目代码数量将减少至IDE可以快速加载的大小，这样可以提高了代码的可读性，进而可以提高研发人员的生产效率；

语言无关性，研发人员可以选用自己最为熟悉的语言和框架来完成他们的微服务项目（当然，一般根据每个公司的实际技术栈需要来了），这样在面对新技术或新框架的选用时，微服务能够更好地进行快速响应；

故障和资源的隔离性，在系统中出现不好的资源操作行为时，例如内存泄露、数据库连接未关闭等情况，将仅仅只会影响单个微服务；

优化跨团队沟通，如果要完全实践微服务架构设计风格，研发团队势必会按照新的原则来进行划分，由之前的按照技能、职能划分的方式变为按照业务（单个微服务）来进行划分，如此这般团队里将有各个方向技能的研发人员，沟通效率上来说要优于之前按照技能进行划分的组织架构；

原生基于“云”的系统架构设计，基于微服务架构设计风格，我们能构建出来原生对于“云”具备超高友好度的系统，与常用容器工具如Docker能够很方便地结合，构建持续发布系统与IaaS、PaaS平台对接，使其能够方便的部署于各类“云”上，如公用云、私有云以及混合云。

## 微服务有什么缺点

增加了系统复杂性、运维难度增加、本地调用变成RPC应用，有些操作会比较耗时、可能会引入分布式事务