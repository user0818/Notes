# JUC简介

在 Java 5.0 提供了 `java.util.concurrent`(简称JUC)包，在此包中增加了在并发编程中很常用的工具类，用于定义类似于线程的自定义子系统,包括线程池，异步 IO 和轻量级任务框架；还提供了设计用于多线程上下文中的 Collection 实现等。

# 进程和线程

## 线程状态

- NEW：新生状态
- RUNNABLE：运行状态
- BLOCKED：阻塞
- WAITING：等待
- TIMED_WAITING：超时等待
- TERMINATED：死亡 

## Wait/Sleep区别

1. 来自不同的类
   - `wait`：`Object`
   - `sleep`：`Thread`

2. 对锁的释放不同
   - `wait`释放锁
   - `sleep`不释放锁（抱着锁睡觉）

3. 使用范围不同
   - `wait`：只能在同步代码块中使用
   - `sleep；`：可以在任何地方睡

