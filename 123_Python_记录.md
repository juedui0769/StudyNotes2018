# Python学习记录


## 01 虚拟环境

可以安装一个虚拟环境，在 Python 2 和 Python 3 之间切换，安装方式如下（Linux 和 windows 不同）

**virtualenvwrapper，virtualenvwrapper-win**

- https://virtualenvwrapper.readthedocs.io/en/latest/install.html（Linux）

```sh
pip install virtualenvwrapper
# or
sudo pip install virtualenvwrapper
```

- https://pypi.org/project/virtualenvwrapper-win/（windows上）

```sh
# using pip
pip install virtualenvwrapper-win

# using easy_install
easy_install virtualenvwrapper-win

# from source
git clone git://github.com/davidmarble/virtualenvwrapper-win.git
cd virtualenvwrapper-win
python setup.py install   # or pip install .
```

可以使用`pip`安装或者`easy_install`安装，或者直接`python setup.py install`，总之首先还是得先安装Python的任意版本，再安装这个虚拟环境。（Python的安装，官网）

**虚拟环境的使用：**

```sh
# 不带参数，列举出本机所有的环境
workon
# py3 是环境名称
workon py3
# deactivate ， 退出环境
deactivate
```

```sh
python -V
```

如上，大写的`V`显示版本……

## 02 IPython

- https://ipython.org/

- ~~https://jupyter.readthedocs.io/en/latest/install.html~~
- https://jupyter.readthedocs.io/en/latest/projects/content-projects.html ，其他的编程语言也可以安装
  - Use another programming language such as R or Java
- https://ipython.readthedocs.io/en/stable/install/index.html，这篇文章是我采用的安装方式
  - `pip install ipython`
  - Beginning with version 6.0, IPython stopped supporting compatibility with Python version lower than 3.3 including all versions of Python 2.7。

```sh
pip install ipython
```

以上，就是安装`ipython`的方式，另外： 6.0版的ipython不再支持`2.7`和`3.3`以下的python

## 03 FreeMind

"FreeMind"是一款免费的“思维导图”软件，用来做笔记是最好不过的了。

- 首页： http://freemind.sourceforge.net/wiki/index.php/Main_Page
- 下载： http://freemind.sourceforge.net/wiki/index.php/Download
  - 需要先安装 Java 环境，才能使用 FreeMind。
  - 在此链接下，可以找到各种环境下的 FreeMind 的安装文件。
- windows环境下的安装文件，已上传到百度网盘备份（由于百度网盘的限速，原始链接可能下载速度更好，下载前请衡量……）
  - 链接: https://pan.baidu.com/s/1qQ7wXzoEMwMsFakXI-bk3A 提取码: j3bj 
- 快捷方式（windows）
  - `Enter` ：创建平级节点
  - `Insert` ： 创建下级节点
  - `F2` ： 编辑节点
  - `Delete` ： 删除节点
  - 更改节点顺序，这个快捷键没有找到，我目前的做法是： 创建一个平级节点，将子节点全部拖动到新的节点上，从而，达到更改顺序的目的。

## 04 PyCharm

因为有 python 虚拟环境，所以，本地可以安装很多版本的python。

那 PyCharm 可以为每个工程指定不同版本的python，不同于 Java 的 Compiler ，python 对应的是 Intercepter，在 Settings 下可以找到 Project Intercepter，在这里设置python解释器。

## 05 Python Shell 清屏命令

```python
import os
n = os.system('cls')
```

https://blog.csdn.net/howard2005/article/details/79879289， https://blog.csdn.net/cxcxrs/article/details/81219395 ， 参考上面的博文。

## 06 pythontutor

http://www.pythontutor.com/





## Book01

> 《简明Python教程第4版》

> Python 3 ： `> 3.5.1`的 Python 发行版


### 基础

#### 注释

注释，是任何存在于 `#` 号右侧的文字。

#### 字面常量

Literal Constants

#### 其他

- 数字、字符串，字符串是不可变的
- 单引号、双引号、三引号

#### 格式化方法

```python
age = 20
name = "Swaroop"

print('{0} was {1} years old when he wrote this book'.format(name, age))
print('Why is {0} playing with that python?'.format(name))
```

数字只是一个可选选项，同样可以写成：

```python
age = 20
name = "Swaroop"

print('{} was {} years old when he wrote this book'.format(name, age))
print('Why is {} playing with that python?'.format(name))
```

`format`的用法很多，……



## Book02

《 Fluent Python 流畅的Python》，[巴西] Luciano Ramalho 著，安道 吴珂 译，2017.5

代码： https://github.com/fluentpython/example-code

### dunder-method

![](./imgs/123_Python_dunder_method.png)

