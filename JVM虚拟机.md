## volatile原理

​	synchronized 表示只有一个线程可以获取作用对象的锁，执行代码，阻塞其他线程。
​	volatile 表示变量在 CPU 的寄存器中是不确定的，必须从主存中读取。保证多线程环境下变量的可见性；禁止指令重排序

知乎：java小刀



## 将常量压入栈的指令：

​	aconst_null 将null对象引用压入栈
​	iconst_m1 将int类型常量-1压入栈
​	iconst_0 将int类型常量0压入栈
​	lconst_0 将long类型常量0压入栈
​	fconst_0 将float类型常量0压入栈
​	dconst_0 将double类型常量压入栈
​	bipush 将一个8位符号整数压入栈
​	sipush 将16位带符号整数压入栈
​	ldc 把常量池中的项压入栈
​	ldc_w 把常量池中的项压入栈（使用宽索引）
​	ldc2_w 把常量池中long类型或者double类型的项压入栈



## 双亲委派模型，类加载器

类加载器：

1.Boot StrapClassLoader 启动 C++

​	<java_home> / lib

2.ExtensionClassLoader java

​	<java_home> / lib/ext

3.ApplicationClassLoader

​	<ClassPath>

4.自定义加载器



## 命令：

命令帮助：javap

用法: javap <options> <classes>

```java
其中, 可能的选项包括:
  -? -h --help -help               输出此帮助消息
  -version                         版本信息
  -v  -verbose                     输出附加信息
  -l                               输出行号和本地变量表
  -public                          仅显示公共类和成员
  -protected                       显示受保护的/公共类和成员
  -package                         显示程序包/受保护的/公共类
                                   和成员 (默认)
  -p  -private                     显示所有类和成员
  -c                               对代码进行反汇编
  -s                               输出内部类型签名
  -sysinfo                         显示正在处理的类的
                                   系统信息 (路径, 大小, 日期, MD5 散列)
  -constants                       显示最终常量
  --module <模块>, -m <模块>       指定包含要反汇编的类的模块
  --module-path <路径>             指定查找应用程序模块的位置
  --system <jdk>                   指定查找系统模块的位置
  --class-path <路径>              指定查找用户类文件的位置
  -classpath <路径>                指定查找用户类文件的位置
  -cp <路径>                       指定查找用户类文件的位置
  -bootclasspath <路径>            覆盖引导类文件的位置

GNU 样式的选项可使用 = (而非空白) 来分隔选项名称
及其值。
```



## 内存模型

![JVM内存模型](.\截图\JVM内存模型.jpg)

堆内存：是给new 一个对象时，给对象存储的空间

栈(线程)：创建一个线程空间，比如main线程

程序计数器当前线程所执行的字节码的行号指示器，字节码解析器的工作是通过改变这个计数器的值，来选取下一条需要执行的字节码指令，分支、循环、跳转、异常处理、线程恢复等基础功能，都需要依赖这个计数器来完成

如果有更高的线程将当前时间片抢走了，执行完后，就是根据程序集计数器找到之前正在执行的程序。执行的行号是字节码执行引擎来修改的

![Java发编译文件](.\截图\Java发编译文件.jpg)



## 堆和栈区别

物理地址：堆的物理地址分配的对象是不连续的，所以性能慢。栈使用的是数据结构中的栈，有先进后出原则，物理地址是连续的，所以性能快。

内存：堆因为不连续，所以分配的内存是在运行期确认的，因此大小不固定。栈因为连续的，所以分配内存大小要在编译期间确认的，大小是固定的。

存放内容：堆存放的是对象的实例和数组，更关注的是数据的存储。栈存放的是局部对象，操作数栈，返回结果，更关注的是程序方法的执行



## JVisualVM 简介

https://blog.csdn.net/gw5205566/article/details/105666637

https://docs.oracle.com/javase/8/docs/technotes/guides/visualvm/index.html



## gc

![Java发编译文件](.\截图\gc回收机制.jpg)

https://www.cnblogs.com/williamjie/p/9222839.html

