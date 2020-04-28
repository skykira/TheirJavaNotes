# Java基础

- [`Class.this` 与 `this` 的区别](https://stackoverflow.com/questions/5666134/what-is-the-difference-between-class-this-and-this-in-java)

- [`toArray()` 与 `toArray(new String[0])`](https://stackoverflow.com/questions/18136437/whats-the-use-of-new-string0-in-toarraynew-string0)

- [`ThreadLocalMap` 概览](https://www.cnblogs.com/xzwblog/p/7227509.html)

- [反射修改 `static final` 的成员变量](https://www.zhihu.com/question/47054187)

# Java 并发

- [对象计算 hashcode 将导致偏向锁膨胀](https://blog.csdn.net/P19777/article/details/103125545)

- [偏向锁的批量重偏向与批量撤销](https://www.cnblogs.com/LemonFive/p/11248248.html)

    未达到批量重偏向阈值时，偏向锁膨胀为轻量级锁；达到批量重偏向阈值时，锁对象才会进行重偏向操作，且重偏向只能有一次。

- [Java 中 `volatile` 的语义](https://www.jianshu.com/p/4e59476963b0)

- [Java中的偏向锁、轻量级锁、重量级锁解析](https://blog.csdn.net/lengxiao1993/article/details/81568130?depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-1&utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-1)

- [Java中的锁降级✨✨](http://openjdk.java.net/jeps/8183909)

    该优化提案显示，目前，实现重量级锁的 `monitor` 对象可以在STW时被清除，清除的是那些只有 `VM Thread`会去访问它们的 `monitor` 对象（也就是，不再使用的 `monitor` 对象）。

    因此，重量级锁可以降级为轻量级锁。

- [`ReentrantReadWriteLock` 保证 `readHolds` 的可见性](https://stackoverflow.com/questions/1675268/java-instance-variable-visibility-threadlocal)

# Java 安全

- [如何理解恶意代码执行 `AccessController.doPrivileged()`](https://stackoverflow.com/questions/37962070/malicious-code-running-accesscontroller-doprivileged)

# 函数式编程

- [`BiConsumer` 为什么可以引用仅有一个参数的方法](https://stackoverflow.com/questions/58046693/biconsumer-and-method-reference-of-one-parameter)

# JVM

- [图说 Java 字节码指令](https://segmentfault.com/a/1190000008606277)

- [Java 编译器优化和运行期优化概览](https://blog.csdn.net/u013305783/article/details/81279175)

- [`Thread-Local Handshakes` 优化安全点STW](https://openjdk.java.net/jeps/312)

    对象的偏向锁撤销操作也可以无需程序运行到 `SafePoint`

- [Java启动参数 `javaagent` 的使用](https://www.cnblogs.com/rickiyang/p/11368932.html)

# 分布式

## Raft

- [客户端只读请求的处理](https://zhuanlan.zhihu.com/p/36592467)

# DateBase

- [MySQL LEFT JOIN/ INNER JOIN/RIGHT JOIN 执行过程](https://learnku.com/articles/27701)

- [MySQL 多表 Join (表连接) 和 Where 间的执行顺序：`nested loop join` 机制](https://blog.csdn.net/qq_27529917/article/details/78447882)

    每 Join 一次后执行 where 过滤结果集，交叉执行。