第1章 Python数据模型，开篇讲解的就是 dunder method。

### 第1章

第一章 Python 数据模型

```python
import collections

Card = collections.namedtuple('Card', ['rank', 'suit'])


class FrenchDeck:
    ranks = [str(n) for n in range(2, 11)] + list('JQKA')
    suits = 'spades diamonds clubs hearts'.split()

    def __init__(self):
        self._cards = [Card(rank, suit) for suit in self.suits for rank in self.ranks]

    def __len__(self):
        return len(self._cards)

    def __getitem__(self, position):
        return self._cards[position]

suit_values = dict(spades=3, hearts=2, diamonds=1, clubs=0)


def spades_high(card):
    rank_value = FrenchDeck.ranks.index(card.rank)
    return rank_value * len(suit_values) + suit_values[card.suit]


if __name__ == "__main__":
    deck = FrenchDeck()
    print("扑克牌数量：{}".format(len(deck)))
    print("第一张牌是 {}, 最后一张是 {}".format(deck[0], deck[-1]))
    from random import choice
    print("随机抽取三张牌...")
    print(choice(deck))
    print(choice(deck))
    print(choice(deck))
    print("显示最上面三张牌：{}".format(deck[:3]))
    print("显示最下面三张牌：{}".format(deck[:-4:-1]))
    print("显示牌面是 A 的牌： {}".format(deck[12::13]))
    print("=== 顺序显示所有牌 ===")
    for card in deck:
        print(card)
    print("=== 逆序显示所有牌 ===")
    for card in reversed(deck):
        print(card)
    print(Card('Q', 'hearts') in deck)
    print(Card('Q', 'bearts') in deck)
    print("===")
    for card in sorted(deck, key=spades_high):
        print(card)
```

`ranks = [str(n) for n in range(2, 11)] + list('JQKA')` 这一句太简洁了，我每次读源码时，都在这里卡壳。这其实是初始化扑克牌的：2 ~ 10、 J、 Q、 K、 A ，从 2 到 A 。

我们通过数据模型和一些合成来实现我们想要的功能：

- 通过实现 `__len__` 和 `__getitem__` 这两个特殊方法，`FrenchDeck`就跟一个 Python 自有的序列数据类型一样，可以体现出 Python 的核心语言特性（例如迭代和切片）
- 同时这个类还可以用于标准库中诸如 `random.choice`、`reversed` 和 `sorted` 这些函数。
- 另外，对合成的运用使得 `__len__` 和 `__getitem__` 的具体实现可以代理给 `self._cards` 这个 Python 列表（即 list 对象）。

#### 1.2 如何使用特殊方法

首先明确一点，特殊方法的存在是为了被 Python 解释器调用的，你自己并不需要调用它们。

- 也就是说没有 `my_object.__len__()`这种写法，而应该使用 `len(my_object)` 。
- 在执行 `len(my_object)` 的时候，如果 `my_object` 是一个自定义类的对象，那么 Python 会自己去调用其中由你实现的 `__len__` 方法。

很多时候，特殊方法的调用是隐式的，比如 `for i in x:` 这个语句，背后实用的是 `iter(x)`，而这个函数的背后则是 `x.__iter__()` 方法。当然前提是这个方法在 `x` 中被实现了。

#### 1.2.4 自定义的布尔值

默认情况下，我们自己定义的类的实例总被认为是真的，除非这个类对 `__bool__` 或者 `__len__` 函数有自己的实现。

- `bool(x)` 的背后是调用 `x.__bool__()` 的结果；
- 如果不存在 `__bool__` 方法，那么 `bool(x)` 会尝试调用 `x.__len__()`。若返回 `0`，则 `bool` 会返回 `False`；否则返回 `True`。

#### 1.3 特殊方法一览

Python 语言参考手册中的“Data Model”（https://docs.python.org/3/reference/datamodel.html）一章列出了 `83` 个特殊方法的名字，其中 `47` 个用于实现算术运算、位运算和比较操作。

#### 总结

第一章是个概括性的章节，以“扑克牌”类开局，从“特殊方法”谈起；表达“自定义”类，实现某些特殊方法就可以具备内置类同样的功能和待遇；双下划线方法（特殊方法）是Python的特色，也是其简洁性的证明；第一章只是抛砖引玉，后续章节会陆续展开。

### 第2章

第2章 序列构成的数组

不管是哪种数据结构，字符串、列表、字节序列、数组、XML元素，抑或是数据库查询结果，它们都共用一套丰富的操作：迭代、切片、排序，还有拼接。

#### 2.1 内置序列类型概览

![](./imgs/123_Python_list.png)

