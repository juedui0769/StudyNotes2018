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

### 2 学习

2019年5月11日10:15:53， 阅读完《Dart IN ACTION》第一章，值得注意的几个地方：
- 只有一个`false`值, `0`等不是false值。
- `Dart`是单线程的语言,但可以使用`isolate`来开发'并发'程序，各`isolate`之间是不共享内存的，通过"消息"来通讯
- 类型是可选的。 `var`, `final` 或者 `String`, `List`, `int`
- 函数是一等公民
- `import`, `part` 可以引入其他的`*.dart`文件，`library`是定义当前文件为一个library，类似Java中的`package`
- ...








