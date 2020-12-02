<!-- TOC -->

- [数据结构](#数据结构)
  - [HashMap](#hashmap)
  - [ConcurrentHashMap](#concurrenthashmap)
  - [Append-only B+ Tree](#append-only-b-tree)
  - [LSM tree](#lsm-tree)
  - [红黑树与 AVL 树](#红黑树与-avl-树)
  - [LinkedBlockingQueue 与 ArrayBlockingQueue](#linkedblockingqueue-与-arrayblockingqueue)
- [Java](#java)
  - [Java 基础](#java-基础)
    - [Reference](#reference)
    - [函数式编程](#函数式编程)
    - [Java 安全](#java-安全)
  - [Java 并发](#java-并发)
    - [volatile](#volatile)
    - [AQS](#aqs)
    - [偏向锁✨](#偏向锁)
  - [Java IO](#java-io)
- [JVM](#jvm)
  - [垃圾收集](#垃圾收集)
    - [CMS](#cms)
    - [G1](#g1)
    - [ZGC](#zgc)
  - [元空间](#元空间)
  - [字节码操作](#字节码操作)
  - [JVM 调优](#jvm-调优)
- [分布式](#分布式)
  - [分布式算法](#分布式算法)
    - [Paxos](#paxos)
    - [Raft](#raft)
    - [BFT](#bft)
  - [分布式锁](#分布式锁)
    - [Redis 分布式锁](#redis-分布式锁)
  - [分布式事务](#分布式事务)
- [MySQL](#mysql)
  - [查询语句](#查询语句)
  - [基本原理](#基本原理)
  - [innodb 存储引擎✨](#innodb-存储引擎)
- [缓存](#缓存)
- [开源框架](#开源框架)
  - [Spring](#spring)
    - [源码解析✨](#源码解析)
  - [Dubbo](#dubbo)
  - [Kafka](#kafka)
    - [底层原理✨](#底层原理)
  - [Zookeeper](#zookeeper)
    - [zab 原理解析](#zab-原理解析)
  - [Tomcat](#tomcat)
  - [Netty](#netty)
    - [内存管理](#内存管理)
    - [底层原理](#底层原理-1)
- [Linux](#linux)
- [计算机网络](#计算机网络)
  - [HTTP](#http)
  - [SOCKET](#socket)
  - [TCP](#tcp)
  - [NIO](#nio)
- [通信协议](#通信协议)
- [解决方案](#解决方案)
  - [负载均衡](#负载均衡)
  - [SSO](#sso)
  - [点赞评论计数设计](#点赞评论计数设计)
- [编程素养](#编程素养)
  - [设计模式](#设计模式)
  - [常用指令](#常用指令)
    - [网络相关](#网络相关)
- [工具包](#工具包)

<!-- /TOC -->

# 数据结构

## HashMap

- [HashMap 源码分析](https://segmentfault.com/a/1190000012926722)

- [JDK1.7 HashMap infinite loop](https://my.oschina.net/u/1024107/blog/758588)

- [HashMap 删除节点时的树退化为链表](https://www.cnblogs.com/lifacheng/p/11032482.html)

## ConcurrentHashMap

- [ConcurrentHashMap 源码分析](https://blog.csdn.net/u011392897/article/details/60479937)

- [ConcurrentHashMap 扩容图文详解](https://blog.csdn.net/ZOKEKAI/article/details/90051567)

    正在迁移的hash桶遇到 get 操作会发生什么？

    答：在扩容过程期间形成的 hn 和 ln 链是使用的类似于复制引用的方式，也就是说 ln 和 hn 链是复制出来的，而非原来的链表迁移过去的，所以原来 hash 桶上的链表并没有受到影响，因此从迁移开始到迁移结束这段时间都是可以正常访问原数组 hash 桶上面的链表，迁移结束后放置上fwd，往后的访问请求就直接转发到扩容后的数组去了。

- [ConcurrentHashMap 方法总结](https://juejin.im/post/5b001639f265da0b8f62d0f8#comment)

- [LongAdder 解析](https://juejin.im/post/6844903909127880717)

## Append-only B+ Tree

- [Append-only B+ Tree](https://blog.csdn.net/lpstudy/article/details/83722007)

    理念类似 MVCC 保留历史版本，类似 copyOnWrite 修改通过新增的方式。

## LSM tree

- [LevelDB 设计与实现 —— LSM tree](https://blog.csdn.net/anderscloud/article/details/7182165)

    通过顺序写 log，内存写 skiplist，延迟更新删除的方式，加速写入的速度。

## 红黑树与 AVL 树

- [红黑树与 AVL 树比较](https://www.zhihu.com/question/19856999/answer/1254240739)

    红黑树类似添加了缓存的 AVL 树。

## LinkedBlockingQueue 与 ArrayBlockingQueue

- [LinkedBlockingQueue 与 ArrayBlockingQueue 源码分析](https://blog.csdn.net/javazejian/article/details/77410889)

    LinkedBlockingQueue 实现中，以 put() 方法为例，生产者线程不停地添加元素，如果添加后容量不满，会唤醒正在等待 notFull 的生产者。只有容量满了之后，生产者会阻塞。之后某一个消费者消费了元素后，检测到队列在消费之前是满的，则会唤醒等待的生产者。
    
    所以，在队列从空向满，生产者添加任务的时候，生产者不会阻塞，当因队列满阻塞时，会有消费者线程帮忙唤醒。

- [为什么ArrayBlockingQueue不使用LinkedBlockingQueue类似的双锁实现?](https://blog.csdn.net/icepigeon314/article/details/93792519)

    首先，ArrayBlockingQueue 可以使用双锁实现，设计者可能认为 Java 并不需要支持两个类似的 BlockingQueue。

# Java

## Java 基础

- [`Class.this` 与 `this` 的区别](https://stackoverflow.com/questions/5666134/what-is-the-difference-between-class-this-and-this-in-java)

- [`toArray()` 与 `toArray(new String[0])`](https://stackoverflow.com/questions/18136437/whats-the-use-of-new-string0-in-toarraynew-string0)

- [`ThreadLocalMap` 概览](https://www.cnblogs.com/xzwblog/p/7227509.html)

    `ThreadLocal` 是 `Thread` 操纵自己的 `ThreadLocalMap` 的工具。

- [反射修改 `static final` 的成员变量](https://www.zhihu.com/question/47054187)

- [非静态内部类中的隐藏变量 `this$0` 指向外部类](https://stackoverflow.com/questions/28462849/what-does-it-mean-if-a-variable-has-the-name-this0-in-intellij-idea-while-deb/28462949)

- [反射方法 `getDeclaredField()` 与 `getField()` 的区别](https://docs.oracle.com/javase/tutorial/reflect/class/classMembers.html)

    父类属性与私有属性不可兼得。

- [反射加载自定义的 `java.lang.System` 类](https://zhuanlan.zhihu.com/p/77366251)

- [Java lambda 表达式内变量须为 final 原因](https://www.zhihu.com/question/28190927/answer/39786939)

- [理解 Java 的线程中断](https://blog.csdn.net/canot/article/details/51087772)

- [Java 自定义序列化](https://www.jianshu.com/p/352fa61e0512)

- [Java 动态代理](https://github.com/skykira/TheirNotes/tree/master/SourceCode/JDK%E5%8A%A8%E6%80%81%E4%BB%A3%E7%90%86)

    - [JDK 自带动态代理源码分析](https://blog.csdn.net/weixin_43217817/article/details/102268504)

    > JDK 操作字节码生成所需要的代理类 `Class`，该代理类继承了 `Proxy` 并实现了我们的接口。Proxy 类实例提供了保存 `InvocationHandler` 自定义逻辑的功能。
    >
    > 代理类中所有的方法（包括类似 `toString()`）都通过我们自定义的 `InvocationHandler` 的 `invoke` 方法来实现，因此单个代理类的方法会被加入同样的逻辑。

    - [CGLib 动态代理](https://dannashen.github.io/2019/05/09/Cglib%E5%8A%A8%E6%80%81%E4%BB%A3%E7%90%86/)

        拦截器中对被拦截方法的调用通过 `proxy.invokeSuper(obj, args);` 完成，相当于子类直接调用父类。

- [检查型异常与非检查型异常](https://blog.csdn.net/u013630349/article/details/50850880)

- [类加载器全盘负责思想的限制](https://blog.csdn.net/byamao1/article/details/62884720)

    > 调用者只会使用它自己的 ClassLoader 来装载别的类

### Reference

- [Java Reference核心原理分析](https://mp.weixin.qq.com/s/8f29ZfGvZVPe0bO-FahokQ)

- [PhantomReference & Cleaner 的运行原理](https://zhuanlan.zhihu.com/p/29454205)

### 函数式编程

- [`BiConsumer` 为什么可以引用仅有一个参数的方法](https://stackoverflow.com/questions/58046693/biconsumer-and-method-reference-of-one-parameter)

### Java 安全

- [如何理解恶意代码执行 `AccessController.doPrivileged()`](https://stackoverflow.com/questions/37962070/malicious-code-running-accesscontroller-doprivileged)

- [java沙箱绕过](https://www.anquanke.com/post/id/151398)

## Java 并发

- [用户态内核态间切换耗时的原因](https://blog.csdn.net/u010727189/article/details/103401970)

- [Java 线程状态转换图](http://mcace.me/java%E5%B9%B6%E5%8F%91/2018/08/24/java-thread-states.html)

- [synchronized的实现原理](https://www.cnblogs.com/longshiyVip/p/5213771.html)

- [深入理解Java线程池：ThreadPoolExecutor](http://www.ideabuffer.cn/2017/04/04/%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3Java%E7%BA%BF%E7%A8%8B%E6%B1%A0%EF%BC%9AThreadPoolExecutor/#addWorker%E6%96%B9%E6%B3%95)

    > *池化思想*用于解决系统资源管理方面的问题，能够降低资源消耗，同时使资源可控。
    >
    > 在并发环境下，系统不能够确定在任意时刻中，有多少任务需要执行，有多少资源需要投入。导致以下问题：
    > 1. 频繁申请销毁调度，给系统带来额外消耗
    > 2. 对资源无限申请情况，缺少抑制手段
    > 3. 系统无法合理管理内部资源分布

    流程：
    未达到 corePoolSize，提交的任务被包装成 Worker，直接运行。否则，提交为任务，等待执行。若是任务队列满了，就添加新的 Worker，到达最大线程容量时，拒绝策略生效。

- [终止线程池原理](https://www.cnblogs.com/trust-freedom/p/6693601.html)

    调用 `shutdown()` 后，会将线程池状态置为 `SHUTDOWN` 并中断所有空闲线程，但正在执行任务的线程不会。
    
    `SHUTDOWN` 状态的线程将不再有新的任务加入，之前执行任务的线程执行完任务后，最终会阻塞在获取任务处，导致线程池无法从 `SHUTDOWN` 状态变为结束状态。

    因此，线程池的设计中，Doug Lea 在所有可能导致线程池产终止的地方安插了 `tryTerminated()`，尝试终止线程池，在其中判断如果线程池已经进入终止流程，没有任务等待执行了，但线程池还有线程，便中断唤醒一个空闲线程。当该线程被唤醒继续运行到退出时，会继续传播中断行为。

- [UncaughtExceptionHandler 解析](https://www.jianshu.com/p/f22efc8ef594)

- [weakCompareAndSet() 解析](https://www.jianshu.com/p/55a66113bc54)

    `weakCompareAndSet()` 底层不会创建任何 happen-before 的保证，也就是不会对 volatile 字段操作的前后加入内存屏障。因此就无法保证多线程操作下对除了 weakCompareAndSet 操作的目标变量(该目标变量一定是一个 volatile 变量)之其他的变量读取和写入数据的正确性。

- [CompletableFuture 的执行模型](https://zhuanlan.zhihu.com/p/88321058)

    Async 后缀代表将任务交给默认或指定线程池，不带 Async 后缀的方法，可能使用上一个方法的线程，也可能使用主线程，二者选一。

### volatile

- [volatile 的底层实现](https://www.zhihu.com/question/65372648/answer/415311977)

    通过 Ringbus + MESI协议的方式，被称为 Cache Locking。

    MESI大致的意思是：若干个CPU核心通过ringbus连到一起。每个核心都维护自己的Cache的状态。如果对于同一份内存数据在多个核里都有cache，则状态都为S（shared）。一旦有一核心改了这个数据（状态变成了M），其他核心就能瞬间通过ringbus感知到这个修改，从而把自己的cache状态变成I（Invalid），并且从标记为M的cache中读过来。同时，这个数据会被原子的写回到主存。最终，cache的状态又会变为S。

### AQS

- [AbstractQueuedSynchronizer 源码解读](https://www.cnblogs.com/micrari/p/6937995.html)

- [CLH、MCS队列锁](https://www.cnblogs.com/sanzao/p/10567529.html)

    自旋锁具有一定的缺陷，非公平、线程饥饿、锁标识同步耗费资源，因此产生了队列锁，对多个自旋锁进行管理。

- setHead()

    队列中的节点，获取到锁后，将 head 指向该节点，同时将 thread、pred、next 置为 null，仿佛就变为了虚拟头结点，仅保留后继结点对该头节点的指向。

### 偏向锁✨

- [偏向锁的批量重偏向与批量撤销](https://www.cnblogs.com/LemonFive/p/11248248.html)

    假设有类 A。

    1. 初始状态的对象 a 为可偏向未偏向状态

    2. 当`线程 1` 对对象 a1 加锁后，a1 对象头 markword 状态偏向于`线程 1`，之后`线程 1`加锁时，发现锁偏向于自己，无需 CAS 替换对象头，可直接进入同步块。
    3. `线程 2`想要对对象 a1 加锁，发现 a1 偏向于 `线程 1`，触发锁撤销，此时 a1 的锁升级为轻量级锁。
    4. `线程 2`在 `BiasedLockingDecayTime`(默认25s) 时间内对类 A 的偏向锁撤销数量达到重偏向阈值(BiasedLockingBulkRebiasThreshold)时，触发批量重偏向，epoch 值加一，相当于将 A 类所有对象的偏向锁失效，重置为可偏向未偏向状态。此时，若有线程对对象 a 加锁，对象将会偏向于该线程。
    5. `线程 x`(或其它任意线程) 对已偏向其它线程的对象 ax 加锁，触发偏向锁撤销，当锁撤销数量达到批量锁撤销阈值(BiasedLockingBulkRevokeThreshold)时，触发批量锁撤销，类 A 新生成对象 markword 初始状态为无锁不可偏向状态。

- `Eliminating Synchronization-Related Atomic Operations with Biased Locking and Bulk Rebiasing` [偏向锁论文阅读](https://www.jianshu.com/p/e47ad923dee5)

    `轻量级锁`的技术假设是，在实际程序中，大多数锁的获取是没有争用的。
    
    `偏向锁`的技术假设是，大多数监视器不仅是没有争用的，而且在它们的生命周期中仅有一个线程进入和退出。

    批量重偏向认为，当偏向锁撤销数量达到某个阈值时，对象当下的偏向已经没有收益了，需要重新选择偏向。批量锁撤销认为，达到该阈值时，该类确实存在竞争，已经不适合使用偏向锁了。

    偏向锁的升级过程：

    1. T1 持有 a1 的偏向锁，T2 获取锁时，发现 a1 偏向 T1。
    2. 查看 a1 markword 中 epoch 值是否与类 A 的 epoch 值一致，不一致则表示 a1 当前的偏向无效，可以直接将 a1 偏向于自己。
    3. 如果一致，开始执行锁撤销。
    4. 当程序运行到全局安全点时，T2 遍历偏向锁持有线程的线程栈，查找与对象 a1 相关的 `Lock Record`。
    
    > 偏向锁获取时，会在当前线程线程栈中置入一个空置的 `Lock Record`，且无需 CAS 填入 `displaced markword`。

    5. 遍历到后，将轻量级锁需要的 markword 填充进去，完成轻量级锁升级。
    6. 如果没有遍历到，则将对象 markword 状态改为可偏向未偏向状态，然后重新 CAS 尝试获取锁。

- [对象计算 hashcode 将导致偏向锁膨胀](https://www.zhihu.com/question/52116998)

    当一个对象当前正处于偏向锁状态，并且需要计算其identity hash code的话，则它的偏向锁会被撤销，并且锁会膨胀为重量锁。

- [Java 中 `volatile` 的语义](https://www.jianshu.com/p/4e59476963b0)

- [Java中的偏向锁、轻量级锁、重量级锁解析](https://blog.csdn.net/lengxiao1993/article/details/81568130?depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-1&utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-1)

- [Monitor 降级](https://wiki.openjdk.java.net/display/HotSpot/Async+Monitor+Deflation#AsyncMonitorDeflation-AnExampleofObjectMonitorInterferenceDetection)

    Monitor 全局存在多个，初始没有，需要的时候，创建；不用时，减少。

- [Java中的锁降级](https://www.zhihu.com/question/63859501)
  - [Java中的锁级降提案✨✨](http://openjdk.java.net/jeps/8183909)

    该优化提案显示，目前，实现重量级锁的 `monitor` 对象可以在STW时被清除，清除的是那些只有 `VM Thread`会去访问它们的 `monitor` 对象（也就是，不再使用的 `monitor` 对象）。

    因此，重量级锁可以降级为轻量级锁。

- [`ReentrantReadWriteLock` 保证 `readHolds` 的可见性](https://stackoverflow.com/questions/1675268/java-instance-variable-visibility-threadlocal)

- [LockSupport.park() 原理](https://blog.csdn.net/anlian523/article/details/106752414)

    如果中断状态为true，那么park无法阻塞。

## Java IO

- [Java I/O体系从原理到应用](https://mp.weixin.qq.com/s/khyOVIqFp1vNK29OIMBBuQ)

- [阻塞非阻塞与同步异步的理解](https://www.zhihu.com/question/19732473/answer/88599695)

  ✨[概念理解](https://www.zhihu.com/question/19732473/answer/871835155): 
  同步异步关注的是，通信双方的协作模式，同步则意味着针对当前业务，双方需要串行进行，异步则可以并行进行。阻塞非阻塞关注的是，调用者的状态，在业务等待期，是阻塞等待还是忙等。

  在不同层次，上述概念有不同的表现形式。
   - 在进程通信层面，同步则意味着阻塞，异步则意味着非阻塞。
   - 在Linux I/O层面，同步可以阻塞，可以非阻塞，异步为非阻塞。
   - 在[网络通信](https://www.zhihu.com/question/19732473/answer/308092103)中，主要是同步阻塞和异步非阻塞。同步意味着，发送方发出请求后等待返回结果，然后发出下一次请求。异步意味着，发送方不必等待结果可发送下一次请求。

  总的来说，[异步是编程语言和调用的API协同模拟出来的一种程序控制流风格](https://www.zhihu.com/question/19732473/answer/103182244)。程序可以在同步 API 上模拟出异步调用(比如多线程)，也可以屏蔽底层的异步接口。

# JVM

- [图说 Java 字节码指令](https://segmentfault.com/a/1190000008606277)

- [Java 编译器优化和运行期优化概览](https://blog.csdn.net/u013305783/article/details/81279175)

- [`Thread-Local Handshakes` 优化安全点STW](https://openjdk.java.net/jeps/312)

    对象的偏向锁撤销操作也可以无需程序运行到 `SafePoint`

- [Java启动参数 `javaagent` 的使用](https://www.cnblogs.com/rickiyang/p/11368932.html)

- [JVM 创建对象时的快速分配与慢速分配](https://developer.aliyun.com/article/724637)

    JVM 创建对象的过程：
    
    1. 尝试在TLAB中分配对象
   
    2. 如果TLAB中没有空间，则可以使用原子指令从eden分配新的TLAB或直接在eden中创建对象
   
    3. 如果伊甸园中没有地方，那么将进行垃圾收集
   
    4. 如果之后没有足够的空间，则尝试在老年代中进行分配
   
    5. 如果失败，那么报 OOM
   
    6. 对象设置标志头，然后调用构造函数

- [缓存行伪共享 (False Sharing)](http://ifeve.com/falsesharing/)

- [理解 TLAB](https://www.jianshu.com/p/2343f2c0ecc4)
- [理解 PLAB](https://blog.csdn.net/superfjj/article/details/107220117)

    线程位于 Survivor 区和 old 区的分配缓冲区。

## 垃圾收集

- [OopMap](https://blog.csdn.net/anyan8023/article/details/74914623?utm_source=blogxgwz3)

    每个方法可能会有好几个oopMap，就是根据 safepoint 把一个方法的代码分成几段，每一段代码一个 oopMap，作用域自然也仅限于这一段代码。

### CMS

- [CMS 垃圾收集过程](https://zhuanlan.zhihu.com/p/54286173)

  - [CMS GC日志分析](https://www.cnblogs.com/zhangxiaoguang/p/5792468.html)

  - [concurrent-preclean 和 concurrent-abortable-preclean 两个阶段的作用](https://zhuanlan.zhihu.com/p/150696908)

    YGC 时，卡表的作用是找到跨带引用。此时，卡表的作用是记录并发标记阶段被应用程序并发修改的对象引用。

    preclean 阶段是对这些 card marking 产生的 dirty card 进行 clean，CMS GC 线程会扫描 dirty card 对应的内存区域，更新之前记录的过时的引用信息，并且去掉 dirty card 的标记。

    concurrent-abortable-preclean 用于调度 remark 的开始时机，防止连续 STW。

- [CMS 垃圾收集过程](https://www.bilibili.com/read/cv6830986/)

    卡表还有一个作用就是发生YGC的时候用来查看有没有老年代的对象引用新生代，这样就不用每次都遍历老年代的对象的。

    card table只有一份，既要用来支持young GC又要用来支持CMS。每次young GC过程中都涉及重置和重新扫描card table，这样是满足了young GC的需求，但却破坏了CMS的需求——CMS需要的信息可能被young GC给重置掉了。

    为了避免丢失信息，就在card table之外另外加了一个bitmap叫做mod-union table。在CMS concurrent marking正在运行的过程中，每当发生一次young GC，当young
    GC要重置card table里的某个记录时，就会更新 [mod-union table](https://blog.csdn.net/mark__zeng/article/details/48751053) 对应的bit。

    这样，最后到CMS remark的时候，当时的card table外加mod-union table就足以记录在并发标记过程中old gen发生的所有引用变化了。

### G1

- [G1 垃圾回收算法原理](https://hllvm-group.iteye.com/group/topic/44381)

    - [G1 垃圾回收算法总览](https://www.jianshu.com/p/a3e6a9de7a5d)

    - [G1 概念理解](https://blog.csdn.net/coderlius/article/details/79272773)

    - [深入理解 G1 的 GC 日志](https://club.perfma.com/article/233563)

    - [G1 SATB和 Incremental Update 算法的区别](https://www.jianshu.com/p/8d37a07277e0)

        SATB 认为标记开始时，所有活着的对象在之后并发标记时也是存活的，因此当白对象断开某个引用时，将该引用压入遍历堆栈，也就是将该白对象变为灰对象。

        Incremental Update 在某个黑对象又引用了某个白对象时，会将该黑对象置灰，因此该算法在完成并发 marking 后 需要重新扫描根集合，重新将灰置黑。

    - [Region 两个 Bitmap 详解 ](https://mp.weixin.qq.com/s/5BIFme6bmyOA0WbKOllbjw)

        Region 的 prevTAMS 和 nextTAMS 用于记录并发标记过程中，新分配的对象，同时存储个快照。

    - [G1-Card Table 和 RSet 的关系](https://blog.csdn.net/luzhensmart/article/details/106052574)

    - [RSet 结构解释](https://zhuanlan.zhihu.com/p/130479811?utm_source=wechat_session&utm_medium=social&utm_oi=70601460940800)

        RSet 是一个 points-into 结构，记录了谁引用了我。
        
        它可以看做一个 Hashtable<key,int[]>。key 为其它 Region，int[] 代表了其它 Region 的 `Card Table`。垃圾收集时，找到 RSet 中指示的区域，作为 GCRoots 的一部分。
    
    ✨私人总结：
    
    G1 垃圾收集分为两个大部分：

    - 全局并发标记（global concurrent marking）
    - 对象拷贝清理（evacuation）

        针对不同的选定CSet的模式，分别对应young GC与mixed GC
      - Young GC：选定所有young gen里的region
      - Mixed GC：选定所有young gen里的region，外加根据global concurrent marking统计得出收集收益高的若干old gen region

    YGC 日志分析
    ```java
    3.378: [GC pause (G1 Evacuation Pause) (young), 0.0015185 secs]
    //并行阶段
   [Parallel Time: 0.7 ms, GC Workers: 4]
   //GC 线程启动
      [GC Worker Start (ms): Min: 3378.1, Avg: 3378.3, Max: 3378.6, Diff: 0.5]
      //此活动对堆外的根进行扫描，如JVM系统目录、VM数据结构、JNI线程句柄、硬件寄存器、全局变量、线程栈
      [Ext Root Scanning (ms): Min: 0.0, Avg: 0.1, Max: 0.2, Diff: 0.2, Sum: 0.6]
      /*Rset 记录了谁指向我，从而避免了全堆扫描。
      Rset 的维护包括两个方面，写前栅栏（Pre-Write Barrrier）和写后栅栏（Post-Write Barrrier）。
      赋值语句后，等式左值修改它的引用到新的对象。左值原来引用的对象失去了一个引用，它所在的区域的 Rset 应当更新。赋值语句后，等式右值获得了左值对它的引用，因此右值所在区域的 Rset 需要更新。
      但处于性能考虑，RSet 不会立刻被被更新，而是后续通过日志来更新。
      写前栅栏还用于实现 SATB，写后栅栏还用于维护 Card Table。
      */
      //并发优化线程更新 RSet
      [Update RS (ms): Min: 0.0, Avg: 0.0, Max: 0.0, Diff: 0.0, Sum: 0.0]
        //并发优化线程（Concurrence Refinement Threads）专注于通过扫描日志缓冲区记录的 dirty card 来更新 RSet
         [Processed Buffers: Min: 0, Avg: 0.2, Max: 1, Diff: 1, Sum: 1]
      //在收集当前 CSet 之前，考虑到分区外的引用，必须扫描 CSet 分区的 RSet。
      [Scan RS (ms): Min: 0.0, Avg: 0.1, Max: 0.1, Diff: 0.1, Sum: 0.3]
      //扫描 JVM 编译后代码（Native Method）的引用信息
      [Code Root Scanning (ms): Min: 0.0, Avg: 0.0, Max: 0.0, Diff: 0.0, Sum: 0.0]
      //对象拷贝，执行CSet 分区存活对象的转移、CSet 分区空间的回收
      [Object Copy (ms): Min: 0.0, Avg: 0.0, Max: 0.0, Diff: 0.0, Sum: 0.1]
      //完成上述任务后，如果任务队列已空，则工作线程会发起终止要求。
      [Termination (ms): Min: 0.0, Avg: 0.2, Max: 0.3, Diff: 0.3, Sum: 0.7]
         [Termination Attempts: Min: 1, Avg: 1.0, Max: 1, Diff: 0, Sum: 4]
      [GC Worker Other (ms): Min: 0.0, Avg: 0.0, Max: 0.0, Diff: 0.0, Sum: 0.1]
      [GC Worker Total (ms): Min: 0.1, Avg: 0.4, Max: 0.6, Diff: 0.6, Sum: 1.8]
      [GC Worker End (ms): Min: 3378.7, Avg: 3378.7, Max: 3378.8, Diff: 0.1]
    //对象拷贝后，引用有所变化。此处，把嵌在代码里的引用修正到evacuate之后新对象的位置
   [Code Root Fixup: 0.0 ms]
   //把不再引用某个region的nmethod从RSet里记录的code roots清除掉的动作
   [Code Root Purge: 0.0 ms]
   //在任意收集周期会扫描 CSet 与 RSet 记录的 PRT(per region table)，扫描时会在全局卡片表中进行标记，防止重复扫描。在收集周期的最后将会清除全局卡片表中的已扫描标志。
   [Clear CT: 0.1 ms]
   [Other: 0.7 ms]
      //主要用于并发标记周期后的年轻代收集、以及混合收集中，在这些收集过程中，由于有老年代候选分区的加入，往往需要对下次收集的范围做出界定；但单纯的年轻代收集中，所有收集的分区都会被收集，不存在选择。
      [Choose CSet: 0.0 ms]
      [Ref Proc: 0.5 ms]
      [Ref Enq: 0.0 ms]
      [Redirty Cards: 0.1 ms]
      [Humongous Register: 0.0 ms]
      [Humongous Reclaim: 0.1 ms]
      [Free CSet: 0.1 ms]
   [Eden: 304.0M(304.0M)->0.0B(304.0M) Survivors: 2048.0K->2048.0K Heap: 304.5M(512.0M)->529.0K(512.0M)]
    [Times: user=0.01 sys=0.00, real=0.00 secs] 
    ```

    Mixed GC 日志分析(过程与 YGC 完全一致，只是范围不一致)

    ```java
    29.268: [GC pause (G1 Evacuation Pause) (mixed), 0.0059011 secs]
   [Parallel Time: 5.6 ms, GC Workers: 4]
      ... ...
   [Code Root Fixup: 0.0 ms]
   [Code Root Purge: 0.0 ms]
   [Clear CT: 0.1 ms]
   [Other: 0.3 ms]
      ... ...
   [Eden: 14.0M(14.0M)->0.0B(156.0M) Survivors: 10.0M->4096.0K Heap: 165.9M(512.0M)->148.7M(512.0M)]
    [Times: user=0.02 sys=0.01, real=0.00 secs] 
    ```

    Concurrent marking cycle 并发标记周期

    ```java
    //这一行日志是全局并发标记的第一个阶段，即初始化标记，是伴随YGC一起发生的，后面的857M->617M表示YGC发生前后堆内存变化，0.0112237表示YGC的耗时
    [GC pause (G1 Evacuation Pause) (young) (initial-mark) 857M->617M(1024M), 0.0112237 secs]
    //开始并发ROOT区域扫描。扫描的 Suvivor 分区也被称为根分区（Root Region）
    [GC concurrent-root-region-scan-start]
    //结束并发ROOT区域扫描，并统计这个阶段的耗时
    [GC concurrent-root-region-scan-end, 0.0000525 secs]
    [GC concurrent-mark-start]
    [GC concurrent-mark-end, 0.0083864 secs]
    //最终标记阶段完成并发标记阶段后遗留的工作，即SATB buffer处理，并统计这个阶段耗时
    [GC remark, 0.0038066 secs]
    //清理阶段会根据所有Region标记信息，计算出每个Region存活对象信息，并且把Region根据GC回收效率排序
    [GC cleanup 680M->680M(1024M), 0.0006165 secs]
    ```
### ZGC

- [ZGC 读屏障过程](https://zhuanlan.zhihu.com/p/43608166)

    通过染色指针技术和读屏障，使得用户线程在读对象时能够总是读取到对象最新的引用。

- [Full GC 时新生代的垃圾收集方式](https://www.zhihu.com/question/62604570/answer/201129934)

    CMS 之前的垃圾收集器并不能单独收集老年代，Full GC 对整个堆使用 `MSC(Mark-Sweep-Compact)` 算法。

- [Major GC 和 Full GC 的区别](https://www.zhihu.com/question/41922036/answer/93079526)

- [ParallelScavenge 的 fullGC](https://www.zhihu.com/question/48780091/answer/113063216)

- [GCLocker-initiated young GC 多余发生](https://www.jianshu.com/p/ecc57a81f73c)

## 元空间

- [metaspace 元空间特征](https://stackoverflow.com/questions/18339707/permgen-elimination-in-jdk-8)

- [metaspace 元空间存储内容](https://www.jianshu.com/p/a6f19189ec62)

- [metaspace 与 DirectByteBuffer 并无关系](https://www.zhihu.com/question/399007267/answer/1260691185)

- [JDK的运行时常量池、字符串常量池、静态常量池储存位置](https://zhuanlan.zhihu.com/p/206852587)['](https://blog.csdn.net/weixin_43232955/article/details/107411378)

    JDK 8 起，字符串常量池和类的静态变量还是存储在堆中。类的类型、成员变量、方法信息，和运行时常量池（包括，JIT 代码缓存和类静态常量池中的数据）存储在元空间中。

## 字节码操作

- [理解字节码增强工具包 Instrumentation ](https://www.throwable.club/2019/06/29/java-understand-instrument-first/)

    包括 `premain` 和 `agentmain` 的使用

- [Java 字节码增强技术](https://tech.meituan.com/2019/09/05/java-bytecode-enhancement.html)

## JVM 调优

- [jmap 指令慎用](https://blog.csdn.net/seeJavaDocs/article/details/53643227)

# 分布式

## 分布式算法

- [拜占庭将军问题和FLP的启示
](https://www.jianshu.com/p/b620cbabf857)

- [如何解决分布式系统中的“幽灵复现”?](https://developer.aliyun.com/article/749236?utm_content=g_1000107462)['](https://zhuanlan.zhihu.com/p/112681511)

    > 从服务端来看“幽灵复现”问题，就是在failover情况下，新的leader不清楚当前的committed index，也就是分不清log entry是committed状态还是未committed状态，所以需要通过一定的日志恢复手段，保证已经提交的日志不会被丢掉（最大 commit 原则），并且通过一个分界线（如MultiPaxos的StartWorking，Raft的noop，Zab的CurrentEpoch）来决定日志将会被commit还是被drop，从而避免模糊不一的状态。

    为什么未提交的 log 需要丢弃？什么时候未提交的 log 需要丢弃？

    间隔了一个任期的未提交的 log 需要必须丢弃。本质上，每当一任 leader 当选后，便会为客户端展示一份一致的视图，此时不存在的 log 不能无端再次出现。

    zab 和 raft 通过在每次选举成功后，持久化当前任期来保证，新的 leader 必须在当前 leader 视图的基础上进行新的提案的读写。

### Paxos

- [Paxos原理、历程及实战](https://mp.weixin.qq.com/s?__biz=MzAwMDU1MTE1OQ==&mid=403582309&idx=1&sn=80c006f4e84a8af35dc8e9654f018ace&scene=1&srcid=0119gtt2MOru0Jz4DHA3Rzqy&key=710a5d99946419d927f6d5cd845dc9a72ff3d652a8e66f0ddf87d91262fd262f61f63660690d2d5da76a44a29e155610&ascene=0&uin=MjA1MDk3Njk1&devicetype=iMac+MacBookPro11%2C4+OSX+OSX+10.11.1+build(15B42)&version=11020201&pass_ticket=bhstP11nRHvorVXvQ4pt9fzB9Vdzj5sSRBe84783gsg%3D)

- [paxos算法中，如果有两个值被Accept了，其中一个形成了多数派，另外一个值怎么处理？](https://www.zhihu.com/question/57947049/answer/155030335)

### Raft

- [Raft 论文翻译](https://github.com/maemual/raft-zh_cn/blob/master/raft-zh_cn.md)

- [Raft 如何处理上一任未提交的的 log](https://zhuanlan.zhihu.com/p/268299972)['](https://zhuanlan.zhihu.com/p/39105353)

    raft 不直接处理上一任未提交的 log，当选举成功后，leader 发送 noop 的一条 log(这条日志里的 term 将是该任期，也就是最新的 term)，按照协议，follower 当前的日志如果不能与 noop 这条日志匹配（index 与 leader 一致才能匹配）的话，则返回拒绝。leader 将会回退 nextIndex，然后重试 AppendEntries 请求，直到 follower 追上进度，接受了 noop 这条log，才会向 leader 返回确认。

    此时，leader 的 noop log 如果收到了大多数 follower 的确认后，leader 节点中上一任的 log 便已经被提交了。

    当新的任期中，有一条该任期的 log entry 被多数 follower 接受后，以前的为提交的 log 才算是真正提交了。

- [Raft 成员变更](https://zhuanlan.zhihu.com/p/32052223)

- [客户端只读请求的处理](https://zhuanlan.zhihu.com/p/36592467)

- [raft 测验](https://ongardie.net/static/raft/userstudy/quizzes.html)

### BFT

- [PBFT 算法各阶段消息发送数量证明](https://zhuanlan.zhihu.com/p/53897982)

## 分布式锁

### Redis 分布式锁

- [基于Redis的分布式锁到底安全吗?](https://www.jianshu.com/p/dd66bdd18a56)

- [Redission 实现](https://www.jianshu.com/p/f302aa345ca8)

    为了防止用户加锁后，解锁失败，导致其余客户端无法继续获取锁，所以需要给分布式锁一个过期时限，保证解锁失败后，锁能过期自动释放。
    
    这是一种典型的[租约机制](https://zhuanlan.zhihu.com/p/101913195)。

    > 锁的失效时间设置是个纠结的问题。当用户并不知道自己将会用多久的锁时，我们为该锁设置一个较小的租期，同时每隔一段时间，在该锁过期之前，自动续租。用户获得锁后，可以启动一个后台线程，周期性查询用户是否还持有锁，持有则续期。

## 分布式事务

- [二阶段提交协议](https://mp.weixin.qq.com/s/Ixurn9kUBhnVZjrWxVpBTg)

    2PC 存在单点故障，当协调者在提交阶段开始时宕机，参与者将被无限期阻塞。

    为了解决这个同步阻塞的问题，3PC 在参与者中也引入了超时机制——超时未收到 doCommit 消息，便自动提交。如此定会有一致性问题出现，所以 3PC 又多加了一个 canCommit 阶段，当改阶段确认通过后，后续失败的概率会降低，以此通过牺牲一定的一致性，提高了可用性。

    2PC 这个分布式事务协议通常是在 DB 层面实现，TCC 则相当于应用层面的 2PC，通过业务逻辑实现，能够允许程序自定义数据库操作粒度。

# MySQL 

[数据库三范式与反范式](https://zhuanlan.zhihu.com/p/77771583)

- 第一范式：属性不可再分

    如若某列包含两个属性，类似商品和价格，那么针对价格进行查询时，其中某一个属性与其它属性进行联合索引时，都是不够简洁明晰的。

- 第二范式：消除部分依赖

    (学号, 课程名称) → (姓名, 年龄, 成绩, 学分)

    以👆为例，学号和课程，能够唯一定位到学生成绩。姓名仅依赖学号，学分仅依赖课程，本来一条(课程)->(学分)就能够代表的关系，现在却冗余了多份。

- 第三范式：消除传递依赖

    (学号) → (姓名, 年龄, 所在学院, 学院地点, 学院电话)

    以👆为例，学号决定学生的学院，学院决定学院电话，本来一条(学院)->(学院电话)就能表示的关系，现在冗余了多份。

满足三范式的数据库设计，能够最大程度的保证数据库数据简洁无冗余，可以按独立的字段检索查询。

但按照范式设计出来的表，第一，在解决数据冗余的同时，会生成许多表，导致了表数量的复杂性。第二，查询数据的时候，多表查询的时间远远高于单表查询的时间。

具体场景具体分析，反范式的设计认为，一定的冗余，能够提高性能。

## 查询语句

- [INSERT ... ON DUPLICATE KEY UPDATE产生death lock死锁原理](https://blog.csdn.net/pml18710973036/article/details/78452688)

    同时持有了 S 锁和 X 锁。

- [利用 `sum()` 统计列中某个值的数目](https://blog.csdn.net/lavorange/article/details/25004181)

    sum() 统计时可以添加条件

- [MySQL 组内排序: 分组查询每组的前n条记录](https://www.jianshu.com/p/717c4bdad462)

- [optimizer trace 详解](http://blog.itpub.net/28218939/viewspace-2658978/)

- [索引 JSON 字段](https://developer.aliyun.com/article/303208)

    在MySQL 5.7中，支持两种 Generated Column，即 Virtual Generated Column 和 Stored Generated Column，前者只将 Generated Column 保存在数据字典中（表的元数据），并不会将这一列数据持久化到磁盘上；后者会将 Generated Column 持久化到磁盘上，而不是每次读取的时候计算所得。

- [JSON 字段使用场景](https://juejin.cn/post/6844903737308381198#heading-1)

    对于数据结构容易变化，数据表中仅有少部分记录需要的数据。

- [分库分表后，跨库分页查询方案](https://blog.csdn.net/uiuan00/article/details/102716457)

## 基本原理

- [数据库事务中的一致性](https://www.zhihu.com/question/31346392)

- [Innodb 双写防止 `partial page write`](https://blog.csdn.net/jolly10/article/details/79791574)

- [ICP(Index Condition Pushdown) 索引下推](https://database.51cto.com/art/201907/599968.htm)

    针对辅助索引能覆盖到的列，将 where 条件的判断下推到存储引擎层。也就是，覆盖索引中，本来使用不到索引的过滤条件，在回表前提前完成了过滤，即为索引条件下推。

- [Mysql 意向锁](https://blog.csdn.net/zcl_love_wx/article/details/82015281)

    意向锁是表级锁，申请行级锁时，由数据库自动提前申请，保证了行级锁与表级锁的共存。

- [in 和 exist 区别](https://blog.csdn.net/wqc19920906/article/details/79800374)

    in 会缓存表达式中的数据，是在内存中操作，但需要遍历 B 表。
    exist 不会缓存数据，但对于 A 中的每一条数据并不需要遍历 B 表。

    in 子查询可以优化为半连接。例如，物化表时，内连接可自动优化左右连接。而且，总能转换为 exist 语句。

- [channge buffer](https://dev.mysql.com/doc/refman/8.0/en/innodb-change-buffer.html)

    在内存中，`channge buffer` 占用了缓冲池的一部分。在磁盘上，`channge buffer` 是系统表空间的一部分，当数据库服务器关闭时，对辅助索引的更改将存储在其中。

- [LSN](https://www.cnblogs.com/drizzle-xu/p/9713378.html)

    系统最大 LSN > redo 日志中的 > 脏页中的 > checkpoint 中的

- [主从分离，从库同样有读写的负荷为什么会提升性能?](https://zhuanlan.zhihu.com/p/34782828)

- [自增 ID 导致主从不一致](https://blog.csdn.net/Saintyyu/article/details/104330881)

- [水平分库扩展](https://www.cnblogs.com/barrywxx/p/11532122.html)

    成倍扩容，提前双写。

- [分库分表，事务问题解决方案](https://mp.weixin.qq.com/s/lgxP0mzwwrV05aUmYb7hJA)['](https://mp.weixin.qq.com/s/9G-_U6di0wgawJXKeVCM-w)

## innodb 存储引擎✨

- 独立表空间的物理结构

    表空间由区构成，每256个区组成一个组，段是一个逻辑概念，每个区包含64个页。

    每个索引包含两个段，叶子段和非叶子段。段由区的链表构成，由元数据结构 INODE Entry 描述，该结构持有链表的起始节点以及段中位于碎片区的页的定位。

    段的元数据结构 INODE Entry 统一存储于表空间第一个区的第三个页面，该页面类型为 INODE 类型，多个此类页面由链表相连接。

    每个组的第一个区的第一个页中，存储了该组内 256 个区对应的元数据结构 XDES Entry，每个 XDES Entry 中有每个区内 64 个页是否空闲的 bitmap。

    每个索引两个段，索引的根页面的 Page Header 部分的两个 Segment Header 结构存储了指向该索引两个段的指针。另，在系统表空间第一个区的第七个页 Data Directory Header 中，存储了各个索引的元数据信息，其中包含了索引的根页面位置。

    因此，从数据字典的索引表中，找到索引根页面的位置，然后得到索引两个段的 INODE Entry 的位置，然后可以寻得该段内所有区或者零碎页。

- in 子查询转化为 semi join 的实现方法

    1. table pullout 子查询表搜索条件只有主键或者唯一索引列时，因为不会出现重复，可以直接转为内连接

    2. duplicate weedout 对半连接后的结果集，构建临时表，消除重复
    3. LooseScan 如果将子查询表作为驱动表，且能够使用到该表的索引时，可以只取相同索引值第一个，相当于分组，然后进行内连接
    4. Semi-join Materialization 为子查询建立物理表，然后内连接
    5. FirstMatch 外层表的每一条数据，与内层表进行匹配，匹配成功一次，便不再仅需进行匹配

    > 将子查询转换为半连接或是 `Exist` 条件的好处是，可能可以帮助子查询使用到索引。

- [InnoDB recovery过程解析](https://sq.163yun.com/blog/article/172546631668785152)

- [崩溃恢复机制](https://www.zhihu.com/question/287892854/answer/1472244704)

    当mysql重启进入崩溃恢复时,首先利用redo恢复数据库的整体一致性,然后会根据保存binlog是否完整来决定事务重做或者回滚,如果binlog事务本身包含commit,则进行重做,否则回滚.

- [MySQL更新语句是如何执行的](https://zhuanlan.zhihu.com/p/146968292)

- [MVCC 是否解决了幻读](https://segmentfault.com/a/1190000020680168)

# 缓存

- [数据库和缓存双写一致性](https://www.cnblogs.com/rjzheng/p/9041659.html?spm=a2c6h.12873639.0.0.2020fe8dqb3Ls4)

- [缓存方案](https://zhuanlan.zhihu.com/p/79944852)

    热点数据缓存击穿问题，可以使用双缓存（多级缓存）。
    
    并发读取时，第一个线程获取锁成功，负责更新主缓存，后续线程返回副缓存数据。并发写入时，加分布式锁，更新副缓存，更新完毕后删除主缓存。

# 开源框架

## Spring

- [GenericTypeResolver.resolveTypeArguments(Class<?> clazz, Class<?> genericIfc)](https://stackoverflow.com/questions/34271764/generictyperesolver-resolvetypearguments-returns-null)获取继承泛型类的子类的泛型类型

    对于 `AsyncConfigurationSelector` 类
    ```java
    public class AsyncConfigurationSelector extends AdviceModeImportSelector<EnableAsync>{
        ...
        public void someMethod(){
            Class<?> annType = GenericTypeResolver.resolveTypeArgument(this.getClass(), AdviceModeImportSelector.class);
            //annType 类型便是继承的父类的泛型类型
        }
    }
    ```

- [`$$` 与 `<generated>` 代表的含义](https://stackoverflow.com/questions/33605246/what-does-and-generated-means-in-java-stacktrace)

- [RootBeanDefinition与GenericBeanDefinition](https://www.cnblogs.com/chwy/p/13514589.html)

- [ApplicationContext 的继承体系](https://zhuanlan.zhihu.com/p/210268684)

### 源码解析✨

- [`@Configuration` 源码解析](https://mp.weixin.qq.com/s/5UvbeEnZBS7niAJw_f-6pQ) [](https://juejin.im/post/6860387888413343757)

    由 `ConfigurationClassPostProcessor` 完成对配置类的代理操作

    - `postProcessBeanFactory` 方法

        1. 列出 `beanFactory` 中被 `@Configuration` 注释的 `beanDefinition`
        2. 将上面每一个 `beanDefinition` 的 `beanClass` 替换为基于 CGlib 的代理类
        3. 代理类自带两个拦截器
           -  `new BeanMethodInterceptor()`
                
                代理 `beanMethod` 方法，控制 bean 的创建或获取。只有第一次调用调用原方法，后续从 `beanFactory` 中获取 `bean`。用配置类中的 BeanMethod 时，当前执行方法非调用方法时(外层第一层为调用方法)，需要调用beanFactory.getBean()获得。

           -  `new BeanFactoryAwareMethodInterceptor()`

                为代理类注入 `beanFactory` 属性
                
- 项目中声明的 Bean 是如何被注册的？

    > 归根结底，通过项目中声明的配置类。
    > 
    > 从 Spring 的 main 方法开始，通过 @ComponentScan 遍历找到  路径下所有的 class 文件，注册为 BeanDefinition。
    > 
    > 找到其中的 @Configuration 类，解析与其相关的类注册为  BeanDefinition。
    
    主要逻辑依赖 BeanDefinitionRegistry 后置处理器  `ConfigurationClassPostProcessor`。
    
    1. 从 beanDefinitionRegistry 中找出所有配置类，作为候选，等 待处理。
    
        - @Configuration 修饰的类
        - @Component、@ComponentScan、@Import、 @ImportResource 修饰的类或者有 @Bean 方法的类
    
        一波筛选后，此时的候选 BeanDefinition 其实只有  `application`。
    
    2. 构造 BeanNameGenerator，为待会注册的 BeanDefinition 生   成 beanName
    3. 创建配置类 ConfigurationClassParser，用于待会解析扫描出的    所有配置类，并添加到名为 configurationClass 的 map 中。
    4. parser.parse(candidates); //开始解析
       1. 解析内部成员类，递归解析
    
       2. 解析 @PropertySource 注解指示的类
       3. 解析 @ComponentScan 注解路径下的类，生成  BeanDefinition，如果存在被 @ComponentScan 修饰的类，递归 解析
       4. 解析 @Import 注解导入的类
    
            1. 导入类类型为 `ImportSelector.class` 时
    
                1. 类型为 DeferredImportSelector.class 时
    
                    添加到 `deferredImportSelectors`，等待处理
    
                2. 其他，递归处理，直到将需要导入的普通配置类处理完
    
            2. 导入 `ImportBeanDefinitionRegistrar.class` 类型时
                
                导入 `importBeanDefinitionRegistrars`，等待处理
            
            3. 都不是，作为普通配置类被解析
       5. 解析 @ImportResource 注解指示的类，填入   `importedResources` 中
       6. 解析 @Bean 修饰的方法引入的 bean
       7. 解析接口中默认方法可能引入的 bean
       8. 解析父类，返回然后递归解析
       9. 若全部递归完成，返回null，跳出解析过程
       10. `parse()` 方法返回前，处理   `deferredImportSelectors` 中需要延迟注册的    BeanDefinition，因为这些 BeanDefinition 注册有 condition，所以需要在其余的加载完后，再轮到他们
    
    5. this.reader.loadBeanDefinitions(configClasses);
    
       将 configClasses 中的类解析为 BeanDefinition，只有  @ComponentScan 扫描出来的类全部变为了 BeanDefinition，其余的暂时存在于 configClass 中。
    
        1. registerBeanDefinitionForImportedConfigurationClass  (configClass);
    
            将 ImportSelector.class 导入的类解析
    
        2. loadBeanDefinitionsForBeanMethod(beanMethod);
    
            将 BeanMethod 导入的类解析
    
        3. loadBeanDefinitionsFromImportedResources (configClass.getImportedResources());
    
            将上面存入 `importedResources` 中的类解析
    
        4. loadBeanDefinitionsFromRegistrars(configClass.   getImportBeanDefinitionRegistrars());
    
            将上面存入 `importBeanDefinitionRegistrars` 中的    类处理掉，调用 `ImportBeanDefinitionRegistrar` 的   接口方法 registrar.registerBeanDefinitions (metadata, this.registry)
    
    6. loadBeanDefinitions 步骤中如果有新的 beanDefinition 被发现，加入 candidates，递归重新解析

- @Async 源码解析

    1. `@EnableAsync` 引入 `AsyncConfigurationSelector.class`，然后引入 `ProxyAsyncConfiguration.class`，最终引入一个 bean `AsyncAnnotationBeanPostProcessor`。
    2. `AsyncAnnotationBeanPostProcessor`会生成并持有一个切面 `AsyncAnnotationAdvisor`。
    3. 当扩展点 `postProcessAfterInitialization()` 被调用时，判断
       1. 当前 bean 已经是代理对象时，判断切面能否应用 bean 的方法，可以则放入代理的切面中。
       2. 当前 bean 非代理对象时，切面合格，则创建代理，否则返回原始 bean。

- [Bean 创建过程源码解析](https://segmentfault.com/a/1190000022309143#item-3-3)

  - createBean 中的扩展点

    1. 执行 bean 实例化前置处理器, `InstantiationAwareBeanPostProcessor` 的 `postProcessBeforeInstantiation()` 方法
   
    2. bean 实例化
    3. `MergedBeanDefinitionPostProcessor` 的 `postProcessMergedBeanDefinition()` 方法

       - CommonAnnoatationBeanPostProcessor 执行时，首先调用父类 InitDestroyAnnotationBeanPostProcessor 的此方法，它解析和初始化（@PostConstruct）和销毁（@PreDestory）相关的注解并缓存到 lifecycleMetadataCache 中。父类完成后，它本身负责缓存 @Resource 注解的资源

    4. 执行 bean 实例化后置处理器, `InstantiationAwareBeanPostProcessor` 的 `postProcessAfterInstantiation()` 方法
    5. 属性值注入前，进行处理`InstantiationAwareBeanPostProcessor` 的 `postProcessPropertyValues()` 方法
    6. 应用属性值，解析属性值中的 bean 引用, 未加载的去加载
    7. 调用 bean 前置处理器, `BeanPostProcessor` 的 `postProcessBeforeInitialization()`
        - 此时会执行 `@PostConstruct`
    8. bean 初始化，若是 InitializingBean, 调用 `afterPropertiesSet()`
    9.  调用自定义 `init-method`。
    10. 调用 bean 后置处理器, `BeanPostProcessor` 的 `postProcessAfterInitialization()`
        - AOP 可在此时返回新的 bean 实例
    11. 调用 `DestructionAwareBeanPostProcessor` 的 `requiresDestruction` 方法, 判断时候需要注册 bean 销毁逻辑

- [`prepareContext() 方法`](https://www.cnblogs.com/youzhibing/p/9697825.html)

    1. 将 context 中的 environment 替换成 SpringApplication 中创建的 environment
    2. 生成 Application 主类的 BeanDefinition，并注册到 BeanDefinitionMap

- [`AbstractApplicationContext` 的 `refresh()` 方法源码解析](https://blog.csdn.net/f641385712/article/details/88041409)

- [Spring代理创建及 AOP 链式调用过程](https://blog.csdn.net/l6108003/article/details/106577515)

    创建 bean 的过程中，在扩展点 `AbstractAutoProxyCreator.getEarlyBeanReference(）` 或 `AbstractAutoProxyCreator.postProcessAfterInitialization()` 处，获取 `AbstractAutoProxyCreator.wrapIfNecessary()` 方法检测类型为 `Advisor.class` 的 bean 以及被 `@Aspect` 注解注释(主要)的 bean。
    
    将切面 bean 中的增强方法封装为 Advisor，并将切面方法按照固定顺序排序。

    切面 bean 按照 `PriorityOrdered`、`Ordered` 接口或 `@Order` 注解顺序排序。

    选出合格的切面后，创建当前 bean 的代理。

    以 JdkDynamicAopProxy 为例，它作为 InvocationHandler，在调用到它的 invoke() 方法时，它会将上面合格的切面生成为 MethodInterceptor 的调用链，每当代理类方法被调用时，切面方法将被应用。

  - [AOP 切面执行顺序](https://blog.csdn.net/qq_32331073/article/details/80596084?utm_medium=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase)

    对于切面内部的 Advisor 顺序，为了满足 `Around -> Before -> Around -> After -> AfterReturning -> AfterThrowing` 的顺序，在执行时的 Advisor 调用链中，它们的顺序如下图所示，该顺序在为 bean 生成代理时便排好序了。

    ![同一个Aspect内Advisor的顺序
](https://github.com/skykira/TheirNotes/blob/master/source/picture/%E5%90%8C%E4%B8%80%E4%B8%AAAspect%E5%86%85Advisor%E7%9A%84%E9%A1%BA%E5%BA%8F.png?raw=true)

- [Spring 循环依赖](https://blog.csdn.net/f641385712/article/details/92801300)

    循环依赖是因为对象之间循环引用，造成闭环。造成循环依赖情况包括，构造器注入、prototype 模式的属性注入、单例模式的属性注入或方法注入三种。
    
    Spring 能解决单例模式的属性注入造成循环依赖的情况。解决方法是将 bean 实例化后初始化前的中间态暴露出来，然后通过三级缓存保证引用正确。

  - [Spring 解决循环依赖为什么使用三级缓存，而不是二级缓存](https://www.cnblogs.com/grey-wolf/p/13034371.html)

    `getEarlyBeanReference()` 为 `SmartInstantiationAwareBeanPostProcessor` 提供了在 bean 初始化前暴露自己的机会，用于解决初始化 bean 时， `BeanPostProcessor` 最终将 bean 包装为代理类，而之前暴露出去的 bean 是原始类的问题。

  - [@Async 导致循环依赖报错原理](https://blog.csdn.net/f641385712/article/details/92797058)

    以[该循环依赖](https://github.com/skykira/TheirNotes/tree/master/source/SourceCode/%40Async)为例阐述报错流程: 
    
    1. getBean -> doGetBean -> createBean -> doCreateBean 获取 C, 创建原始bean `c`，将 c 加入三级缓存 `singletonFactories` 中

    2. populateBean 填充 C 属性 D
    3. getBean -> doGetBean -> createBean -> doCreateBean 获取 D，创建原始bean `d`，将 d 加入三级缓存 `singletonFactories` 中
    4. populateBean 填充 D 属性 C
    5. doGetBean 时，从三级缓存中获取 c 的早期引用。假设有切面，此时经过后置处理器 `AbstractAutoProxyCreator` 的 `getEarlyBeanReference()` 生成 `c` 的代理类对象（一号），@Transactional 注解生成代理也是通过插入切面来完成的，不会额外创建代理。代理类 `c`（一号） 赋给了 `d`
    6. initializeBean 初始化 `d`，有切面，生成代理类 `d`。然后经过 @Async 的后置处理器时，@Async 的后置处理器做了判断，如果传入的是代理类，则直接将增强添加到当前代理中，不会重新创建新的代理类。最终 d 初始化完成，成为代理类 `d`。`d` 没有暴露早期引用，无需进行循环依赖检查。
    7. 代理类 `d` 赋值给了 `c`
    8. initializeBean 初始化 `c`，因为暴露早期引用时已经进行过切面代理，AbstractAutoProxyCreator 缓存中已经有了记录，wrapIfNecessary() 便不再进行代理，然后就返回了原始 `c`！然后经过 @Async 的后置处理器时，因为 @Async 并没有实现早期引用逻辑，此时需要对原始 `c` 进行代理，生成代理类 `c`(二号)。

        Spring 的原则是，早期暴露过了，此时后置处理时就不再动它了。

    9.  初始化完成后，因为 `c` 的早期引用暴露出去了，因此需要循环依赖检查。发现 `d` 依赖 `c`，且位于 `alreadyCreated` 中，说明创建过，得到过 `c` 的早期引用(一号)，因此单例 `c` 出现了不同的版本在不同的引用中，报错。
    
    - [@Lazy 解除循环依赖](https://www.cnblogs.com/yangxiaohui227/p/10523025.html)

- [@RequestMapping 映射关系]()

    `RequestMappingHandlerMapping` 的 `afterPropertiesSet()` 开启映射关系的处理。`isHandler()` 方法判断，只有标有 @Controller 或 @RequestMapping 的 bean 才会被扫描。

- [`AbstractAdvisorAutoProxyCreator` 决定是否要对当前 bean 进行代理](https://www.cnblogs.com/zcmzex/p/8822509.html)

    > spring 依赖注入时，什么时候会创建代理类，什么时候是普通 bean？

- [Spring TargetSource](https://blog.csdn.net/shenchaohao12321/article/details/85538163)

    代理类 JdkDynamicAopProxy 的 AdvisedSupport 持有代理目标相关的信息。
    
    在 AOP 链式调用开始执行时，通过 `targetSource.getTarget()` 获得真正的目标对象 target。通过这种机制使得方法调用变得灵活,可以扩展出很多高级功能,如:target pool(目标对象池)、hot swap(运行时目标对象热替换)。

## Dubbo

- [接口自适应类 T$Adaptive 查看](https://blog.csdn.net/swordyijianpku/article/details/105737163?utm_medium=distribute.pc_relevant.none-task-blog-baidujs-2)

    根据url中的参数，在运行时加载 SPI 扩展类。

    SPI的实现类分为三类，1. 自适应实现类，2. 包裹类，3. 普通实现类，在 ExtensionLoader的getExtension(name) 时，框架会自动将包裹类包在普通实现类外部。例如，ProtocolFilterWrapper 具有生成 invoker 调用链的能力，生成 Extension 时，它自动包裹住普通 Extension，当调用 export() 方法导出时，便能够调用到它的增强方法。

- [接口 Wrapper 类查看](https://www.jianshu.com/p/57d53ff17062)

    可以通过生成的 Wrapper 子类，调用接口实现类的方法，相当于反射调用的另一种实现。

- ReferenceAnnotationBeanPostProcessor 继承 AnnotationInjectedBeanPostProcessor<Reference> 完成 Reference 属性注入。

- ServiceAnnotationBeanPostProcessor 继承 BeanDefinitionRegistryPostProcessor 完成 @Service(指 com.alibaba.dubbo.config.annotation.Service) 注释类的扫描，并将构建对应的 ServiceBean 的 BeanDefinition 注入到 Spring 容器中，当实例化 bean 时，将对 ServiceBean 进行属性注入（比如，ref 属性）。

- [dubbo Filter 之 ContextFilter](https://blog.csdn.net/yuanshangshenghuo/article/details/107722549)

    通过 ContextFilter 对 Invocation 中的 attachments 与 RpcContext 中的 LOCAL/SERVER_LOCAL 进行消息传递。

- [本地存根和本地伪装](http://dubbo.apache.org/zh-cn/blog/dubbo-stub-mock.html#fn3)

- Dubbo [初始化过程](https://www.dazhuanlan.com/2019/12/08/5dec9c3fb6b49/)

    `DubboAutoConfiguration$MultipleDubboConfigConfiguration` 的注解 `@EnableDubboConfig` 引入了 [DubboConfigConfigurationRegistrar.class](https://www.dazhuanlan.com/2020/02/09/5e3f67a09f421/)，又导致引入了 `DubboConfigConfiguration.Single.class`，它们的注解 `@EnableDubboConfigBindings` 引入了 `DubboConfigBindingsRegistrar.class` 以处理 `DubboConfigConfiguration.Single.class` 的注解值，即 `@EnableDubboConfigBindings` 的值（配置类元素数组），处理过程中，为每一个dubbo 配置类生成（空的） BeanDefinition 和对应的 com.alibaba.dubbo.config.spring.beans.factory.annotation.DubboConfigBindingBeanPostProcessor#0，该 DubboConfigBindingBeanPostProcessor 内部持有对应的 beanDefinition 的名称。

    身为 BeanDefinitionRegistryPostProcessor 的 ServiceAnnotationBeanPostProcessor 的 postProcessBeanDefinitionRegistry() 找到所有带有 @Service 注释的类，并将它注册为类型为 ServiceBean 的 beanDefinition，其中记录了对真正的 @Service 服务提供者类的引用。

    当 bean 实例化时，DubboConfigBindingBeanPostProcessor 的 postProcessBeforeInitialization() 方法匹配到对应的 beanName，然后对 Dubbo 的 AbstractConfig 各个配置类进行对应的赋值，根据属性前缀与 application.properties 中的属性进行绑定。

    populateBean() 时，AbstractAutowireCapableBeanFactory 的 applyPropertyValues() 方法将 serviceBean 中的属性（例如：ref）由 RuntimeReference 转换为真正的代理类。之后，ServiceBean 作为 InitializingBean，在执行 afterPropertiesSet() 方法时，会对内部的其他属性，如：provider、protocol、module等进行赋值，通过在 beanFactory 中根据类型找到对应的 bean。

    所有属性设置完毕，待会可以导出服务。

- Dubbo Reference 装配过程

    在 bean 实例化过程中，bean 刚实例化，在 populateBean() 之前，执行 applyMergedBeanDefinitionPostProcessors() 时，会对每个 bean 应用 MergedBeanDefinitionPostProcessor 的 postProcessMergedBeanDefinition() 方法，其中 ReferenceAnnotationBeanPostProcessor 会在方法中执行 findInjectionMetadata() 找到调用 InstantiationAwareBeanPostProcessor 的 postProcessPropertyValues() 方法，其中 AnnotationInjectedBeanPostProcessor 的 postProcessPropertyValues() 内部的 findInjectionMetadata() 方法对每个类找到其中带有 @Reference 注释的属性，包装为 dubbo 的 AnnotationInjectedBeanPostProcessor.AnnotatedInjectionMetadata 类型并缓存起来。

    populateBean() 时，AnnotationInjectedBeanPostProcessor(实际为 ReferenceAnnotationBeanPostProcessor，属性 annotationType 为 @Reference) 作为 InstantiationAwareBeanPostProcessor，被调用其 postProcessPropertyValues() 方法（针对spring中，该方法在spring5.1以后被替换为 postProcessProperties()），该方法从缓存中得到当前 bean 的需要被注入的属性，执行 AnnotationInjectedBeanPostProcessor 中 AnnotatedFieldElement 的 inject() 方法，getInjectedObject() -> ReferenceAnnotationBeanPostProcessor.doGetInjectedBean() -> buildReferenceBeanIfAbsent(referencedBeanName|ReferenceBean 名称, reference|注释, injectedType|属性类型, getClassLoader()) 得到 ReferenceBean，并将 beanFactory 中的 dubbo 配置类设置到其中。最后，将该 ReferenceBean 生成 InvocationHandler，如果该服务为远端服务，则在此时进行初始化，设置 ReferenceBean 的 ref 属性，然后创建jdk动态代理，注入成功。

- DubboProtocol refer() 为 DubboInvoker 构建的 ExchangeClient

    - DubboProtocol
    1. Protocol.refer() 生成 Invoker，DubboProtocol 生成 DubboInvoker
    2. 开始为 DubboInvoker 构建 ExchangeClient
    3. initClient() 中 Exchangers.connect(url, requestHandler) 完成初始化 client，完成后在外面包裹 `ReferenceCountExchangeClient`
        - requestHandler 为匿名内部类`ExchangeHandlerAdapter`，真正处理 `ExchangeChannel channel, Object message` 
    - Exchangers
    1. connect() 方法委托给 Exchanger 的实现类 HeaderExchanger
    - HeaderExchanger
    1. HeaderExchanger 自然是创建 HeaderExchangerClient(Client client, boolean needHeartbeat)，传入的 NettyClient 被包装为 HeaderExchangeChannel
    - Transporters
    1. Transporters.connect(url, new `DecodeHandler`(new `HeaderExchangeHandler`(handler)))，connect() 方法委托给 Transporter 的实现类，NettyTransporter
    - NettyTransporter
    1. new NettyClient(url, listener) 创建基于 Netty 的客户端。
    - NettyClient
    1. super(url, wrapChannelHandler(url, handler));对 handler 进行最后的包裹。
    2. ChannelHandlers.wrap(handler, url);
    - ChannelHandlers
    1. new `MultiMessageHandler`(new `HeartbeatHandler`(ExtensionLoader.getExtensionLoader(Dispatcher.class).getAdaptiveExtension().dispatch(handler, url)));
    - Dispatcher 对应于 Dubbo 的线程模型
    1. Dispatcher 不同的实现类，对应不同的 handler。以 AllDispatcher 为例，创建 `AllChannelHandler`(handler, url)

    所以说，[ChannelHandler](https://blog.csdn.net/yuanshangshenghuo/article/details/108631503) 最外层是 MultiMessageHandler，最内层是 requestHandler，真正处理 channel 数据。发送接收数据，从外到内。

## Kafka

- [Kafka 的 push 与 pull 设计](https://blog.csdn.net/my_momo_csdn/article/details/93921625?utm_medium=distribute.pc_relevant.none-task-blog-baidulandingword-1&spm=1001.2101.3001.4242)

### 底层原理✨

- [kafka 时间轮设计](https://blog.lovezhy.cc/2020/01/11/Kafka%E6%8C%87%E5%8D%97-%E6%97%B6%E9%97%B4%E8%BD%AE%E5%AE%9E%E7%8E%B0/)

    **个人见解**，kafka 时间轮通过 hash 使添加任务的时间复杂度降到 o(1)，同时通过刻度分组，使得延迟队列 delayQueue 的添加速度增快。

    每个 timeWheel 持有自己的上一级时间轮。

    每个 timeTask 持有自己的绝对时间，放入不同的 bucket 是根据绝对时间来的，所以无论 currentTime 是否推进都不影响 timeTask 添加到时间轮中。然后，被放入了 timeTask 的 bucket 的过期时间也会更新（同样根据任务的绝对时间来更新的）

    delayQueue 中没有轮的概念，仅有持有绝对过期时间的 bucket，如果上个轮次的某个 bucket 被遍历出来，后来这个 bucket 的绝对过期时间更新了，就要重新添加进去。

    currentTime 仅在有任务出 delayQueue 时被推进，否则不变。这不会导致问题，因为 delayQueue 中的 bucket 持有绝对过期时间。

    currentTime，其实，唯二的作用就是锚定多个时间轮，使得层级时间轮之间时间相对一致，否则 timeTask 添加到上级时间轮中时，可能会被执行掉。另一个就是下面这一段所说的，用于执行任务。

    对于最低一级时间轮来说，currentTime 的精确度完全不所谓，虽然它现在已经是无所谓的。执行任务前，它自然会被推进。

    顺带一提，timeTask 的执行是通过重新插入 timeTask 的方法来顺便执行的（这样也同时兼容了时间轮的降级操作）。delayQueue 中的 bucket 时间到了后，先推进 currentTime，然后添加 timeTask，该执行的执行。

- Kafka 客户端原理

    客户端 NetworkClient 产生的消息分区后，发往各分区对应的 RecordAccumulator，多条消息在收集器中累积成为一条批记录，如果分区内的第一条批记录满了，则开始发送这条批记录。发送时，将不同分区的多条批记录按照服务端节点分组，构造 ClientRequest，然后发往对应节点。
    
    客户端的请求发送和响应接收通过 Selector 模式进行。Selector 轮询得到可读或可写事件时，通过 KafkaChannel 将处理逻辑通过传输层交给 SocketChannel 进行实际发送。

    KafkaChannel 持有 Send 对象 和 NetworkReceive。Send 和 NetworkReceive 都用字节缓冲区来缓存通道中的数据，Send 包含一个缓冲区，NetworkReceive 包含两个缓冲区，一个普通缓冲区，还有一个 Size 缓冲区。实例化时，为 size 缓冲区分配  4 字节，当 size 缓冲区读满后，通过 size.getInt() 得到 NetworkReceive 普通缓冲区的大小，分配内存，并开始接收数据。

- [Group coordinator & Group leader 与分区重分配](https://www.jianshu.com/p/a8461707d6ea)

    关于 consumers 的维护，有两个重要的概念：group coordinator & group leader:
    
    1. group coordinator: 是特殊的 broker。consumers poll 消息 & commit 消费消息记录时，会发送 heartbeats 到 group coordinator 来同时告知自己的健康状况。
    
    2. group leader: 第一个加入 consumer group 的 consumer 就是该 group 的 group leader。group coordinator 会把 consumers 列表发给 group leader 来维护。group leader 负责 assign & reassign partitions。
    
    如果 consumer crashed／network failure，长时间没有发送 heartbeats 到 group coordinator 时，group coordinator 会认为该 consumer 失联，并通知 group leader rebalance partition，group leader 将 rebalance 的结果通知 group coordinator，由 coordinator 来通知 consumers 新的 partitions 关系。

- [再均衡过程](https://www.zhihu.com/question/63892760/answer/214411241)

    consumer caller thread 只有当 poll() 的时候才会知道到底 rebalance 有没有被 trigger，heartbeat thread 当收到 heartbeat error code 的时候只会 set 一个 flag，这个 flag 只有 caller thread 触发 poll() 的时候才会查看并发送joinRequest。

    在 poll() 里面我们是用一个 do-while on pollOnce()，每次都会看一下那个 flag，所以会得知

    再均衡步骤：
    
    1. 第一阶段(FIND_COORDINATOR)

        消费者找到本消费组对应的 GroupCoordinator 所在的 broker，并建立连接。查找时，会向集群中负载最小的节点发送 FindCoordinatorRequest 请求。

    2. 第二阶段(JOIN GROUP)

        消费者会向 GroupCoordinator 发送 JoinGroupRequest请求。请求中的 protocol_metadata 字段由 PartitionAssignor接口的 `subscription()` 方法得到。

        如果是原消费者重新加入消费者组，发送请求前还会触发 ConsumerRebalanceListener 的 `onPartitionsRevoked()` 方法。

        GroupCoordinator 收到请求后，为消费者组内的消费者选出一个 leader，并为消费者组选定一个分区分配策略。

        然后，GroupCoordinator 返回 JoinGroupResponse。但，members 字段只有 leader 有数据。

    3. 第三阶段( SYNC GROUP)

        消费者组 leader 根据 GroupCoordinator 返回的分区策略完成具体的分区分配后，向 GroupCoordinator 发送 SyncGroupRequest 请求。所有消费者都会发送该请求，GroupCoordinator 将消费者组 leader 发送来的分配方案和消费者组的元数据一起存入 _consumer_offsets 主题中，然后将响应发送给各个消费者。

        当消费者收到所属的分配方案之后会调用 PartitionAssignor 中的 `onAssignment()`方法。随后再调用 ConsumerRebalanceListener 中的 `OnPartitionAssigned()` 方法。之后开启心跳任务，消费者定期向服务端的 GroupCoordinator 发送 HeartbeatRequest 来确定彼此在线。

    4. 第四阶段( HEARTBEAT)

        正式开始消费前，消费者通过 OffsetFetchRequest 请求获取上次提交的消费位移并从此处继续消费。消费者通过向 GroupCoordinator 发送心跳来维持它们与消费组的从属关系，以及它们对分区的所有权关系。

## Zookeeper

- [zookeeper 如何保证事务按顺序生效？](https://time.geekbang.org/column/article/239261)

- [zookeeper 选举过程保证一致性](https://www.jianshu.com/p/f30ae8e75d6d) 
  
    > 关于 zookeeper 的[问题一](https://segmentfault.com/q/1010000023814986)、[问题二](https://www.zhihu.com/question/324291664)?

    当客户端连接的节点崩溃后，[客户端超时会进行重连](http://www.caotama.com/29507.html)，不会出现事务返回失败，但最终又成功的情况。

- [zookeeper 不稳定解决方案](https://zhuanlan.zhihu.com/p/25594630)

- [zookeeper 分区后的行为](https://cwiki.apache.org/confluence/display/ZOOKEEPER/FailureScenarios)

- [zxid 溢出处理](https://segmentfault.com/q/1010000022201186)

- [Zookeeper 崩溃选举之后的数据同步过程](https://blog.csdn.net/qq_41775852/article/details/104947943)

- [zookeeper sessionMovedException 处理过程](https://blog.csdn.net/trntaken/article/details/108808165)

- [fast leader election](https://mp.weixin.qq.com/s/bS_Se3UnEVGKJgILLhhiTA)['](https://yq.aliyun.com/articles/298075)

    zab 协议四个阶段 —— 选举，发现，同步，广播。
    
    fle 三个阶段 —— 选举，恢复，广播。

### zab 原理解析

1. zookeeper 崩溃后，加载磁盘中的快照和事务日志，但事务日志中包含已经commit和未commit的proposal，这样岂不是应用了未经过仲裁的proposal？

- [官网文档 ZooKeeper Internals](https://zookeeper.apache.org/doc/r3.3.5/zookeeperInternals.html)

    > any uncommited proposals from a previous epoch seen by a new leader will be committed by that leader before it becomes active.

    新当选 leader 会提交上一任的未提交 proposal。如果提案p1被发出后，未被提交，leader 崩溃，重新开始选举，新的 leader 如果因为崩溃等原因，没有P1，则 P1 提案被返回失败。如果新的leader有P1，则P1成功。

    > ZooKeeper messaging consists of two phases：Leader Activation and Active messaging。

    选举后，leader要开始消息广播，需要两个步骤。第一，leader 激活。leader 当选需要两个条件，拥有最大 zxid 同时被法定任务的服务器承诺跟随。第一点是硬性条件，但第二点只要有极高的把握即可，成为准 leader 后，leader activation 会对该leader进行二次检查。

## Tomcat

- [Tomcat 架构解析](https://mp.weixin.qq.com/s/fU5Jj9tQvNTjRiT9grm6RA)

- [Tomcat 处理请求过程源码解析](https://blog.csdn.net/leileibest_437147623/article/details/85287568?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase)

## Netty

> Reactor 模式
> 
> Reactor模式是一个事件驱动，用于一种处理一个或多个客户端并发进行服务请求的设计模式。
> 
> 它将服务端接收请求与事件处理分离，提高了系统处理并发的能力，`java NIO 的 reactor 模式是基于系统内核的多路复用技术实现的`。

- [SocketChannel 与 ServerSocketChannel 区别](https://blog.csdn.net/hzmlg1988/article/details/88082492)

- [Netty 启动源码分析](https://mp.weixin.qq.com/mp/appmsgalbum?action=getalbum&album_id=1342147420482011137&__biz=MzI2NzY4MjM1OQ==#wechat_redirect)(猿灯塔|需微信中打开)

- [netty 时间轮设计](https://zacard.net/2016/12/02/netty-hashedwheeltimer/)

- [JDK Epoll 空轮询 bug](https://www.jianshu.com/p/3ec120ca46b2)

### 内存管理

- [Netty 内存管理模型](https://zhuanlan.zhihu.com/p/259819465)

    内存对象池化的主要目的是提高内存的使用率。
    
    池化的简单实现思路，是在 JVM 堆内存上构建一层内存池，通过 allocate 方法获取内存池中的空间，通过 release 方法将空间归还给内存池。
    
    对象的生成和销毁，会大量地调用 allocate 和 release 方法，因此内存池面临碎片空间回收的问题，在频繁申请和释放空间后，内存池需要保证连续的内存空间，用于对象的分配。
    
    基于这个需求，有两种算法用于优化这一块的内存分配：伙伴系统和 slab 系统。
    
    - 伙伴系统，用完全二叉树管理内存区域，左右节点互为伙伴，每个节点代表一个内存块。内存分配将大块内存不断二分，直到找到满足所需的最小内存分片。内存释放会判断释放内存分片的伙伴（左右节点）是否空闲，如果空闲则将左右节点合成更大块内存。例如：ChunkList 的内存管理。

    - slab 系统，主要解决内存碎片问题，将大块内存按照一定内存大小进行等分，形成相等大小的内存片构成的内存集。按照内存申请空间的大小，申请尽量小块内存或者其整数倍的内存，释放内存时，也是将内存分片归还给内存集。例如：TinySubPage 和 SmallSubPage。

- [Netty对零拷贝三个层次的实现](https://zhuanlan.zhihu.com/p/88599349)

### 底层原理

- [Java NIO wakeup实现原理](https://my.oschina.net/7001/blog/1509533)

    Selector的select()方法时，会阻塞。

    pollWrapper(不同 Selector 实现，该结构不一样) 负责保存当前 selector 对象上注册的 FD，当调用 Selector 的 select() 方法时，会将pollWrapper的内存地址传递给内核，由内核负责轮训pollWrapper中的FD，一旦有事件就绪，将事件就绪的FD传递回用户空间，阻塞在select()的线程就会被唤醒。

    以 WindowsSelectorImpl 为例，它在实例化时顺便实例化了一个管道实例 wakeupPipe，这个管道包含两个 wakeupSourceFd 和wakeupSinkFd 两个[文件描述符](https://www.yuque.com/myboy-ipf95/kgi7au/kh8vee)(进程可以通过文件描述符表定位自己已打开的各类输入输出流)。系统将 wakeupSourceFd 作为第一个 FD 加到 pollWrapper 中让系统进行监控，Selector 需要 wakeup() 的时候，就往 wakeupSinkFd 写入一字节的数据，它立即被发送到 wakeupPiepe 的 source 端，wakeupSourceFd 有了数据，Selector 便立即返回了。

- [EpollSelectorImpl 实现原理](https://zhuanlan.zhihu.com/p/159346800)

- [NioEventLoop 源码解析](https://blog.csdn.net/TheLudlows/article/details/82961193)

- [inBound 与 outBound 处理器](https://blog.csdn.net/weixin_41947378/article/details/108059942)['](https://zhuanlan.zhihu.com/p/104232421)

    ChannelHandler 的方法提供具体的业务逻辑，ChannelHandlerContext 负责调用链的进行。ChannelHandlerAdaptor 继承了 ChannelHandler，其方法的默认实现为 直接调用 ChannelHandlerContext 的方法。

    入站事件由远端数据触发，出站事件由用户代码触发。

- [ctx.channel().writeAndFlush()和ctx.writeAndFlush()的区别](https://blog.csdn.net/chehec2010/article/details/90643436)['](https://blog.csdn.net/qq_34827263/article/details/100729528)['](https://www.cnblogs.com/qdhxhz/p/10234908.html)

- [AbstractNioByteChannel 源码分析](https://www.jianshu.com/p/d82fc1c52805)['](https://www.cnblogs.com/brandonli/p/10210592.html)

    对于 AbstractNioByteChannel 和 AbstractNioMessageChannel，这两个类主要实现read和write的框架，它们的实现大致相同AbstractNioByteChannel读写的是byte array，而AbstractNioMessageChannel读的时候会把byte array转换成结构化的对象，写的时候把结构化对象序列化成byte array。
    
    AbstractNioByteChannel定义了3个抽象方法用来实现真正的读写操作: doReadBytes, doWriteBytes, doWriteFileRegion。
    
    AbstractNioMessageChannel第了两个2个抽象方法用来实现真正的结构化数据类型的读写: doReadMessages, doWriteMessage。

- [Netty 服务端运行时]

    NioServerSocketChannel 初始化时设置自己的 pipeline，会将 ServerBootstrapAcceptor 置于倒数第二的位置，之后在 bossGroup 中选择一个 NioEventLoop 完成 register ，然后开始监听。

    每个 NioEventLoop 对象在 NioEventLoopGroup 实例化时被创建，创建时，openSelector() 会为每个 NioEventLoop 创建 selector。

    当 NioServerSocketChannel 对应的 NioEventLoop 接收到 SelectionKey.OP_READ 或 SelectionKey.OP_ACCEPT 信号时，NioMessageUnsafe 的 read() 方法中，doReadMessages 会生成该链接对应的 NioSocketChannel，之后触发 pipeline，执行到 ServerBootstrapAcceptor 时，它会为新生成的 NioSocketChannel 对应的 pipeline 设置 childHandler（也就是 ChannelInitializer），同时将该 NioSocketChannel register 到 workerGroup 的一个 NioEventLoop 对象中，然后该 NioEventLoop 运行时将监听该 Channel。

- 异步串行无锁化

    1. 对于外部线程提交给 IO 线程的任务，以及 Handler 中业务任务的处理交给业务线程池，都需要通过 inEventLoop() 方法进行判断，减少了上下文切换。
    2. MPSC 队列无锁队列提高性能。

# Linux

- Linux IO 模型
  - [简述 Linux IO 模型](https://mp.weixin.qq.com/s/3C7Iv1jof8jitOPL_4c_bQ)
  - [详述 Linux IO 模型](https://www.jianshu.com/p/486b0965c296)

- [多路复用之select、poll、epoll](https://www.wemeng.top/2019/08/22/%E8%81%8A%E8%81%8AIO%E5%A4%9A%E8%B7%AF%E5%A4%8D%E7%94%A8%E4%B9%8Bselect%E3%80%81poll%E3%80%81epoll%E8%AF%A6%E8%A7%A3/)['](https://wenchao.ren/2019/07/Select%E3%80%81Epoll%E3%80%81KQueue%E5%8C%BA%E5%88%AB/)

- [Linux 零拷贝技术](https://mp.weixin.qq.com/s/0SHaQBgMJ4MlKjX6m08EpQ)

  Linux I/O 设备与主存信息传送的控制方式分为程序轮询、中断、DMA等。

  在 Linux 中零拷贝技术主要有 3 个实现思路:

    1. 用户态直接 I/O
    2. 减少数据拷贝次数
    3. 写时复制技术

  以读取磁盘数据，写入网卡为例，实现方式：

    1. 用户态直接 I/O
      
        内核仅进行必要的配置，虽然还有用户内核空间上下文切换的开销，但减少了 CPU 拷贝

    2. mmap() + write
      
        将用户缓冲区的部分区域映射到内核缓冲区，减少了一次内核向用户缓冲的 CPU 拷贝

    3. sendfile

        sendfile 命令之前，读取写入通过 read() 和 write() 命令，存在四次上下文切换以及向用户空间的冗余拷贝。

        Sendfile 调用中 I/O 数据对用户空间是完全不可见的，完全交由内核去进行数据传输。不过，问题是，过程中用户无法对数据进行修改了。

    4. Sendfile + DMA gather copy

        磁盘读入到内核缓冲区的数据，本来需要传输到 Socket 缓冲区，然后才通过 DMA 拷贝到网卡中。

        如今，在硬件的支持下，Sendfile 拷贝方式仅仅是将数据的描述信息，即，缓冲区文件描述符和数据长度的拷贝，拷贝到 Socket 缓冲区中。

        发往网卡时，直接从内核缓冲区中取。又减少了一次 CPU 拷贝。

    5. Splice

        Sendfile 只适用于将数据从文件拷贝到 socket 套接字上，而 Splice 系统调用，不仅不需要硬件支持，还实现了两个文件描述符之间的数据零拷贝。

    6. 写时复制

        内核缓冲区被多个进程共享时，降低了系统开销。

    7. 缓冲区共享

        通过缓冲区池，它能被同时映射到用户空间（user space）和内核态（kernel space），完全省去了拷贝。

- [Linux中的文件描述符与打开文件之间的关系](https://blog.csdn.net/cywosp/article/details/38965239)

- [Linux Pipeline 原理](https://blog.csdn.net/oguro/article/details/53841949)

    匿名管道，内核中一小块缓冲区域。命名管道，会在文件系统中建立，可以用于任意进程之间通信。

- [KeepAlived 原理](https://blog.csdn.net/qq_24336773/article/details/82143367)

# 计算机网络

- 为什么网络同时需要 IP 和 MAC 地址？

    - [历史原因](https://www.zhihu.com/question/21546408/answer/53576595)，TCP/IP（因特网） 完成了对异构网络间的互联互通，但没有定义物理层和数据链路层的具体细节，其需要运行于以太网等其它二层网络之上。

    - [功能原因](https://www.zhihu.com/question/21546408/answer/149670503)，IP 地址与地域相关，它虽然具有唯一标识作用，但不能唯一标识某台主机，MAC 地址才能唯一标识某台主机。如果仅有 IP 地址，主机 A IP 换了以后，发往主机 A 的消息不会发往 A 原来的 IP，而是会发送到 A 的 MAC 地址新对应的 IP 地址。所以，IP 用于路由，但无法永久唯一标识。

## HTTP

- [http 版本特性](http://muyiy.cn/question/network/117.html)

    http1.0: 无状态，无连接
    http1.1: 长连接，管道化请求，缓存控制
    http2.0: 多路复用，头部压缩，server push，二进制分帧

- [当我们在谈论HTTP队头阻塞时，我们在谈论什么](https://blog.csdn.net/uxiad7442kmy1x86dtm3/article/details/79416171)['](https://blog.csdn.net/m0_37145844/article/details/109280985?utm_medium=distribute.pc_aggpage_search_result.none-task-blog-2~all~sobaiduend~default-1-109280985.nonecase&utm_term=tcp%E9%98%9F%E5%A4%B4%E9%98%BB%E5%A1%9E&spm=1000.2123.3001.4430)

   HTTP/2 over TCP 解决了 http request 级别的队头阻塞问题，但应用层协议无法解决 TCP 传输层的对头阻塞问题。

- [TCP/IP, SPDY, WebSocket 三者之间的关系](https://www.zhihu.com/question/20097129/answer/15017791)

## SOCKET

- [Socket 端口复用](https://bbs.csdn.net/topics/390945826)

    一般 TCP 的 SO_REUSEADDR 用于服务器，以便服务器崩溃重启时，可直接 Bind 处于 TIME_WAIT 状态的端口。

    对于 TCP，我们不可能启动捆绑相同 IP 地址和相同端口号的多个服务器。

    端口复用时，只有一个Socket可以得到数据。

- [多个 Socket 监听同一端口](https://blog.51cto.com/ticktick/779866)['](https://stackoverflow.com/questions/3329641/how-do-multiple-clients-connect-simultaneously-to-one-port-say-80-on-a-server)['](https://blog.csdn.net/u011580175/article/details/80306414)

    一个进程可以与多个套接字关联，两个独立的进程不可以侦听同一端口。
    
    服务器可以使用多个子进程/线程为每个套接字提供服务。操作系统（特别是UNIX）在设计上允许子进程从父进程继承所有文件描述符（FD）。因此，只要进程通过父子关系与A相关联，便可以由更多进程A1，A2..监听进程A侦听的所有套接字。
    
## TCP

- [TCP/IP 三次握手思考](https://blog.csdn.net/lengxiao1993/article/details/82771768?utm_medium=distribute.wap_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase&depth_1-utm_source=distribute.wap_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase)

- [TCP/IP 四次挥手](https://blog.csdn.net/ThinkWon/article/details/104903925?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-3.nonecase&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-3.nonecase)

- [TCP 半连接和全连接](http://jm.taobao.org/2017/05/25/525-1/)

    Syn Queue 为半连接队列，等待 server accept；Accept Queue 为全连接队列，存储完成了三次握手的连接。

    调用命令 ss -lnts 可以查看 Socket 连接情况：

    - 当连接处于Listening状态时，Recv-Q表示全连接队列实际使用情况，Send-Q表示全连接队最大容量。
    - 当连接处于非Listening状态时，Recv-Q表示接受缓冲区还没有读取的数据大小，Send-Q表示发送缓冲区还没有被对端ACK的数据大小。

- [TCP_NODELAY选项](https://www.jianshu.com/p/ccafdeda0b95)

    TCP_NODELAY 关闭时，会开启 [Nagle 算法](https://www.cnblogs.com/postw/p/9710772.html)。

    该算法要求一个 tcp 连接上最多只能有一个未被确认的未完成的小分组，在该分组 ack 到达之前不能发送其他的小分组，tcp 需要收集这些少量的分组，并在 ack 到来时以一个分组的方式发送出去。因此会有延迟。

## NIO

- [IO 多路复用要搭配非阻塞 IO](https://www.zhihu.com/question/37271342)

    select() 返回可读是可能可读，read() 时可能会没有数据。

    - [使用epoll时需要将socket设为非阻塞吗？](https://www.zhihu.com/question/23614342/answer/61986309)
    
        以 epoll 为例，当使用阻塞式的 fd，数据从无到有会通知进程来读，但进程不知道有多少数据，当二次循环来读时，若没有数据，则进程被阻塞。

# 通信协议

- [通信协议之序列化](http://blog.chinaunix.net/uid-27105712-id-3266286.html)

- [Dubbo 3.0 前瞻：常用协议对比及 RPC 协议新形态探索](https://mp.weixin.qq.com/s/x0pZcsKkFhcwClJJL3X7AQ)

# 解决方案

## 负载均衡

- [DNS 轮询负载均衡](https://mp.weixin.qq.com/s/4dzqbh2wfzbQzgFodP2_6Q)

- [lvs 会接受客户端的请求，为什么说 lvs 没有流量产生？](https://www.zhihu.com/question/36868056/answer/151700242)

    对于 LVS 三种代理方式中的两种 —— IP Tunnelling 和 Direct Routing，客户端请求的报文每次会经过 lvs 转发到 realserver，但 realserver 回复的报文不会经过lvs。

- [长连接与负载均衡](https://mp.weixin.qq.com/s/LKGzc6XBAWYdskVQQJFLHw)

## SSO

- OAuth 2.0 授权码登录过程

  1. filter 拦截 request，向浏览器返回重定向，浏览器重定向到 SSO 服务器 `ssoServerUrl + "/jump" + "?redirectUrl={0}&loginType={1}&needSaveSsoToken=false"`。

  2. SSO 服务器返回重定向地址 `getSsoServerUrl() + Constant.LOGIN_URL + "?redirectUrl=" + requestUrl + "&loginType=" + loginType`，重定向到 SSO 登录页面。

  3. 登录时，携带回调地址。登录成功，微信服务器回调接口，返回 token 信息。

  4. SSO 服务器解析得到 token，保存 token 信息，然后重定向到请求的初始地址，并携带 token 信息。

- [单点登录](https://juejin.im/entry/6844903782996770824#comment)['](https://zhuanlan.zhihu.com/p/25007591)

- [JWT 的优缺点](https://www.cnblogs.com/nangec/p/12687258.html)

## 点赞评论计数设计

- [微博计数器的设计](https://blog.cydu.net/weidesign/2012/12/06/weibo-counter-service-design-for-velocity/)

    1. 数据存储结构优化
    2. 数据存储压缩
    3. 冷热数据分离
    4. 批量查询
    5. 消息度列去重

# 编程素养

- [正则表达式的环视](https://blog.csdn.net/lxcnn/article/details/4304754)

    > `str.replaceFirst("(?<=.{5}).+", "...")`
    >
    > 保留 `str` 的前五位字符，其余字符用 `...` 代替
    
    - [正则表达式参考文档](http://notes.tanchuanqi.com/tools/regex.html)

- 将 javassist 动态生成的类打印出来

    `(ClassGenerator)ccp.getClassPool().get("com.alibaba.dubbo.common.bytecode.Proxy0").debugWriteFile()`

## 设计模式

- 简单工厂模式、工厂方法模式、抽象工厂模式

    1. 简单工厂封装了创建对象的过程，调用就生产产品 A；
    2. 工厂方法模式封装了多个简单工厂，可以根据入参，生产不同的产品 A、B；
    3. 抽象工厂封装了工厂方法，可以根据入参，返回不同的工厂。

## 常用指令

### 网络相关

 - [netstat](https://www.cnblogs.com/faberbeta/p/12981880.html)

    查看 pid 与 port 的关系

# 工具包

- [原码补码工具](http://www.atoolbox.net/Tool.php?Id=952)

- [红黑树工具](https://rbtree.phpisfuture.com/)

- [Raft 演示工具](https://raft.github.io/)