“容器序列”存放的是它们所包含的任意类型的对象的引用，而“扁平序列”里存放的是值而不是引用。换句话说，扁平序列其实是一段连续的内存空间。由此可见扁平序列其实更加紧凑，但是它里面只能存放诸如字符、字节和数值这种基础类型。

![](./imgs/123_Python_sequence_01.png)

#### 2.2 列表推导和生成器表达式

> 列表推导 是构建列表（list）的快捷方式，而 生成器表达式 则可以用来创建其他任何类型的序列。
>
> .……
>
> 如果你的代码里并不经常使用它们，那么很可能你错过了许多写出可读性更好且更高效的代码的机会。

很多 Python 程序员都把 列表推导（list comprehension）简称 listcomps，生成器表达式（generator expression）则称为 genexps。

```python
if __name__ == "__main__":
    symbols = '$¢£¥€¤'
    codes = [ord(symbol) for symbol in symbols]
    print(codes)
    symbols = [ chr(x) for x in [36, 162, 163, 165, 8364, 164]]
    print(symbols)
```

`for` 循环可以胜任很多任务：遍历一个序列以求得总数或挑出某个特定的元素、用来计算总和或是平均数，还有其他任何你想做的事情。

> 列表推导 也可能被滥用。
>
> 通常的原则是： 只用列表推导来创建新的列表，并且尽量保持简短。
>
> 如果列表推导的代码超过了两行，你可能就要考虑是不是得用 `for` 循环重写了。

**列表推导不会再由变量泄漏的问题**

列表推导、生成器表达式，以及同它们很相似的集合推导和字典推导，在 Python3 中都有了自己的局部作用域，就像函数似的。表达式内部的变量和赋值只在局部起作用，表达式的上下文里的同名变量还可以被正常引用，局部变量并不会影响到它们。

```python
if __name__ == '__main__':
    x = 'ABC'
    dummy = [ord(x) for x in x]
    print(x)
    print(dummy)
```

如上，外部的`x` 和生成器表达式中的 `x` 互不干扰。

> 列表推导可以帮助我们把一个序列或是其他可迭代类型中的元素过滤或是加工，然后再新建一个列表。
>
> Python 内置的 `filter` 和 `map` 函数组合起来也能达到这一效果，但是可读性上打了不小的折扣。

**列表推导同filter和map的比较**

```python
if __name__ == '__main__':
    symbols = '$¢£¥€¤'
    beyond_ascii = [ord(s) for s in symbols if ord(s) > 127]
    print(beyond_ascii)
    beyond_ascii = list(filter(lambda c: c > 127, map(ord, symbols)))
    print(beyond_ascii)
```

代码如上，列表推导比filter/map简洁很多。

**使用列表推导计算笛卡尔儿积**

```python
if __name__ == '__main__':
    colors = ['black', 'white']
    sizes = ['S', 'M', 'L']
    tshirts = [(color, size) for color in colors for size in sizes]
    print(tshirts)
    for color in colors:
        for size in sizes:
            print((color, size))
    tshirts = [(color, size) for size in sizes
                             for color in colors]
    print(tshirts)
```

#### 2.2.4  生成器表达式

> 虽然也可以用 “列表推导” 来初始化 元组、数组或其他序列类型，但是生成器表达式是更好的选择。
>
> 这是因为生成器表达式背后遵守了迭代器协议，可以逐个地产出元素，而不是先建立一个完整的列表，然后再把这个列表传递到某个构造函数里。
>
> 前面那种方式显然能够节省内存。
>
> 生成器表达式的语法跟列表推导差不多，只不过把方括号换成圆括号而已。

```python
if __name__ == '__main__':
    symbols = '$¢£¥€¤'
    a = tuple(ord(symbol) for symbol in symbols)
    print(a)

    import array

    b = array.array('I', (ord(symbol) for symbol in symbols))
    print(b)
```

`tuple(ord(symbol) for symbol in symbols)` ： 如果生成器表达式是一个函数调用过程中的唯一参数，那么不需要额外再用括号把它围起来。



```python
if __name__ == '__main__':
    colors = ['blank', 'white']
    sizes = ['S', 'M', 'L']
    for tshirt in ('%s %s' % (c, s) for c in colors for s in sizes):
        print(tshirt)
```

代码如上，生成器表达式逐个产出元素，从来不会一次性产出一个含有 `6` 个T恤样式的列表。

#### 2.3.1 元组和记录

> 元组其实是对数据的记录： 元组中的每个元素都存放了记录中一个字段的数据，外加逐个字段的位置。正是这个位置信息给数据赋予了意义。

```python
if __name__ == '__main__':
    lax_coordinates = (33.9425, -118.408056)
    city, year, pop, chg, area = ('Tokyo', 2003, 32450, 0.66, 8014)
    traveler_ids = [('USA', '31195855'), ('BRA', 'CE342567'),
                    ('ESP', 'XDA205856')]
    for passport in sorted(traveler_ids):
        print('%s/%s' % passport)
    for country, _ in traveler_ids:
        print(country)
```

