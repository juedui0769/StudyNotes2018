# CountDownLatch

![](./imgs/116_CountDownLatch_001.png)

`CountDownLatch`本身的代码很简单，它主要是调用自己内部的这个`Sync`来实现功能；

而`Sync`继承自`AbstractQueuedSynchronizer`，这个类非常复杂，它有两个内部类：

- `Node`
- `ConditionObject`





# End