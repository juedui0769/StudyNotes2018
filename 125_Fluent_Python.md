# Fluent Python

## 第 1 章



## 第 2 章

> P24， 2.3.2  元组拆包
>
> `print('%s/%s' % ('USA', '31195855'))` ，`%`运算符就利用了*元组拆包*

> P26 ， 2.3.4  具名元组，
>
> `collections.namedtuple` 是一个工厂函数，它可以用来构建一个带字段名的元组和一个有名字的类。



## 第 3 章

> P70， 3.8.1 集合字面量，`set()`、`{1}`、`{1, 2, 3}`
>
> 句法陷阱： `{}` 表示的是空字典；如果要表示空集，那么必须写成 `set()` 的形式



## 第7章  函数装饰器和闭包

>  搜索 “python 装饰器 java aop” ， 看看网友们怎么理解的。

> ```python
> from functools import wraps
> ```
> 一直不懂 `functools.wraps` 有什么用？找到一篇文章 https://python3-cookbook.readthedocs.io/zh_CN/latest/c09/p02_preserve_function_metadata_when_write_decorators.html 介绍的比较好，可以阅读，理解！

7.8.1  使用functools.lru_cache做备忘

LRU : Least Recently Used

> @singledispatch  不是为了把 Java 的那种方法重载带入 Python。在一个类中为同一个方法定义多个重载变体，比在一个函数中使用一长串 if/elif/elif/elif 块要更好。但是这两种方案都有缺陷，因为它们让代码单元（类或函数）承担的职责太多。 @singledispatch 的优点是支持模块化扩展：各个模块可以为它支持的各个类型注册一个专门函数。

7.9  叠放装饰器

P175  函数金字塔

## 第 8 章

**对象引用、可变性和垃圾回收**

http://pythontutor.com/visualize.html#mode=edit

```python
l1 = [3, [66, 55, 44], (7, 8, 9)]
l2 = list(l1)
l1.append(100)
l1[1].remove(55)
print('l1:', l1)
print('l2:', l2)
l2[1] += [33, 22]
l2[2] += (10, 11)
print('l1:', l1)
print('l2:', l2)
```

### weakref

`weakref.ref` 类其实是低层接口，供高级用途使用，多数程序最好使用 `weakref` 集合 和 `finalize`。也就是说，应该使用 `WeakKeyDictionary`、`WeakValueDictionary`、`WeakSet` 和 `finalize` （在内部使用弱引用），不要自己动手创建并处理 `weak.ref` 实例。

> 示例 8-19 ，的结果，让我很费解。仔细对照作者的说明，才看懂。我觉得这个例子值得复习多遍，直到深埋记忆！

https://docs.python.org/3/library/weakref.html ， weakref —— Weak references ，官方文档

> P199 ， 8.6.2  弱引用的局限
>
> 基本的 `list` 和 `dict` 实例不能作为弱引用的目标，但是它们的子类可以轻松地解决这个问题。
>
> `int` 和 `tuple` 实例不能作为弱引用的目标，甚至它们的子类也不行。

> P200 ， 千万不要依赖字符串或整数的驻留！比较字符串或整数是否相等时，应该使用 `==` ，而不是 `is` 。
>
> 驻留是 Python 解释器内部使用的一个特性。
>
> 共享字符串字面量是一种优化措施，称为*驻留*（interning）。CPython 还会在小的整数上使用这个优化措施，防止重复创建 “热门” 数字，如 0、-1 和 42。注意， CPython 不会驻留所有字符串和整数，驻留的条件是实现细节，而且没有文档说明。

## 第 9 章



























# 其他

## Pingo

http://www.pingo.io/docs/ ， 用来操作树莓派的python类库





# End