#### 2.3.2 元组拆包

我们把元组 `('Tokyo', 2003, 32450, 0.66, 8014)` 里的元素分别赋值给变量 `city, year, pop, chg, area` ，而这所有的赋值我们只用一行声明就写完了。

> 元组拆包 可以应用到任何可迭代对象上，唯一的硬性要求是，被可迭代对象中的元素数量必须要跟接受这些元素的元组的空档数一致。
>
> 除非我们用 `*` 来表示忽略多余的元素

1. 最好辨认的元组拆包形式就是平行赋值

```python
lax_coordinates = (33.9425, -118.408056)
latitude, longitude = lax_coordinates
```

2. 另外一个很优雅的写法当属不使用中间变量交换两个变量的值

```python
b, a = a, b
```

3. 还可以用 `*` 运算符把一个可迭代对象拆开作为函数的参数

```python
divmod(20, 8)
t = (20, 8)
divmod(*t)
```

4. 让一个函数可以用元组的形式返回多个值，比如 `os.path.split()` 函数就会返回以路径和最后一个文件名组成的元组 `(path, last_part)`

```python
>>> import os
>>> _, filename = os.path.split('/home/luciano/.ssh/idrsa.pub')
>>> filename
'idrsa.pub'
```

> 在进行拆包的时候，我们不总是对元组里所有的数据都感兴趣，`_` 占位符能帮助处理这种情况。

5. 用 `*` 来处理剩下的元素， 在 Python 中，函数用 `*args` 来获取不确定数量的参数，算是一种经典写法了。

> 于是 Python3 里，这个概念被扩展到了平行赋值中

```python
>>> a, b, *rest = range(5)
>>> a, b, rest
(0, 1, [2, 3, 4])
>>> a, b, *rest = range(2)
>>> a, b, rest
(0, 1, [])
```

在平行赋值中，`*` 前缀只能用在一个变量名前面，但是这个变量可以出现在赋值表达式的任意位置

```python
>>> a, *body, c, d = range(5)
>>> a, *body, c, d
(0, [1, 2], 3, 4)
>>> *head, b, c, d = range(5)
>>> head, b, c, d
([0, 1], 2, 3, 4)
```

#### 2.3.3 嵌套元组拆包

接受表达式的元组可以是嵌套式的，例如 `(a, b, (c, d))` 。只要这个接受元组的嵌套结构符合表达式本身的嵌套结构，Python 就可以作出正确的对应。

```python
if __name__ == '__main__':
    metro_areas = [
        ('Tokyo', 'JP', 36.933, (35.689722, 139.691667)),  # <1>
        ('Delhi NCR', 'IN', 21.935, (28.613889, 77.208889)),
        ('Mexico City', 'MX', 20.142, (19.433333, -99.133333)),
        ('New York-Newark', 'US', 20.104, (40.808611, -74.020386)),
        ('Sao Paulo', 'BR', 19.649, (-23.547778, -46.635833)),
    ]

    print('{:15} | {:^9} | {:^9}'.format('', 'lat.', 'long.'))
    fmt = '{:15} | {:9.4f} | {:9.4f}'
    for name, cc, pop, (latitude, longitude) in metro_areas:  # <2>
        if longitude <= 0:  # <3>
            print(fmt.format(name, latitude, longitude))
```

#### 2.3.4 具名元组

`collections.namedtuple` 是一个工厂函数，它可以用来构建一个带字段名的元组和一个有名字的类——这个带名字的类对调试程序有很大帮助。

```python
if __name__ == '__main__':
    from collections import namedtuple
    City = namedtuple('City', 'name country population coordinates')
    tokyo = City('Tokyo', 'JP', 36.933, (35.689722, 139.691667))
    print(tokyo)
    print(tokyo.population)
    print(tokyo.coordinates)
    print(tokyo[1])
```

> 创建一个具名元组需要两个参数，一个是类名，另一个是类的各个字段的名字。
>
> 后者可以是由数个字符串组成的可迭代对象，或者是由空格分隔开的字段名组成的字符串。
>
> 存放在对应字段里的数据要以一串参数的形式传入到构造函数中



```python
if __name__ == '__main__':
    from collections import namedtuple
    City = namedtuple('City', 'name country population coordinates')

    print(City._fields)

    LatLong = namedtuple('LatLong', 'lat long')
    delhi_data = ('Delhi NCR', 'IN', 21.935, LatLong(28.613889, 77.208889))
    delhi = City._make(delhi_data)

    print(delhi._asdict())

    for key, value in delhi._asdict().items():
        print(key + ' : ', value)
```

