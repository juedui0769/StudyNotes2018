# 环境准备

## NumPy

https://pypi.org/project/numpy/ ， https://www.numpy.org/， https://docs.scipy.org/doc/numpy/user/install.html， 上面三个网址，一路寻觅，最终在下面的链接中，找到安装的方法：

- https://scipy.org/install.html

第一版是针对 Python 2 的，所以，我切入到 python 2.7 环境，如下：

```python
workon py2

python -m pip install --user numpy scipy matplotlib ipython jupyter pandas sympy nose

(py2) F:\>python -m pip install --user numpy scipy matplotlib ipython jupyter pandas sympy nose
Can not perform a '--user' install. User site-packages are not visible in this virtualenv.
You are using pip version 18.0, however version 19.0.1 is available.
You should consider upgrading via the 'python -m pip install --upgrade pip' command.
```

控制台输出如上所示，`--user` 参数似乎有问题。

```python
    Complete output from command python setup.py egg_info:

    Matplotlib 3.0+ does not support Python 2.x, 3.0, 3.1, 3.2, 3.3, or 3.4.
    Beginning with Matplotlib 3.0, Python 3.5 and above is required.

    This may be due to an out of date pip.

    Make sure you have pip >= 9.0.1.


    ----------------------------------------
Command "python setup.py egg_info" failed with error code 1 in c:\users\wxg\appdata\local\temp\pip-i
nstall-qfq851\matplotlib\
You are using pip version 18.0, however version 19.0.1 is available.
You should consider upgrading via the 'python -m pip install --upgrade pip' command.
```

如上，`Matplotlib` 似乎要指定版本再安装，默认版本是需要Python 3.5及以上环境支持的。

```python
(py2) F:\>python -m pip install matplotlib==2.2.3
    
Requirement already satisfied: setuptools in e:\python_env\py2\lib\site-packages (from kiwisolver>=1
.0.1->matplotlib==2.2.3) (36.2.0)
Installing collected packages: backports.functools-lru-cache, six, pytz, cycler, kiwisolver, python-
dateutil, pyparsing, numpy, matplotlib
Successfully installed backports.functools-lru-cache-1.5 cycler-0.10.0 kiwisolver-1.0.1 matplotlib-2
.2.3 numpy-1.16.0 pyparsing-2.3.1 python-dateutil-2.7.5 pytz-2018.9 six-1.12.0
You are using pip version 18.0, however version 19.0.1 is available.
You should consider upgrading via the 'python -m pip install --upgrade pip' command.
```

如上，指定版本安装： `pip install matplotlib==2.2.3` ， `pip install applicationName==version`

- 参考： https://matplotlib.org/

IPython的安装也要指定版本，

- 参考： https://github.com/ipython/ipython ， **IPython 5.x LTS** is the compatible release for Python 2.7. If you require Python 2 support, you **must** use IPython 5.x LTS. 
- 官网： http://ipython.org/ ， IPython 5.8: minor bugfixes (July 28, 2018)

```python
(py2) F:\>python -m pip install ipython==5.8
    
Successfully built simplegeneric win-unicode-console
Installing collected packages: pygments, backports.shutil-get-terminal-size, simplegeneric, enum34,
decorator, ipython-genutils, traitlets, scandir, pathlib2, wcwidth, prompt-toolkit, win-unicode-cons
ole, colorama, pickleshare, ipython
Successfully installed backports.shutil-get-terminal-size-1.0.0 colorama-0.4.1 decorator-4.3.0 enum3
4-1.1.6 ipython-5.8.0 ipython-genutils-0.2.0 pathlib2-2.3.3 pickleshare-0.7.5 prompt-toolkit-1.0.15
pygments-2.3.1 scandir-1.9.0 simplegeneric-0.8.1 traitlets-4.3.2 wcwidth-0.1.7 win-unicode-console-0
.5
You are using pip version 18.0, however version 19.0.1 is available.
You should consider upgrading via the 'python -m pip install --upgrade pip' command.
```

如上，ipython指定版本后，就可以正确安装了。但是，jupyter 的安装是个麻烦，我的安装总是爆红，但是可以运行，我不确定是在python 3下安装成功过，还是什么其他原因的影响，总之 `(py2) F:\>jupyter notebook` 是可以启动的。

```python
python -m pip install jupyter
(py2) F:\>jupyter notebook
```

因为，jupyter安装报红，后面的必须重新安装一下，

```python
python -m pip install pandas sympy nose


Requirement already satisfied: six>=1.5 in e:\python_env\py2\lib\site-packages (from python-dateutil
>=2.5.0->pandas) (1.12.0)
Building wheels for collected packages: sympy, mpmath
  Building wheel for sympy (setup.py) ... done
  Stored in directory: C:\Users\Wxg\AppData\Local\pip\Cache\wheels\80\4a\4d\e32c3a23146e3041f20d8a4c
92978bc966f45d9a0044c91588
  Building wheel for mpmath (setup.py) ... done
  Stored in directory: C:\Users\Wxg\AppData\Local\pip\Cache\wheels\63\63\95\9c762029f09a698f157c3ea8
a9869f98bc2f90ad14a3d5c74a
Successfully built sympy mpmath
Installing collected packages: pandas, mpmath, sympy, nose
Successfully installed mpmath-1.1.0 nose-1.3.7 pandas-0.23.4 sympy-1.3
```

### 安装总结：

先安装

```python
python -m pip install numpy scipy matplotlib==2.2.3 ipython==5.8 jupyter
```

再安装

```python
python -m pip install pandas sympy nose
```

jupyter，可能有问题，但是貌似不影响使用？！

## IPython进入pylab模式

```python
In [32]: %pylab
Using matplotlib backend: TkAgg
Populating the interactive namespace from numpy and matplotlib
e:\python_env\py2\lib\site-packages\IPython\core\magics\pylab.py:161: UserWarning: pylab import has
clobbered these variables: ['rec']
`%matplotlib` prevents importing * from pylab and numpy
  "\n`%matplotlib` prevents importing * from pylab and numpy"

```





# 利用Python进行数据分析

>  第一版

《利用Python进行数据分析》、《Python for Data Analysis》

Wes McKinney 著，唐学韬 等译，机械工业出版社，2013.9



## 第 2 章

**引言**

https://github.com/wesm/pydata-book ，本书代码位置，

- https://github.com/wesm/pydata-book/tree/1st-edition， 这是第一版的代码位置









































# End