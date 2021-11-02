## ioc作用：

​	管理对象的创建和依赖关系的维护解耦，由容器去维护具体的对象托管了类的产生过程，Spring 中的 IoC 的实现原理就是：工厂模式加反射机制

​	Spring IOC 负责创建对象，管理对象（通过依赖注入（DI），装配对象，配置对象，并且管理这些对象的整个生命周期。

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



## ApplicationContext和BeanFactory有什么区别

BeanFactory是Spring中非常核心的组件，表示Bean工厂，可以生成Bean，维护Bean，而ApplicationContext继承了BeanFactory，所以Application Context拥有Bean Factor所有的特点，也是一个Bean工厂，但是Application Context除开继承了Bean Factory之外，还继承了诸如EnvironmentCapable、MessageSource、ApplicationEventPublisher等接口，从而Application Context还有获取系统环境变量、国际化、事件发布等功能，这是BeanFactory所不具备的