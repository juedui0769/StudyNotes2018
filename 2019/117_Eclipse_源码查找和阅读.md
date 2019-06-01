> 117_Eclipse_源码查找和阅读.md

## 1

在百度搜索到这篇文章 <https://blog.csdn.net/YLD10/article/details/80754052> ，文中给出链接 <https://archive.eclipse.org/eclipse/downloads/> 指向Eclipse各版本下载地址。

> 其实我不懂， SDK即源码吗？

## 2 Eclipse 4.8.0 Release Build: 4.8

承上 <https://archive.eclipse.org/eclipse/downloads/drops4/R-4.8-201806110500/> 打开这个链接，来到`4.8`版本的主页。

信息很多，首先看看"Logs and Test Links"

### 2.1 Logs and Test Links

<https://archive.eclipse.org/eclipse/downloads/drops4/R-4.8-201806110500/testResults.php>

这个页面可以看到eclipse的一些组件? 能进入到构建和测试的应该是重要的组件吧！

`search.tests` 我很感兴趣，打开对应的win版本下的测试`XML` -> <https://archive.eclipse.org/eclipse/downloads/drops4/R-4.8-201806110500/testresults/html/org.eclipse.search.tests_ep48I-unit-cen64-gtk3_linux.gtk.x86_64_8.0.html>

> 只能大概浏览一下，并不知道各个测试的用意。

### 2.2 Summary of Unit Tests Results

这个标题下列出的是测试通过率，比如win64下的：失败`4`,通过`102362`,测试话费的时间`17658.363`s (约4.91小时)







