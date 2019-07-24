
> 后续笔记转移到“印象笔记” 2019年7月24日10:43:37

# Java并发编程

## 书籍

- Book01《Java并发编程实战》
  - 《Java Concurrency in Practice》
  - 作者： Brian Goetz, Tim Peierls, Joshua Bloch, Joseph Bowbeer, David Holmes, Doug Lea
  - 译者： 童云兰， 等
  - 机械工业出版社， 2012年2月第1版第1此印刷
- 

## Book01 拾遗

> 2.1 什么是线程安全性
> 
> 无状态对象一定是线程安全的。

> 3.4 不变性
> 
> 不可变对象一定是线程安全的。






### 线程安全类的定义

当多个线程访问某个类时，这个类始终都能表现出正确的行为，那么就称这个类是线程安全的。

当多个线程访问某个类时，不管运行时环境采用何种调度方式或者这些线程将如何交替执行，并且在主调代码中不需要任何额外的同步或协同，这个类都能表现出正确的行为，那么就称这个类是线程安全的。

在线程安全类的对象实例上执行的任何串行或并行操作都不会使对象处于无效状态。

### 一个无状态的Servlet

```java
@ThreadSafe
public class StatelessFactorizer implements Servlet {
    public void service(ServletRequest req, ServletResponse resp) {
        BigInteger i = extractFromRequest(req);
        BigInteger[] factors = factor(i);
        encodeIntoResponse(resp, factors);
    }
}
```

与大多数Servlet相同，`StatelessFactorizer`是无状态的：它既不包含任何域，也不包含任何对其他类中域的引用。计算过程中的临时状态仅存在于线程栈上的局部变量中，并且只能由正在执行的线程访问。访问`StatelessFactorizer`的线程不会影响另一个访问同一个`StatelessFactorizer`的线程的计算结果，因为这两个线程并没有共享状态，就好像它们都在访问不同的实例。由于线程访问无状态对象的行为并不会影响其他线程中操作的正确性，因此无状态对象是线程安全的。

> 无状态对象一定是线程安全的。

大多数Servlet都是无状态的，从而极大地降低了在实现Servlet线程安全性时的复杂性。只有当Servlet在处理请求时需要保存一些信息，线程安全性才会成为一个问题。

### 竞态条件（Race Condition）

在并发编程中，这种由于不恰当的执行时序而出现不正确的结果是一种非常重要的情况，它有一个正式的名字：竞态条件（Race Condition）。

当某个计算的正确性取决于多个线程的交替执行时序时，那么就会发生竞态条件。换句话说，就是正确的结果要取决于运气。

最常见的竞态条件类型就是“先检查后执行（Check-Then-Act）”操作，即通过一个可能失效的观测结果来决定下一步的动作。

... 两家星巴克之间有几分钟的路程，而就在这几分钟的时间里，系统的状态可能会发生变化。

这种观察结果的失效就是大多数竞态条件的本质——基于一种可能失效的观察结果来做出判断或者执行某个计算。

### 延迟初始化中的竞态条件

使用“先检查后执行”的一种常见情况就是延迟初始化。延迟初始化的目的是将对象的初始化操作推迟到实际被使用时才进行，同时要确保只被初始化一次。

```java
@NotThreadSafe
public class LazyInitRace {
    private ExpensiveObject instance = null;
    
    public ExpensiveObject getInstance() {
        if (instance == null) {
            instance = new ExpensiveObject();
        }
        return instance;
    }
}
```

### 多个状态的情况

`AtomicLong`, `AtomicReference`只能在单一状态下有效，当多个状态同时需要变化时，就必须加锁了。（自己写的，不知对不对？）

> 要保持状态的一致性，就需要在单个原子操作中更新所有相关的状态变量。（Book01 P20）

### 同步代码块（Synchronized Block）

Java提供了一种内置的锁机制来支持原子性：同步代码块（Synchronized Block）。

每个Java对象都可以用作一个实现同步的锁，这些锁被称为内置锁（Intrinsic Lock）或监视器锁（Monitor Lock）。

线程在进入同步代码块之前会自动获得锁，并且在退出同步代码块时自动释放锁，而无论是通过正常的控制路径退出，还是通过从代码块中抛出异常退出。

### 重入

程序清单2-7 如果内置锁不是可重入的，那么这段代码将发生死锁

```java
public class Widget {
    public synchronized void doSomething() {
        ...
    }
}

public class LoggingWidget extends Widget {
    public synchronized void doSomething() {
        System.out.println(toString() + ": calling doSomething");
        super.doSomething();
    }
}
```

### synchronized service

Book01 P24

由于`service`是一个`synchronized`方法，因此每次只有一个线程可以执行。

…… 这些请求将排队等待处理。我们将这种 Web 应用程序称之为不良并发（Poor Concurrency）应用程序：可同时调用的数量，不仅受到可用处理资源的限制，还受到应用程序本身结构的限制。

### 缩小同步代码的作用范围

幸运的是，通过缩小同步代码块的作用范围，我们很容易做到既确保`Servlet`的并发性，同时又维护线程安全性。

要确保同步代码块不要过小，并且不要将本应是原子的操作拆分到多个同步代码块中。

应该尽量将不影响共享状态且执行时间较长的操作从同步代码块中分离出去，从而在这些操作的执行过程中，其他线程可以访问共享状态。

```java
@ThreadSafe
public class CachedFactorizer implements Servlet {
    @GuardedBy("this") private BigInteger lastNumber;
    @GuardedBy("this") private BigInteger[] lastFactors;
    @GuardedBy("this") private long hits;
    @GuardedBy("this") private long cacheHits;
    
    public synchronized long getHits() { return hits; }
    public synchronized double getCacheHitRatio() {
        return (double) cacheHits / (double) hits;
    }
    public void service(ServletRequest req, ServletResponse resp) {
        BigInteger i = extractFromRequest(req);
        BigInteger[] factors = null;
        synchronized (this) {
            ++hits;
            if (i.equals(lastNumber)) {
                ++cacheHits;
                factors = lastFactors.clone();
            }
        }
        if (factors == null) {
            factors = factor(i);
            synchronized (this) {
                lastNumber = i;
                lastFactors = factors.clone();
            }
        }
        encodeIntoResponse(resp, factors);
    }
}
```

当访问状态变量或者在复合操作的执行期间，`CachedFactorizer`需要持有锁，**但在执行时间较长的因数分解运算之前要释放锁**。

> 通常，在简单性与性能之间存在这相互制约因素。当实现某个同步策略时，一定不要盲目地为了性能而牺牲简单性（这可能会破坏安全性）。

> 当执行时间较长的计算或者可能无法快速完成的操作时（例如，网络I/O或控制台I/O），一定不要持有锁。

### 重排序（Reordering）

> 在没有同步的情况下，编译器、处理器以及运行时等都有可能对操作的执行顺序进行一些意想不到的调整。在缺乏足够同步的多线程程序中，要想对内存操作的执行顺序进行判断，几乎无法得出正确的结论。