Stop-The-World机制简称STW，是在执行垃圾收集算法时，[Java](http://www.jb51.net/list/list_207_1.htm)应用程序的其他所有线程都被挂起（除了垃圾收集帮助器之外）。java中一种全局暂停现象，全局停顿，所有Java代码停止，native代码可以执行，但不能与JVM交互；这些现象多半是由于gc引起。

当S0内存占用超过50%时，会直接将对象放进老年代管理，而运行14秒后会导致老年代满了触发full gc。

解决方案：将老年代设置的内存空间降低

![JVM上线设置内存](.\截图\JVM上线设置内存.jpg)

## 垃圾收集器

**年轻代**

Serial 垃圾收集器（单线程，通常用在客户端应用上。因为客户端应用不会频繁创建很多对象，用户也不会感觉出明显的卡顿。相反，它使用的资源更少，也更轻量级。）使用复制算法

ParNew 垃圾收集器（多线程，追求降低用户停顿时间，适合交互式应用。）复制算法

Parallel Scavenge 垃圾收集器（追求 CPU 吞吐量，能够在较短时间内完成指定任务，适合没有交互的后台计算。）复制算法

**老年代**

Serial Old 垃圾收集器 单线程收集器，标记整理算法

Parallel Old垃圾收集器 多线程收集器，标记整理算法

CMS 垃圾收集器（以获取最短 GC 停顿时间为目标的收集器，它在垃圾收集时使得用户线程和 GC 线程能够并发执行，因此在垃圾收集过程中用户也不会感到明显的卡顿。）标记清除算法，运作过程：初始标记，并发标记，重新标记，并发清除，收集结束会产生大量空间碎片

**G1 收集器**：(不会产生空间碎片，可以精确地控制停顿。)  标记整理算法实现，**运作流程主要包括以下：初始标记，并发标记，最终标记，筛选标记**

## G1l垃圾回收器

https://www.oracle.com/cn/technical-resources/articles/java/g1gc.html

Garbage First 简称 G1，是继 CMS 垃圾回收器之后，又一款并发的垃圾回收器，在 JDK7 中被去掉 Experimental 标识，开始可以被正式使用，在 JDK9 中被 JVM 设置为默认的垃圾回收器。G1 是垃圾收集器发展史上的一个新的里程碑，它采用分区算法，基于 Region 的内存布局方式，对整个堆内存进行局部回收，既能回收新生代，也能回收老年代。G1 垃圾回收器的目标是在期望的停顿时间内，尽可能地提高系统的吞吐量。

https://blog.csdn.net/Sqdmn/article/details/107007406?spm=1001.2101.3001.6661.1&utm_medium=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-1.nonecase&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-1.nonecase

### G1收集器参数设置

可选项及默认值																	描述
-XX:+UseG1GC					  						采用 Garbage First (G1) 收集器
-XX:MaxGCPauseMillis=n	设置最大GC 暂停时间。这是一个大概值，JVM 会尽可能的满足此值
-XX:InitiatingHeapOccupancyPercent=n	设置触发标记周期的 Java 堆占用率阈值。默认占用率是整个 Java 堆的 45%。默认值 45.

-XX:NewRatio=n										new/old 年代的大小比例. 默认值 2.
-XX:SurvivorRatio=n								 eden/survivor 空间的大小比例. 默认值 8.
-XX:MaxTenuringThreshold=n	对象晋升年代的最大阀值。默认值 15.这个参数需要注意的是：最大值是15，不要超过这个数啊，要不然会被人笑话。原因为：JVM内部使用 4 bit （1111）来表示这个数。
-XX:ParallelGCThreads=n		设置在垃圾回收器的并行阶段使用的线程数。默认值因与 JVM 运行的平台而不同。
-XX:ConcGCThreads=n				并发垃圾收集器使用的线程数。默认值因与 JVM 运行的平台而不同。
-XX:G1ReservePercent=n			设置作为空闲空间的预留内存百分比以降低晋升失败的可能性。默认值10
-XX:G1HeapRegionSize=n			使用G1，Java堆被划分为大小均匀的区域。这个参数配置各个子区域的大小。此参数的默认值根据堆大小的人工进行确定。最小值为 1Mb 且最大值为 32Mb。
-XX:G1PrintRegionLivenessInfo 默认值false, 在情理阶段的并发标记环节,输出堆中的所有 regions 的活跃度信息
-XX:G1PrintHeapRegions					默认值false, G1 将输出那些 regions 被分配和回收的信息
-XX:+PrintSafepointStatistics								  输出具体的停顿原因
-XX: PrintSafepointStatisticsCount=1					输出具体的停顿原因
-XX:+PrintGCApplicationStoppedTime	             停顿时间输出到GC日志中
-XX:-UseBiasedLocking										   取消偏向锁
-XX:+UseGCLogFileRotation								  开启滚动日志输出，避免内存被浪费
-XX:+PerfDisableSharedMem							   关闭 jstat 性能统计输出特性，使用 jmx 代替
-XX:InitiatingHeapOccupancyPercent设置触发标记周期的 Java 堆占用率阈值。默认占用率是整个 Java 堆的 45%

![2021-11-25_172511](.\截图\2021-11-25_172511.jpg)

## CMS 收集器和 G1 收集器的区别

CMS 收集器是老年代的收集器，可以配合新生代的 Serial 和 ParNew 收集器一起使用； 

G1 收集器收集范围是老年代和新生代，不需要结合其他收集器使用；

CMS 收集器以最小的停顿时间为目标的收集器； 

G1 收集器可预测垃圾回收的停顿时间 

CMS 收集器是使用“标记-清除”算法进行的垃圾回收，容易产生内存碎片 

G1 收集器使用的是“标记-整理”算法，进行了空间整合，降低了内存空间碎片。



## 简述分代垃圾回收器是怎么工作的

分代回收器有两个分区：老生代和新生代，新生代默认的空间占比总空间的 1/3，老生代的默认占比是 2/3。 

新生代使用的是复制算法，新生代里有 3 个分区：Eden、To Survivor、From Survivor，它们的默认占比是 8:1:1，它的执行流程如下： 

​	把 Eden + From Survivor 存活的对象放入 To Survivor 区； 

​	清空 Eden 和 From Survivor 分区； 

​	From Survivor 和 To Survivor 分区交换，From Survivor 变 To Survivor，To Survivor 变 From Survivor。

​	每次在 From Survivor 到 To Survivor 移动时都存活的对象，年龄就 +1， 当年龄到达 15（默认配置是 15）	   	时，升级为老生代。大对象也会直接进入老生代。 

​	老生代当空间占用到达某个值之后就会触发全局垃圾收回，一般使用标记整理的执行算法。以上这些循环往复	就构成了整个分代垃圾回收的整体执行流程。

## 类的加载过程

https://blog.csdn.net/xuemengrui12/article/details/82707473

JVM会通过加载、连接(验证、准备和解析)、初始化三个步骤来对该类进行初始化

类的加载是指把类的.class文件中的数据读入到内存中，通常是创建一个字节数组读入.class文件，然后产生与所加载类对应的Class对象。加载完成后，Class对象还不完整，所以此时的类还不可用。当类被加载后就进入连接阶段，这一阶段包括验证、准备(为静态变量分配内存并设置默认的初始值)和解析(将符号引用替换为直接引用)三个步骤。最后JVM对类进行初始化，包括：

1)如果类存在直接的父类并且这个类还没有被初始化，那么就先初始化父类;