- `_fields` 属性是一个包含这个类所有字段名称的元组。
- 用 `_make()` 通过接受一个可迭代对象来生成这个类的一个实例，它的作用跟 `City(*delhi_data)` 是一样的。
- `_asdict()` 把具名元组以 `collecitons.OrderedDict` 的形式返回，我们可以利用它来把元组里的信息友好地呈现出来。

#### 2.3.5 作为不可变列表的元组

|                            | 列表 | 元组 |                                                              |
| -------------------------- | ---- | ---- | ------------------------------------------------------------ |
| `s.__add__(s2)`            | ✔    | ✔    | s + s2，拼接                                                 |
| `s.__iadd__(s2)`           | ✔    |      | s += s2，就地拼接                                            |
| `s.append(e)`              | ✔    |      | 在尾部添加一个新元素                                         |
| `s.clear()`                | ✔    |      | 删除所有元素                                                 |
| `s.__contains__(e)`        | ✔    | ✔    | s 是否包含 e                                                 |
| `s.copy()`                 | ✔    |      | 列表的浅复制                                                 |
| `s.count(e)`               | ✔    | ✔    | e 在 s 中出现的次数                                          |
| `s.__delitem__(p)`         | ✔    |      | 把位于 p 的元素删除                                          |
| `s.extend(it)`             | ✔    |      | 把可迭代对象 it 追加给 s                                     |
| `s.__getitem__(p)`         | ✔    | ✔    | s[p]，获取位置 p 的元素                                      |
| `s.__getnewargs__()`       | ✔    | ✔    | 在 pickle 中支持更加优化的序列化                             |
| `s.index(e)`               | ✔    | ✔    | 在 s 中找到元素 e 第一次出现的位置                           |
| `s.insert(p, e)`           | ✔    |      | 在位置 p 之前插入元素 e                                      |
| `s.__iter__()`             | ✔    | ✔    | 获取 s 的迭代器                                              |
| `s.__len__()`              | ✔    | ✔    | len(s)，元素的数量                                           |
| `s.__mul__(n)`             | ✔    | ✔    | s * n，n 个 s 的重复拼接                                     |
| `s.__imul__(n)`            | ✔    |      | s *= n，就地重复拼接                                         |
| `s.__rmul__(n)`            | ✔    | ✔    | n * s，反向拼接                                              |
| `s.pop([p])`               | ✔    |      | 删除最后或者是（可选的）位于 p 的元素， 并返回它的值         |
| `s.remove(e)`              | ✔    |      | 删除 s 中的第一次出现的 e                                    |
| `s.reverse()`              | ✔    |      | 就地把 s 的元素倒序排列                                      |
| `s.__reversed__()`         | ✔    |      | 返回 s 的倒序迭代器                                          |
| `s.__setitem__(p, e)`      | ✔    |      | s[p] = e，把元素 e 放在位置 p，替代已经在那个位置的元素      |
| `s.sort([key], [reverse])` | ✔    |      | 就地对 s 中的元素进行排序，可选的参数有键（key）和是否倒序（reverse） |

除了跟增减元素相关的方法之外，元组支持列表的其他所有方法。

#### 2.4 切片

在 Python 里，像列表（list）、元组（tuple）和字符串（str）这类序列类型都支持切片操作

#### 2.4.1 为什么切片和区间会忽略最后一个元素

在切片和区间操作里不包含区间范围的最后一个元素是 Python 的风格，这个习惯符合 Python、C和其他语言里以 `0` 作为起始下标的传统。这样做带来的好处如下：

1. 当只有最后一个位置信息时，我们也可以快速看出切片和区间里有几个元素： `range(3)` 和 `my_list[:3]` 都返回 `3` 个元素。

2. 当起止位置信息都可见时，我们可以快速计算出切片和区间的长度，用后一个数减去第一个下标（`stop -start`）即可。

3. 这样做也让我们可以利用任意一个下标来把序列分割成不重叠的两部分，只要写成 `my_list[:x]` 和 `my_list[x:]` 就可以了，如下所示。

```python
>>> l = [10, 20, 30, 40, 50, 60]
>>> l[:2]
[10, 20]
>>> l[2:]
[30, 40, 50, 60]
```

#### 2.4.2 对对象进行切片

我们可以用 `s[a:b:c]` 的形式对 s 在 a 和 b 之间以 c 为间隔取值。c 的值还可以为负，负值意味着反向取值。

```python
>>> s = 'bicycle'
>>> s[::3]
'bye'
>>> s[::-1]
'elcycib'
>>> s[::-2]
'eccb'
```

#### 2.4.3 多维切片和省略

