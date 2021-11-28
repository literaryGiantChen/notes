## AOP **和** **IOC** **概念** 

### AOP

​	Aspect Oriented Program, 面向(方面)切面的编程;Filter(过滤器) 也是一种 AOP. AOP 是一种新的方法论, 是对传统 OOP(Object-Oriented Programming, 面向对象编程) 的补充. AOP 的主要编程对象是切面(aspect), 而切面模块化横切关注点.可以举例通过事务说明.

### IOC 

​	把复杂系统分解成相互合作的对象，这些对象类通过封装以后，内部实现对外部是透明的，从而降低了解决问题的复杂度，而且可以灵活地被重用和扩展

​	Invert Of Control, 控制反转. 也成为 DI(依赖注入)其思想是反转 资源获取的方向. 传统的资源查找方式要求组件向容器发起请求查找资源.作为 回应, 容器适时的返回资源. 而应用了IOC 之后, 则是容器主动地将资源推送 给它所管理的组件,组件所要做的仅是选择一种合适的方式来接受资源. 这种行 为也被称为查找的被动形式

### ioc作用：

​	管理对象的创建和依赖关系的维护解耦，由容器去维护具体的对象托管了类的产生过程，Spring 中的 IoC 的实现原理就是：工厂模式加反射机制

​	Spring IOC 负责创建对象，管理对象（通过依赖注入（DI），装配对象，配置对象，并且管理这些对象的整个生命周期。

### ioc理解：

ioc是一种编程思想，用来解耦提高代码的可维护性的

### 什么是IOC设计模式

按照我的理解，我认为ioc设计模式 是抽象工厂模式的升级版。抽象工厂方法是从工厂类中获取同一接口的不同实现，虽然看似是减少了耦合。但是耦合代码实际上还是存在的。而ioc模式呢，是直接将耦合的代码移出去，通过配置xml文件的方式替代耦合代码，在容器启动的时候，ioc根据配置的XML文件生成依赖对象并注入，所以在使用ioc模式后，使抽象工厂内部代码耦合转到了外部的配置文件，从而实现真正解耦，这就是我理解升级版抽象工厂模式。



## 所谓依赖注入（Dependency Injection）

即组件之间的依赖关系由容器在应用系统运行期来决定，也就是由容器动态地将某种依赖关系的目标对象实例注入到应用系统中的各个关联的组件之中
	依赖注入的基本原则是：应用组件不应该负责查找资源或者其他依赖的协作对象。
	主要优势为：
		查找定位操作与应用代码完全无关。
		不依赖于容器的 API，可以很容易地在任何容器以外使用应用对象。
		不需要特殊的接口，绝大多数对象可以做到完全不必依赖容器



## spring beans你怎样定义类的作用域？

​	当定义一个 在 Spring 里，我们还能给这个 bean 声明一个作用域。它可以通过 bean 定义中的 scope 属性来定义。如，当 Spring 要在需要的时候每次生产一个新的 bean 实例，bean 的 scope 属性被指定为 prototype。另一方面，一个 bean 每次使用的时候必须返回同一个实例，这个 bean 的 scope 属性 必须设为 singleton。

singleton : 	bean 在每个 Spring ioc 容器中只有一个实例。

prototype：  一个 bean 的定义可以有多个实例。

request：     每次 http 请求都会创建一个 bean，该作用域仅在基于 web 的 SpringApplicationContext 情形下有效。

session：	 在一个 HTTP Session 中，一个 bean 定义对应一个实例。该作用域仅在基于 web 的SpringApplicationContext 情形下有效。

global-session：	在一个全局的 HTTP Session 中，一个 bean 定义对应一个实例。该作用域仅在基于 web 的 Spring ApplicationContext 情形下有效。



## Spring 框架中的单例 bean 是线程安全的吗？

