# 环境准备（1）

>  这是第一版的环境安装，基于Python2，有很多问题，建议在Python3上搭建环境！

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

## 安装总结：

先安装

```python
python -m pip install numpy scipy matplotlib==2.2.3 ipython==5.8 jupyter
```

再安装

```python
python -m pip install pandas sympy nose
```

jupyter，可能有问题，但是貌似不影响使用？！

2019年1月25日13:38:01，`pandas`要也要指定版本，按照书中的使用`0.8.2`版。

```python
python -c "import pandas"
pip uninstall pandas

Uninstalling pandas-0.23.4:
  Would remove:
    e:\python_env\py2\lib\site-packages\pandas-0.23.4.dist-info\*
    e:\python_env\py2\lib\site-packages\pandas\*
Proceed (y/n)? y
  Successfully uninstalled pandas-0.23.4

python -c "import pandas"

python -m pip install pandas==0.8.2

(py2) F:\wxg103\pythonProjects\forDataAnalysis\ch02>python -m pip install pandas==0.8.2
DEPRECATION: Python 2.7 will reach the end of its life on January 1st, 2020. Please upgrade your Pyt
hon as Python 2.7 won't be maintained after that date. A future version of pip will drop support for
 Python 2.7.
Looking in indexes: http://mirrors.aliyun.com/pypi/simple
Collecting pandas==0.8.2
  Could not find a version that satisfies the requirement pandas==0.8.2 (from versions: 0.1, 0.2b0,
0.2b1, 0.2, 0.3.0b0, 0.3.0b2, 0.3.0, 0.4.0, 0.4.1, 0.4.2, 0.4.3, 0.5.0, 0.6.0, 0.6.1, 0.7.0rc1, 0.7.
0, 0.7.1, 0.7.2, 0.7.3, 0.8.0rc1, 0.8.0rc2, 0.8.0, 0.8.1, 0.9.0, 0.9.1, 0.10.0, 0.10.1, 0.11.0, 0.12
.0, 0.13.0, 0.13.1, 0.14.0, 0.14.1, 0.15.0, 0.15.1, 0.15.2, 0.16.0, 0.16.1, 0.16.2, 0.17.0, 0.17.1,
0.18.0, 0.18.1, 0.19.0rc1, 0.19.0, 0.19.1, 0.19.2, 0.20.0rc1, 0.20.0, 0.20.1, 0.20.2, 0.20.3, 0.21.0
rc1, 0.21.0, 0.21.1, 0.22.0, 0.23.0rc2, 0.23.0, 0.23.1, 0.23.2, 0.23.3, 0.23.4, 0.24.0rc1)
No matching distribution found for pandas==0.8.2

```

没有 `0.8.2` 版本，不过根据提示，我决定使用 `0.9.0` 版本。



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





# Book1 : 利用Python进行数据分析

>  第一版

《利用Python进行数据分析》、《Python for Data Analysis》

Wes McKinney 著，唐学韬 等译，机械工业出版社，2013.9



## 第 2 章

**引言**

https://github.com/wesm/pydata-book ，本书代码位置，

- https://github.com/wesm/pydata-book/tree/1st-edition， 这是第一版的代码位置



# Book2 : 第二版

> https://www.jianshu.com/p/04d180d90a3f ，简书上的第二版中文译文

## 环境准备（2）

**虚拟环境，安装**

- 参考： https://github.com/juedui0769/StudyNotes2018/blob/master/123_Python_%E8%AE%B0%E5%BD%95.md ， 这个链接下有安装虚拟环境的记录。

```python
# 进入Python 3 的虚拟环境
workon py3
# 退出环境
deactivate
```

**更新pip（pip的安装参考网文）**

```python
python -m pip install --upgrade pip
```

**numpy**

- 以下代码复制自 https://scipy.org/install.html

```python
python -m pip install numpy scipy matplotlib ipython jupyter pandas sympy nose
```

运行上面代码后的输出比较多，下面是节选：

```python
Successfully built sympy mpmath pandocfilters prometheus-client
jupyter-console 6.0.0 has requirement prompt-toolkit<2.1.0,>=2.0.0, but you'll have prompt-toolkit 1
.0.15 which is incompatible.
Installing collected packages: numpy, scipy, cycler, python-dateutil, pyparsing, kiwisolver, matplot
lib, tornado, jupyter-core, pyzmq, jupyter-client, ipykernel, jupyter-console, jsonschema, nbformat,
 Send2Trash, prometheus-client, entrypoints, defusedxml, pandocfilters, testpath, MarkupSafe, jinja2
, webencodings, bleach, mistune, nbconvert, pywinpty, terminado, notebook, widgetsnbextension, ipywi
dgets, qtconsole, jupyter, pytz, pandas, mpmath, sympy, nose
Successfully installed MarkupSafe-1.1.0 Send2Trash-1.5.0 bleach-3.1.0 cycler-0.10.0 defusedxml-0.5.0
 entrypoints-0.3 ipykernel-5.1.0 ipywidgets-7.4.2 jinja2-2.10 jsonschema-2.6.0 jupyter-1.0.0 jupyter
-client-5.2.4 jupyter-console-6.0.0 jupyter-core-4.4.0 kiwisolver-1.0.1 matplotlib-3.0.2 mistune-0.8
.4 mpmath-1.1.0 nbconvert-5.4.0 nbformat-4.4.0 nose-1.3.7 notebook-5.7.4 numpy-1.16.0 pandas-0.23.4
pandocfilters-1.4.2 prometheus-client-0.5.0 pyparsing-2.3.1 python-dateutil-2.7.5 pytz-2018.9 pywinp
ty-0.5.5 pyzmq-17.1.2 qtconsole-4.4.3 scipy-1.2.0 sympy-1.3 terminado-0.8.1 testpath-0.4.2 tornado-5
.1.1 webencodings-0.5.1 widgetsnbextension-3.4.2
```