`[]` 运算符里还可以使用以逗号分开的多个索引或者是切片，外部库 `NumPy` 里就用到了这个特性，二维的 `numpy.ndarray` 就可以用 `a[i, j]` 这种形式来获取，抑或是用 `a[m:n, k:l]` 的方式来得到二维切片。

> 省略（ellipsis）的正确书写方法是三个英语句号（`...`）
>
> 这节没细讲

2.4.3 节，可以直接略过，没什么用，特别是初期时……

#### 2.4.4 给切片赋值

```python
>>> l = list(range(10))
>>> l
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
>>> l[2:5] = [20, 30]
>>> l
[0, 1, 20, 30, 5, 6, 7, 8, 9]
>>> del l[5:7]
>>> l
[0, 1, 20, 30, 5, 8, 9]
>>> l[3::2] = [11, 22]
>>> l
[0, 1, 20, 11, 5, 22, 9]
>>> l[2:5] = 100
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: can only assign an iterable
>>> l[2:5] = [100]
>>> l
[0, 1, 100, 22, 9]
```

`l[2:5] = 100` ，这句报错，赋值语句的右侧必须是个可迭代对象，`l[2:5] = [100]`

上面代码，就是切片的赋值操作，`del l[5:7]` 是删除……

#### 2.5 对序列使用`+`和`*`

```python
>>> l = [1, 2, 3]
>>> l * 5
[1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3]
>>> 5 * 'abcd'
'abcdabcdabcdabcdabcd'
```

如果想要把一个序列复制几份然后再拼接起来，更快捷的做法是把这个序列乘以一个整数。同样，这个操作会产生一个新序列。

`+` 和 `*` 都遵循这个规律，不修改原有的操作对象，而是构建一个全新的序列。

#### 建立由列表组成的列表

```python
>>> board = [['_'] * 3 for i in range(3)]
>>> board
[['_', '_', '_'], ['_', '_', '_'], ['_', '_', '_']]
>>> board[1][2] = 'X'
>>> board
[['_', '_', '_'], ['_', '_', 'X'], ['_', '_', '_']]
```

以下代码演示错误的做法：

```python
>>> weird_board = [['_'] * 3] * 3
>>> weird_board
[['_', '_', '_'], ['_', '_', '_'], ['_', '_', '_']]
>>> weird_board[1][2] = '0'
>>> weird_board
[['_', '_', '0'], ['_', '_', '0'], ['_', '_', '0']]
```

#### 2.6 序列的增量赋值

```python
>>> l = [1, 2, 3]
>>> id(l)
39467848
>>> l *= 2
>>> id(l)
39467848
>>> l
[1, 2, 3, 1, 2, 3]
>>> t = (1, 2, 3)
>>> id(t)
35889944
>>> t
(1, 2, 3)
>>> t *= 2
>>> t
(1, 2, 3, 1, 2, 3)
>>> id(t)
30530728
```

对不可变序列进行重复拼接操作的话，效率会很低，因为每次都有一个新对象，而解释器需要把原来对象中的元素先复制到新的对象里，然后再追加新的元素。

#### 2.7 list.sort方法和内置函数sorted

`list.sort` 方法会就地排序列表，也就是说不会把原列表复制一份。这也就是这个方法的返回值是 `None` 的原因，提醒你本方法不会新建一个列表。

> 在这种情况下返回 `None` 其实是 Python 的一个惯例： 如果一个函数或者方法对对象进行的是就地改动，那它就应该返回 `None`，好让调用者知道传入的参数发生了变动，而且并未产生新的对象。

与 `list.sort` 相反的是内置函数 `sorted`，它会新建一个列表作为返回值。这个方法可以接受任何形式的可迭代对象作为参数，甚至包括不可变序列或生成器。而不管 `sorted` 接受的是怎样的参数，它最后都会返回一个列表。

不管是 `list.sort` 方法还是 `sorted` 函数，都有两个可选的关键字参数。

- `reverse` ：如果被设定为 `True`，被排序的序列里的元素会以降序输出（也就是说把最大值当作最小值来排序）。这个参数的默认值是 `False`。
- `key` ： 一个只有一个参数的函数，这个函数会被用在序列里的每一个元素上，所产生的结果将是排序算法依赖的对比关键字。这个参数的默认值是恒等函数（identity function），也就是默认用元素自己的值来排序。
  - 比如说，在对一些字符串排序时，可以用 `key=str.lower` 来实现忽略大小写的排序，
  - 或者是用 `key=len` 进行基于字符串长度的排序。

> 可选参数 `key` 还可以在内置函数 `min()` 和 `max()` 中起作用。
>
> 另外，还有些标准库里的函数也接受这个参数，像 `itertools.groupby()` 和 `heapq.nlargest()` 等。

