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

clone方法
保护方法，实现对象的浅复制，只有实现了Cloneable接口才可以调用该方法，否则抛出CloneNotSupportedException异常。

getClass方法
final方法，获得运行时类型。

toString方法
该方法用得比较多，一般子类都有覆盖。

finalize方法
该方法用于释放资源。因为无法确定该方法什么时候被调用，很少使用。

equals方法
该方法是非常重要的一个方法。一般equals和==是不一样的，但是在Object中两者是一样的。子类一般都要重写这个方法。

hashCode方法
该方法用于哈希查找，重写了equals方法一般都要重写hashCode方法。这个方法在一些具有哈希功能的Collection中用到。
一般必须满足obj1.equals(obj2)==true。可以推出obj1.hash- Code()==obj2.hashCode()，但是hashCode相等不一定就满足equals。不过为了提高效率，应该尽量使上面两个条件接近等价。

wait方法
wait方法就是使当前线程等待该对象的锁，当前线程必须是该对象的拥有者，也就是具有该对象的锁。wait()方法一直等待，直到获得锁或者被中断。wait(long timeout)设定一个超时间隔，如果在规定时间内没有获得锁就返回。

调用该方法后当前线程进入睡眠状态，直到以下事件发生。

（1）其他线程调用了该对象的notify方法。

（2）其他线程调用了该对象的notifyAll方法。

（3）其他线程调用了interrupt中断该线程。

（4）时间间隔到了。

此时该线程就可以被调度了，如果是被中断的话就抛出一个InterruptedException异常。

notify方法：该方法唤醒在该对象上等待的某个线程。

notifyAll方法：该方法唤醒在该对象上等待的所有线程。

**System类**、**Runtime类**、**Date类**、**SimpleDateFormat类**、**Calendar类**、**Math类**、**Random类**



## 常量

1.8之前在(perm)持久代中，1.8之后在元空间

### 常量池分类

常量池大体可以分为：静态常量池，运行时常量池。

静态常量池 存在于class文件中，比如经常使用的javap -verbose中，常量池总是在最前面把？

运行时常量池呢，就是在class文件被加载进了内存之后，常量池保存在了方法区中，通常说的常量池 值的是运行时常量池。所以呢，讨论的都是运行时常量池

https://blog.csdn.net/qq_41376740/article/details/80338158