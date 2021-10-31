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