​	不是，Spring 框架中的单例 bean 不是线程安全的。spring 中的 bean 默认是单例模式，spring 框架并没有对单例 bean 进行多线程的封装处理。实际上大部分时候 spring bean 无状态的（比如 dao 类），所有某种程度上来说 bean 也是安全的，但如果 bean 有状态的话（比如 view model 对象），那就要开发者自己去保证线程安全了，最简单的就是改变 bean 的作用域，把“singleton”变更为“prototype”，这样请求 bean 相当于 new Bean()了，所以就可以保证线程安全了。
​		**有状态就是有数据存储功能、无状态就是不会保存数据**



## spring bean生命周期

Spring 对 bean 进行实例化；
Spring 将值和 bean 的引用注入到 bean 对应的属性中；
如果 bean 实现了 BeanNameAware 接口，Spring 将 bean 的 ID 传递给 setBean-Name()方法；
如果 bean 实现了 BeanFactoryAware 接口，Spring 将调用 setBeanFactory()方法，将BeanFactory 容器实例传入；
如果 bean 实现了 ApplicationContextAware 接口，Spring 将调用setApplicationContext()方法，将 bean 所在的应用上下文的引用传入进来；
如果 bean 实现了 BeanPostProcessor 接口，Spring 将调用它们的post-ProcessBeforeInitialization()方法；
如果 bean 实现了 InitializingBean 接口，Spring 将调用它们的 after-PropertiesSet()方法。
如果 bean 使用 initmethod 声明了初始化方法，该方法也会被调用；
如果 bean 实现了 BeanPostProcessor 接口，Spring 将调用它们的post-ProcessAfterInitialization()方法；
此时 bean 已经准备就绪，可以被应用程序使用了，它们将一直驻留在应用上下文中，直到该应用上下文被销毁；
如果 bean 实现了 DisposableBean 接口，Spring 将调用它的 destroy()接口方法。同样，
如果 bean 使用 destroy-method 声明了销毁方法，该方法也会被调用。

现在你已经了解了如何创建和加载一个 Spring 容器。但是一个空的容器并没有太大的价值，在你把东西放进去之前，它里面什么都没有。为了从 Spring 的 DI(依赖注入)中受益，我们必须将应用对象装配进 Spring 容器中。



**①. 通过构造器或工厂方法创建 Bean 实例**

**②. 为 Bean 的属性设置值和对其他 Bean 的引用**

**③ . 将 Bean 实例传递给 Bean 后置处理器的 postProcessBeforeInitialization 初始化前的后期处理方法**

**④. 调用 Bean 的初始化方法(init-method)**

**⑤ . 将 Bean 实例传递给 Bean 后置处理器的 postProcessAfterInitialization 初始化后的后期处理方法**

**⑦. Bean 可以使用了**

**⑧. 当容器关闭时, 调用 Bean 的销毁方法(destroy-method)**



## 哪些是重要的 bean 生命周期方法？ 你能重载它们吗？

第一个是 setup，它是在容器加载 bean 的时候被调用

第二个方法是 teardown，它是在容器卸载类的时候被调用



## 在 Spring 中如何注入一个 java 集合？

类型用于注入一列值，允许有相同的值。
类型用于注入一组值，不允许有相同的值。
类型用于注入一组键值对，键和值都可以为任意类型。
类型用于注入一组键值对，键和值都只能为 String 类型。



## 什么是 bean 装配？

Spring 容器中把 bean 组装到一起，前提是容器需要知道 bean的依赖关系，

如何通过依赖注入来把它们装配到一起

### Bean 的配置方式

​	通过全类名（反射）、通过工厂方法（静态工厂方法 & 实 例工厂方法）、FactoryBean



## @Autowired

默认是按照类型装配注入的



## @Autowired 和@Resource 之间的区别

@Autowired默认是按照类型装配注入的，

​	默认情况下它要求依赖对象必须存在（可以设置它 required 属性为 false）



@Resource 默认是按照名称来装配注入的，

​	只有当找不到与名称匹配的 bean 才会按照类型来装配注入



## 循环依赖

三级缓存，提前暴露对象：aop

总：什么是循环依赖问题，A依赖B,B依赖A

分：说明bean的创建过程：实例化，初始化（填充属性）

​	1、先创建A对象，实例化A对象，此时A对象中的B属性为空，填充属性B	

