# 内存泄漏实质 #

内存泄漏实质上是GC时候，被GC Root引用或间接引用着的对象无法被回收，而可以作为GC Root的对象在java中有几种：

1. 虚拟机栈或叫JVM栈（栈帧中的本地变量表）中引用的对象；
> 虚拟机栈是**线程私有的**，每个java方法被执行的时候都会同时创建一个栈帧（Stack Frame）用于**存储局部变量表**、操作栈、动态链接、方法出口等信息。每一个方法被调用直至执行完成的过程，就对应着一个栈帧在虚拟机栈中从入栈到出栈的过程。

2. 方法区中的类静态属性引用的对象；
> 方法区存储**类信息、常量、静态变量**等数据，是线程共享的区域

3. 方法区中常量引用的对象；
> 同上

4. 本地方法栈中JNI（即一般说的Native方法）中引用的对象
> 对应虚拟机栈为虚拟机执行java方法服务，而本地方法栈为虚拟机使用到的Native方法服务


# Android内存泄漏 #
内存泄漏：长周期对象持有短周期对象的引用，而导致短周期的对象在生命周期结束时无法被回收。

即：对象在生命周期结束时被另一个对象通过强引用持有而无法释放



## 永久性内存泄漏： ##
非静态内部类。 如handler、runnable、asnycTask、Thread等，不管是使用匿名内部类的创建模式，还是定义一个非静态的class的模式，都会导致持有外部类使外部类无法释放。（上1）

静态变量，单例模式（上2）

文件操作、数据库操作等closeable类，它们底层是jni实现的（上4），需要手动释放

ps:一般类似onClickListener的用法虽然会使用匿名内部类的方式创建但不会导致内存泄漏，因为只有异步任务的匿名内部类生命周期不同才可能导致内存泄漏，同步的会被作为一个整体被GC掉。

两种特殊情况：
1. 系统监听LocationListener，系统一直持有而永久性泄漏；
2. 监听回调中有耗时操作，导致临时泄漏（见下）。


## 临时性内存泄漏： ##
静态handler有消息存在messageQueue中，由于messageQueue持有message，message持有target（handler对象），handler则会无法回收，直到消息被取出处理后，再次GC时回收。（上1）

