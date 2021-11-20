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