​	2、先创建A对象，如果找到了，直接赋值不存在循环依赖问题（不通），找不到直接创建B对象

​	3、实例化B对象，此时B对象中的a属性为空，填充属性A

​	4、从容器中查找A对象，找不到，直接创建

​	形成闭环的原因

​	此时，如果仔细琢磨的话，会发现A对象是存在的，只不过此时的A对象不是一个完整的状态，只完成了实例化但是未完成初始化，如果在程序调用过程中，拥有了某个对象的引用，能否在后期给他完成赋值操作，可以优先把非完整状态的对象优先赋值，等待后续操作来完成赋值，相当于提前暴露了某个不完整对象的引用，所以解决问题的核心在于实例化和初始化分开操作，这也是解决循环依赖问题的关键，

​	当所有的对象都完成实例化和初始化操作之后，还要把完整对象放在容器中，此时在容器中存在对象的几个状态，完成实例化=但未完成初始化，完整状态，因为都在容器中，所以要使用不同的map结构来进行存储，此时就有了一级缓存和二级缓存，如果一级缓存中有了，那么二级缓存中就不会存在同名的对象，因为他们的查找顺序是1、2、3这样的方式来查找的，一级缓存中放的是完整对象，二级缓存中放的是非完整对象

​	为什么需要三级缓存？三级缓存的value类型是ObjectFactory,是一个函数式接口，存在的意义是保证在整个容器的运行过程中同名的bean对象只能有一个。

​	如果一个对象需要被代理，或者说需要生成代理对象，那么要不要优先生成一个普通对象？答案：要

​	普通对象和代理对象是不能同时出现在容器中的，因此当一个对象需要被代理的时候，就要使用代理覆盖之前的普通对象，在实际的调用过程中，是没有办法确定什么时候对象被使用，所以就要求当某个对象那个被调用的时候，优先判断此对象是否需要被代理，类似于一种回调机制的实现，因此传入lambda表达式的时候，可以通过lambda表达式来执行对象的覆盖过程，getEarlyBeanReference()

​	因此，所有的bean对象在创建的时候都要优先放在三级缓存中，在后续的使用过程中，如果需要被代理则返回代理对象，如果不需要被代理，则直接返回普通对象



## 字段注入

三点缺陷：1.不具备外部可见性 2.会导致循环依赖 3.无法注入不可变对象	

三种依赖注入类型：1.构造器注入适用于强制对象注入 2.setter注入适合于可选对象注入 3.字段注入应该避免，因为对象无法脱离容器而独立运行



## ApplicationContext 和 BeanFactory 有什么区别

BeanFactory是Spring中非常核心的组件，表示Bean工厂，可以生成Bean，维护Bean，而ApplicationContext继承了BeanFactory，所以Application Context拥有Bean Factor所有的特点，也是一个Bean工厂，但是Application Context除开继承了Bean Factory之外，还继承了诸如EnvironmentCapable、MessageSource、ApplicationEventPublisher等接口，从而Application Context还有获取系统环境变量、国际化、事件发布等功能，这是BeanFactory所不具备的



## @Transactional声明事务失效

https://mp.weixin.qq.com/s?__biz=MzAxODcyNjEzNQ==&mid=2247546160&idx=2&sn=3780e10063651f2cc3042f5260d6fb18&chksm=9bd398a8aca411bebf223026d8f2a7251b59a35842366f0ba85b359363566449f55bd28702a5&mpshare=1&scene=23&srcid=1116piM7SUK2PPTg9axEpNZh&sharer_sharetime=1637029661075&sharer_shareid=a6f679473defed89d98ad0e43953a956#rd

### 1. 在同一个类中调用

```
public class A {
    
    public void methodA() {
        methodB();
        
        // 其他操作
    }

    @Transactional
    public void methodB() {
        // 写数据库操作
    }
    
}
```

### 2. @Transactional修饰方法不是public

```
public class TransactionalMistake {
    
    @Transactional
    private void method() {
        // 写数据库操作
    }
    
}
```

### 3. 不同的数据源