```python
>>> fruits = ['grape', 'raspberry', 'apple', 'banana']
>>> fruits
['grape', 'raspberry', 'apple', 'banana']
>>> sorted(fruits)
['apple', 'banana', 'grape', 'raspberry']
>>> fruits
['grape', 'raspberry', 'apple', 'banana']
>>> sorted(fruits, reverse=True)
['raspberry', 'grape', 'banana', 'apple']
>>> sorted(fruits, key=len)
['grape', 'apple', 'banana', 'raspberry']
>>> sorted(fruits, key=len, reverse=True)
['raspberry', 'banana', 'grape', 'apple']
>>> fruits
['grape', 'raspberry', 'apple', 'banana']
>>> fruits.sort()
>>> fruits
['apple', 'banana', 'grape', 'raspberry']
```

#### 2.8 用bisect来管理已排序的序列

> 书上提到的链接 https://code.activestate.com/recipes/577197-sortedcollection/ ，国内访问不了。

https://docs.python.org/3/library/bisect.html， Python 官方文档

```python
import bisect
import sys

HAYSTACK = [1, 4, 5, 6, 8, 12, 15, 20, 21, 23, 23, 26, 29, 30]
NEEDLES = [0, 1, 2, 5, 8, 10, 22, 23, 29, 30, 31]

ROW_FMT = '{0:2d} @ {1:2d}    {2}{0:<2d}'

def demo(bisect_fn):
    for needle in reversed(NEEDLES):
        position = bisect_fn(HAYSTACK, needle)  #1
        offset = position * '  |'  #2
        print(ROW_FMT.format(needle, position, offset)) #3

if __name__ == '__main__':
    if sys.argv[-1] == 'left':  #4
        bisect_fn = bisect.bisect_left
    else:
        bisect_fn = bisect.bisect

    print('DEMO:', bisect_fn.__name__)  #5
    print('haystack ->', ' '.join('%2d' % n for n in HAYSTACK))
    demo(bisect_fn)
```

```python
import bisect

def grade(score, breakpoints=[60, 70, 80, 90], grades='FDCBA'):
    i = bisect.bisect(breakpoints, score)
    return grades[i]

if __name__ == '__main__':
    a = [grade(score) for score in [33, 99, 77, 70, 89, 90, 100]]
    print(a)
```



这一节，没看懂，要多复习揣摩一下！

#### 2.8.2 用bisect.insort插入新元素

排序很耗时，因此在得到一个有序序列之后，我们最好能够保持它的有序。`bisect.insort` 就是为了这个而存在的。

`insort(seq, item)` 把变量 `item` 插入到序列 `seq` 中，并能保持 `seq` 的升序顺序。

```python
import bisect
import random

SIZE = 7

random.seed(1729)

if __name__ == '__main__':
    my_list = []
    for i in range(SIZE):
        new_item = random.randrange(SIZE * 2)
        bisect.insort(my_list, new_item)
        print('%2d ->' % new_item, my_list)
```

#### 2.9.1 数组

如果我们需要一个只包含数字的列表，那么 `array.array` 比 `list` 更高效。

```python
from array import array     #1
from random import random

if __name__ == '__main__':
    floats = array('d', (random() for i in range(10**7)))      #2
    print(floats[-1])   #3
    fp = open('floats.bin', 'wb')
    floats.tofile(fp)   #4
    fp.close()
    floats2 = array('d')    #5
    fp = open('floats.bin', 'rb')
    floats2.fromfile(fp, 10**7)     #6
    fp.close()
    print(floats2[-1])  #7
    print(floats2 == floats)    #8
```

生成的 `floats.bin` 文件有76.2MB，注意将其添加到`.gitignore`中。

> 另外一个快速序列化数字类型的方法是使用 `pickle` （https://docs.python.org/3/library/pickle.html）模块。