有折行，在控制台中下面这句是红色提示：

```python
jupyter-console 6.0.0 has requirement prompt-toolkit<2.1.0,>=2.0.0, but you'll have prompt-toolkit 1.0.15 which is incompatible.
```

**prompt-toolkit**

根据上面的提示，`prompt-toolkit` 需要更新，但是又不能大于 `2.1.0`，可是现在最新的版本是`2.0.7`，可以放心更新，证据如下：

- 使用`pip search prompt_toolkit`  或 https://pypi.org/project/prompt_toolkit/ ，得到的

另外，通过运行 `pip list --outdated` 也得到提示：

```python
(py3) F:\>pip list --outdated
Package        Version Latest Type
-------------- ------- ------ -----
colorama       0.3.9   0.4.1  wheel
cryptography   2.3.1   2.5    wheel
decorator      4.3.0   4.3.2  wheel
idna           2.7     2.8    wheel
ipython        6.5.0   7.2.0  wheel
jedi           0.12.1  0.13.2 wheel
parso          0.3.1   0.3.2  wheel
pickleshare    0.7.4   0.7.5  wheel
prompt-toolkit 1.0.15  2.0.7  wheel
psutil         5.4.8   5.5.0  wheel
Pygments       2.2.0   2.3.1  wheel
PyMySQL        0.9.2   0.9.3  wheel
setuptools     36.2.7  40.6.3 wheel
six            1.11.0  1.12.0 wheel
urllib3        1.23    1.24.1 wheel
wheel          0.29.0  0.32.3 wheel
```

使用 `pip install --upgrade prompt_toolkit` 可以更新，顺便将 IPython 也更新了（中间的红色警告信息就不贴出来了, ipython6.5.0 使用的 prompt_toolkit 不能超过 2.0.0，我现在安装的 2.0.7 已经导致不兼容）。

```python
pip install --upgrade prompt_toolkit
pip install --upgrade ipython
```

**当前版本整理如下（2019年1月25日15:10:47）：**

```python
numpy              1.16.0
pandas             0.23.4
scipy              1.2.0
matplotlib         3.0.2
ipython            7.2.0
ipython-genutils   0.2.0
jupyter            1.0.0
jupyter-client     5.2.4
jupyter-console    6.0.0
jupyter-core       4.4.0
sympy              1.3
nose               1.3.7

notebook           5.7.4
```

用到的pip命令整理如下：

```python
# 更新 pip 自身
python -m pip install --upgrade pip
# 检索
pip search ipython
# 安装
pip install ipython
# 更新已安装的类库
pip install --upgrade ipython
# 列出需要更新的类库
pip list --outdated
# 列出所有已安装的类库
pip list
```



## 第 1 章

https://www.jianshu.com/p/04d180d90a3f， 准备工作



## 第 2 章

https://www.jianshu.com/p/fc93e943e94a， Python语法基础，IPython和Jupyter Notebooks

### jupyter notebook

按照“环境准备（2）”中的方式安装好必要的python内库之后，就可以使用“jupyter notebook”了。

- `python -m pip install numpy scipy matplotlib ipython jupyter pandas sympy nose`
- 详细的安装过程，见上的“环境准备（2）”中的描述。

```python
(py3) F:\>jupyter notebook
[I 18:48:00.087 NotebookApp] Serving notebooks from local directory: F:\
[I 18:48:00.088 NotebookApp] The Jupyter Notebook is running at:
[I 18:48:00.088 NotebookApp] http://localhost:8888/?token=5a410b109967653b639abc7226be1416ae212fc764114ef0
[I 18:48:00.089 NotebookApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
[C 18:48:00.093 NotebookApp]

    To access the notebook, open this file in a browser:
        file:///C:/Users/Wxg/AppData/Roaming/jupyter/runtime/nbserver-8020-open.html
    Or copy and paste one of these URLs:
        http://localhost:8888/?token=5a410b109967653b639abc7226be1416ae212fc764114ef0
```

输入 `jupyter notebook` 后，会自动打开浏览器（详细信息见上面的代码）

### 总结

第二章就是对Python的简介，以及 IPython 和 jupyter notebook 的使用。我将第二章的阅读笔记（forDataAnalysis2\ch02\helloJupyter.ipynb）上传到 gitee 上了。

## 第 3 章

https://www.jianshu.com/p/b444cda10aa0 ， Python的数据结构、函数和文件







































# End