```
public class TransactionalMistake {

    @Transactional
    public void createOrder(Order order) {
        orderRepo1.save(order);
        orderRepo2.save(order);
    }

}
```

### 4. 回滚异常配置不正确

```
public class TransactionalMistake {

    @Transactional(rollbackFor = XXXException.class)
    public void method() throws XXXException {

    }

}
```

### 5. 数据库引擎不支持事务

```
spring.jpa.database-platform=org.hibernate.dialect.MySQL5InnoDBDialect
```



## 为什么IDEA不推荐你使用@Autowired ？

但是当我们使用IDEA写代码的时候，经常会发现`@Autowired`注解下面是有小黄线的，我们把小鼠标悬停在上面，可以看到这个如下图所示的警告信息：`Field injection is not recommended`

@Autowired

Field Injection:这种注入方式通过Java的反射机制实现，所以private的成员也可以被注入具体的对象。

```
@Controller
public class UserController {

    @Autowired
    private UserService userService;

}
```

Constructor Injection:是构造器注入，是我们日常最为推荐的一种使用方式。

```
@Controller
public class UserController {

    private final UserService userService;

    public UserController(UserService userService){
        this.userService = userService;
    }

}
```

Setter Injection:就是通过调用成员变量的set方法来注入想要使用的依赖对象。

```
@Controller
public class UserController {

    private UserService userService;

    @Autowired
    public void setUserService(UserService userService){
        this.userService = userService;
    }
}
```

### 三个依赖之间对比

**可靠性**

从对象构建过程和使用过程，看对象在各阶段的使用是否可靠来评判：

- `Field Injection`：不可靠
- `Constructor Injection`：可靠
- `Setter Injection`：不可靠

由于构造函数有严格的构建顺序和不可变性，一旦构建就可用，且不会被更改。

**可维护性**

主要从更容易阅读、分析依赖关系的角度来评判：

- `Field Injection`：差
- `Constructor Injection`：好
- `Setter Injection`：差

还是由于依赖关键的明确，从构造函数中可以显现的分析出依赖关系，对于我们如何去读懂关系和维护关系更友好。

**可测试性**

当在复杂依赖关系的情况下，考察程序是否更容易编写单元测试来评判

- `Field Injection`：差
- `Constructor Injection`：好
- `Setter Injection`：好

`Constructor Injection`和`Setter Injection`的方式更容易Mock和注入对象，所以更容易实现单元测试。

**灵活性**

主要根据开发实现时候的编码灵活性来判断：

- `Field Injection`：很灵活
- `Constructor Injection`：不灵活
- `Setter Injection`：很灵活

由于`Constructor Injection`对Bean的依赖关系设计有严格的顺序要求，所以这种注入方式不太灵活。相反`Field Injection`和`Setter Injection`就非常灵活，但也因为灵活带来了局面的混乱，也是一把双刃剑。

**循环关系的检测**

对于Bean之间是否存在循环依赖关系的检测能力：

- `Field Injection`：不检测
- `Constructor Injection`：自动检测
- `Setter Injection`：不检测

**性能表现**

不同的注入方式，对性能的影响

- `Field Injection`：启动快
- `Constructor Injection`：启动慢
- `Setter Injection`：启动快

主要影响就是启动时间，由于`Constructor Injection`有严格的顺序要求，所以会拉长启动时间。

所以，综合上面各方面的比较，可以获得如下表格：

![图片](https://mmbiz.qpic.cn/mmbiz_png/R3InYSAIZkEEzMm29UOBU7ODGL8WiabxVCrFGD4eszVx88NPFRaWjLCSdv0sVqXzyerbnahiaz2DaibMd5C7XTXmQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

结果一目了然，`Constructor Injection`在很多方面都是优于其他两种方式的，所以`Constructor Injection`通常都是首选方案！

而`Setter Injection`比起`Field Injection`来说，大部分都一样，但因为可测试性更好，所以当你要用`@Autowired`的时候，推荐使用`Setter Injection`的方式，这样IDEA也不会给出警告了。同时，也侧面反映了，可测试性的重要地位啊！