|                           | 列表 | 数组 |                                                              |
| ------------------------- | ---- | ---- | ------------------------------------------------------------ |
| `s.__add__(s2)`           | ✔    | ✔    | s + s2，拼接                                                 |
| `s.__iadd__(s2)`          | ✔    | ✔    | s += s2，就地拼接                                            |
| `s.append(e)`             | ✔    | ✔    | 在尾部添加一个元素                                           |
| `s.byteswap`              |      | ✔    | 翻转数组内每个元素的字节序列，转换字节序                     |
| `s.clear()`               | ✔    |      | 删除所有元素                                                 |
| `s.__contains__(e)`       | ✔    | ✔    | s 是否包含 e                                                 |
| `s.copy()`                | ✔    |      | 对列表浅复制                                                 |
| `s.__copy__()`            |      | ✔    | 对 `copy.copy` 的支持                                        |
| `s.count(e)`              | ✔    | ✔    | s 中 e 出现的次数                                            |
| `s.__deepcopy__()`        |      | ✔    | 对 copy.deepcopy 的支持                                      |
| `s.__delitem__(p)`        | ✔    | ✔    | 删除位置 p 的元素                                            |
| `s.extend(it)`            | ✔    | ✔    | 将可迭代对象 it 里的元素添加到尾部                           |
| `s.frombytes(b)`          |      | ✔    | 将压缩成机器值的字节序列读出来添加到尾部                     |
| `s.fromfile(f, n)`        |      | ✔    | 将二进制文件 f 内，含有机器值读出来添加到尾部，最多添加 n 项 |
| `s.fromlist(l)`           |      | ✔    | 将列表里的元素添加到尾部，如果其中任何一个元素导致了 TypeError 异常，那么所有的添加都会取消 |
| `s.__getitem__(p)`        | ✔    | ✔    | s[p]，读取位置 p 的元素                                      |
| `s.index(e)`              | ✔    | ✔    | 找到 e 在序列中第一次出现的位置                              |
| `s.insert(p, e)`          | ✔    | ✔    | 在位于 p 的元素之前插入元素 e                                |
| `s.itemsize`              |      | ✔    | 数组中每个元素的长度是几个字节                               |
| `s.__iter__()`            | ✔    | ✔    | 返回迭代器                                                   |
| `s.__len__()`             | ✔    | ✔    | len(s)，序列的长度                                           |
| `s.__mul__(n)`            | ✔    | ✔    | s * n，重复拼接                                              |
| `s.__imul__(n)`           | ✔    | ✔    | s *= n，就地重复拼接                                         |
| `s.__rmul__(n)`           | ✔    | ✔    | n * s ，反向重复拼接                                         |
| `s.pop([p])`              | ✔    | ✔    | 删除位于 p 的值并返回这个值，p 的默认值是最后一个元素的位置  |
| `s.remove(e)`             | ✔    | ✔    | 删除序列里第一次出现的 e 元素                                |
| `s.reverse()`             | ✔    | ✔    | 就地调转序列中元素的位置                                     |
| `s.__reversed__()`        | ✔    |      | 返回一个从尾部开始扫描元素的迭代器                           |
| `s.__setitem__(p, e)`     | ✔    | ✔    | s[p] = e，把位于 p 位置的元素替换成 e                        |
| `s.sort([key], [revers])` | ✔    |      | 就地排序序列，可选参数有 key 和 reverse                      |
| `s.tobytes()`             |      | ✔    | 把所有元素的机器值用 bytes 对象的形式返回                    |
| `s.tofile(f)`             |      | ✔    | 把所有元素以机器值的形式写入一个文件                         |
| `s.tolist()`              |      | ✔    | 把数组转换成列表，列表里的元素类型是数字对象                 |
| `s.typecode`              |      | ✔    | 返回只有一个字符的字符串，代表数组元素在 C 语言中的类型      |

> 从 Python 3.4 开始，数组类型不再支持诸如 `list.sort()` 这种就地排序方法。
>
> 要给数组排序的话，得用 sorted 函数新建一个数组：
>
> `a = array.array(a.typecode, sorted(a))`
>
> 想要在不打乱次序的情况下为数组添加新的元素，`bisect.insort` 能排上用场。

#### 2.9.2 内存视图

`memoryview` 是一个内置类，它能让用户在不复制内容的情况下操作同一个数组的不同切片。

> 内存视图其实是泛化和去数学化的 NumPy 数组。它让你在不需要复制内容的前提下，在数据结构之间共享内存。其中数据结构可以是任何形式，比如 PIL 图片、SQLite数据库和 NumPy 的数组，等等。这个功能在处理大型数据集合的时候非常重要。

#### 2.9.3 NumPy和SciPy

凭借着 NumPy 和 SciPy 提供的高阶数组和矩阵操作，Python 称为科学计算应用的主流语言。

NumPy 实现了多维同质数组（homogeneous array）和矩阵，这些数据结构不但能处理数字，还能存放其他由用户定义的记录。通过 NumPy，用户能对这些数据结构里的元素进行高效的操作。

SciPy 是基于 NumPy 的另一个库，它提供了很多跟科学计算有关的算法，专为线性代数、数值积分和统计学而设计。SciPy 把基于 C 和 Fortran 的工业级数学计算功能用交互式且高度抽象的 Python 包装起来，让科学家如鱼得水。



用到时再专门学习学习

#### 2.9.4 双向队列和其他形式的队列

`collections.deque`类（双向队列）是一个线程安全、可以快速从两端添加或者删除元素的数据类型。





















# End