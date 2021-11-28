## Jshell

打开CMD 输入命令jshell就可以实现java代码，无需编写一个类

![jdk11-jshell](..\notes\截图\jdk11-jshell.jpg)



## 基类

**Object类**

```java
private static native void registerNatives()
protected native Object clone() throws CloneNotSupportedException
public final native Class <?> getClass()
public boolean equals(Object obj)
public native int hashCode()
public String toString()
public final native void notify()
public final native void notifyAll()
public final native void wait(long timeout) throws InterruptedException
public final void wait(long timeout, int nanos) throws InterruptedException
public final void wait() throws InterruptedException
protected void finalize() throws Throwable
```

**System类**、**Runtime类**、**Date类**、**SimpleDateFormat类**、**Calendar类**、**Math类**、**Random类**



## 常量

1.8之前在(perm)持久代中，1.8之后在元空间

### 常量池分类

常量池大体可以分为：静态常量池，运行时常量池。

静态常量池 存在于class文件中，比如经常使用的javap -verbose中，常量池总是在最前面把？

运行时常量池呢，就是在class文件被加载进了内存之后，常量池保存在了方法区中，通常说的常量池 值的是运行时常量池。所以呢，讨论的都是运行时常量池

https://blog.csdn.net/qq_41376740/article/details/80338158