2)如果类中存在初始化语句，就依次执行这些初始化语句。

### 类的加载器

**启动类加载器 **: 它负责将 <JAVA_HOME>/lib路径下的核心类库或-Xbootclasspath参数指定的路径下的jar包加载到内存中，注意必由于虚拟机是按照文件名识别加载jar包的

**扩展类加载器**: 它负责加载<JAVA_HOME>/lib/ext目录下或者由系统变量-Djava.ext.dir指定位路径中的类库，**开发者可以直接使用标准扩展类加载器**

**应用类加载器**: 它负责加载系统类路径`java -classpath`或`-D java.class.path` 指定路径下的类库

**自定义加载器**:

### 双亲委派模型

https://www.jianshu.com/p/9df9d318e838

![img](https://upload-images.jianshu.io/upload_images/5982616-aad63782162c9ae5.png?imageMogr2/auto-orient/strip|imageView2/2/format/webp)

原理：如果一个类加载器收到了类加载请求，它并不会自己先去加载，而是把这个请求委托给父类的加载器去执行，如果父类加载器还存在其父类加载器，则进一步向上委托，依次递归，请求最终将到达顶层的启动类加载器，如果父类加载器可以完成类加载任务，就**成功返回**，倘若父类加载器无法完成此加载任务，**子加载器才会尝试自己去加载**，这就是双亲委派模式



### 父子加载器"之间的关系是继承吗？

https://www.cnblogs.com/hollischuang/p/14260801.html

**双亲委派模型中，类加载器之间的父子关系一般不会以继承（Inheritance）的关系来实现，而是都使用组合（Composition）关系来复用父加载器的代码的。**



### 双亲委派是怎么实现的？

1、先检查类是否已经被加载过 2、若没有加载则调用父加载器的loadClass()方法进行加载 3、若父加载器为空则默认使用启动类加载器作为父加载器。 4、如果父类加载失败，抛出ClassNotFoundException异常后，再调用自己的findClass()方法进行加载

### 为什么Tomcat要破坏双亲委派

**如果采用默认的双亲委派类加载机制，那么是无法加载多个相同的类。**

所以，**Tomcat破坏双亲委派原则，提供隔离的机制，为每个web容器单独提供一个WebAppClassLoader加载器。**