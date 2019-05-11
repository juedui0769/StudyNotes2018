> 109_Dart_Google_编程语言.md

### 1 安装

我是在 <http://dart.goodev.org/install/windows> 使用第三方安装方式安装的，下载非常慢。
- 如果再安装的话，我将选择`zip`方式安装。
- 并且`zip`方式也方便共享给他人。

没有更改安装路径, 默认是安装在`C:\Program Files\Dart`目录下的。
- 此目录下`Dart Update.exe`可以方便更新

```
C:\Program Files\Dart
|- dark-sdk
    |- bin
    |- include
    |- lib
    |- dartdoc_options.yaml
    |- LICENSE
    |- README
    |- revision
    |- version
|- Dart Update.exe
|- dart-icon.ico
|- unins000.dat
|- unins000.exe
```

将`C:\Program Files\Dart\dart-sdk\bin`设置到`PATH`环境变量中,就可以在命令行中运行`dart`命令了。

`dart --version` 查看版本。

### 2 初识`Dart`

2019年5月11日10:15:53， 阅读完《Dart IN ACTION》第一章，值得注意的几个地方：
- 只有一个`false`值, `0`等不是false值。
- `Dart`是单线程的语言,但可以使用`isolate`来开发'并发'程序，各`isolate`之间是不共享内存的，通过"消息"来通讯
- 类型是可选的。 `var`, `final` 或者 `String`, `List`, `int`
- 函数是一等公民
- `import`, `part` 可以引入其他的`*.dart`文件，`library`是定义当前文件为一个library，类似Java中的`package`
- ...


#### 2.1 helloworld

编写一个`hellodart.dart`的文本文件,输入如下内容:

```dart
/* 注释和Java差不多, 也可以使用三个斜杠 */

// 写法一: 一行表达
/// main() => print("Hello World");

// 写法二: 
main() {
	print("Hello Dart!");
}
```

执行`dart hellodart.dart`可以在控制台看到输出结果。(我是在windows下运行的,将dart的bin添加到环境变量中)

> 2019年5月11日16:15:17 -> 突然觉得我更应该去看《Java并发实战》、JVM、mybatis(面试时被问倒)、oracle(工作需要)、事务(面试没答对)、或者TypeScript(正火)、nodejs、webpack、或者Golang……
> 
> Flutter -> 是基于`Dart`的。 好纠结啊！
> <https://juejin.im/post/5cd397ece51d453ae21957c4> 这篇文章的作者说: "铁打的基础，流水的API"，不要花太多的精力沉迷在框架中，把编程的基础技术学好才是正道。如果公司想尝试`Flutter`，专门花一周两周突击一下，肯定能上手。









