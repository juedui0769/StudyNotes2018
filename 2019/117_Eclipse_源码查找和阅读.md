> 117_Eclipse_源码查找和阅读.md

## 1

在百度搜索到这篇文章 <https://blog.csdn.net/YLD10/article/details/80754052> ，文中给出链接 <https://archive.eclipse.org/eclipse/downloads/> 指向Eclipse各版本下载地址。

> 其实我不懂， SDK即源码吗？

## 2 Eclipse 4.8.0 Release Build: 4.8

承上 <https://archive.eclipse.org/eclipse/downloads/drops4/R-4.8-201806110500/> 打开这个链接，来到`4.8`版本的主页。
- 在此链接下的 "Source Tarball" , "eclipse-platform-sources-4.8.tar.xz" 就是源码
- 我已经下载了一份 : G:\迅雷下载\eclipse-platform-sources-4.8.tar.xz
- 并解压到 : D:\wxg\eclipse-platform-sources-I20180611-0500

页面上的信息很多，可以到 <https://archive.eclipse.org/eclipse/downloads/drops4/R-4.8-201806110500/details.html#Repository> 这个链接上查询本页中的信息都代表什么意思，比如：
- Eclipse Repository
- Eclipse SDK
- Platform SDK
- JDT SDK


### 2.1 Logs and Test Links

<https://archive.eclipse.org/eclipse/downloads/drops4/R-4.8-201806110500/testResults.php>

这个页面可以看到eclipse的一些组件? 能进入到构建和测试的应该是重要的组件吧！

`search.tests` 我很感兴趣，打开对应的win版本下的测试`XML` -> <https://archive.eclipse.org/eclipse/downloads/drops4/R-4.8-201806110500/testresults/html/org.eclipse.search.tests_ep48I-unit-cen64-gtk3_linux.gtk.x86_64_8.0.html>

> 只能大概浏览一下，并不知道各个测试的用意。

### 2.2 Summary of Unit Tests Results

这个标题下列出的是测试通过率，比如win64下的：失败`4`,通过`102362`,测试话费的时间`17658.363`s (约4.91小时)









--------------------

## other

### out of memory error

导入Eclipse源码时，出现 "out of memory error" 异常。 Eclipse弹出的异常窗口内容如下：

> **Internal Error**
> 
> An out of memory error has occurred. Consult the "Running Eclipse" section of the read me file for information on preventing this kind of error in the future.
> You are recommended to exit the workbench.
Subsequent errors may happen and may terminate the workbench without warning.
> 
> See the .log file for more details.
>
> Do you want to exit the workbench?

出现内存不足错误。请参阅ReadMe文件的“RuningEclipse”部分，了解有关防止将来发生此类错误的信息。
建议您退出工作台。
随后的错误可能会发生，并可能在没有警告的情况下终止工作台。
有关详细信息，请参见.log文件。

是否要退出工作台？

可以查看Eclipse安装目录下的readme目录下的"readme_eclipse.html"文件中的 "Running Eclipse" 一节来了解详情。

[The Eclipse runtime options](https://help.eclipse.org/mars/index.jsp?topic=/org.eclipse.platform.doc.isv/reference/misc/runtime-options.html)

#### eclipse.ini

实际上修改`eclipse.ini`文件是最便利的。

```
-startup
plugins/org.eclipse.equinox.launcher_1.3.0.v20140415-2008.jar
--launcher.library
plugins/org.eclipse.equinox.launcher.win32.win32.x86_64_1.1.200.v20140603-1326
-product
org.eclipse.epp.package.jee.product
--launcher.defaultAction
openFile
--launcher.XXMaxPermSize
256M
-showsplash
org.eclipse.platform
--launcher.XXMaxPermSize
256m
--launcher.defaultAction
openFile
--launcher.appendVmargs
-vmargs
-Dosgi.requiredJavaVersion=1.6
-Xms40m
-Xmx512m
```

参考此链接 <https://wiki.eclipse.org/Eclipse.ini>， 这里讲解得比较详细，值得阅读！

**[FAQ How do I increase the heap size available to Eclipse?](https://wiki.eclipse.org/FAQ_How_do_I_increase_the_heap_size_available_to_Eclipse%3F)**

```
-startup
plugins/org.eclipse.equinox.launcher_1.3.0.v20140415-2008.jar
--launcher.library
plugins/org.eclipse.equinox.launcher.win32.win32.x86_64_1.1.200.v20140603-1326
-product
org.eclipse.epp.package.jee.product
--launcher.defaultAction
openFile
--launcher.XXMaxPermSize
256M
-showsplash
org.eclipse.platform
--launcher.XXMaxPermSize
256m
--launcher.defaultAction
openFile
--launcher.appendVmargs
-vm
C:\Java\jdk1.8.0_191\bin\javaw.exe
-vmargs
-Dosgi.requiredJavaVersion=1.6
-Xms512m
-Xmx1024m
-XX:+UseParallelGC
-XX:PermSize=256M
-XX:MaxPermSize=512M
```

参考上面的, 修改的如下:

```
-vm
C:\Java\jdk1.8.0_191\bin\javaw.exe
-vmargs
-Dosgi.requiredJavaVersion=1.6
-Xms512m
-Xmx1024m
-XX:+UseParallelGC
-XX:PermSize=256M
-XX:MaxPermSize=512M
```

**[FAQ How do I increase the permgen size available to Eclipse?](https://wiki.eclipse.org/FAQ_How_do_I_increase_the_permgen_size_available_to_Eclipse%3F)**

Note: Oracle Java 8 does not have a separate permanent generation space any more. The -XX:(Max)PermSize option makes no difference (the JVM will ignore it, so it can still be present). -> 注意：Oracle Java 8不再有单独的永久生成空间。-XX：(Max)PermSize选项没有区别(JVM将忽略它，因此它仍然存在)。























