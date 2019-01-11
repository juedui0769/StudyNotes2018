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

### dunder-method

![](./imgs/123_Python_dunder_method.png)

第1章 Python数据模型，开篇讲解的就是 dunder method。





# End