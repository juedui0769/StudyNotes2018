> 110_golang_学习笔记.md

<https://golang.google.cn/>

---------------------------------

### 1 旧笔记梳理

GO语言中文网: <https://studygolang.com/>

Go 安装包下载: <https://studygolang.com/dl>

各平台Go安装参考(包括`Mac`): <http://docs.studygolang.com/doc/install>

下载第三方包 : 参考 <https://gitee.com/juedui0769/MyDocs/blob/master/temp/temp990_golangtc_download_package.md>

gopm : 参考 <https://gitee.com/juedui0769/MyDocs/blob/master/temp/temp991_golang_gopm.md>

之前在慕课网学习`golang`时的笔记: <https://gitee.com/juedui0769/MyDocs/blob/master/temp/temp993_imooc_golang.md>

-----------------------------

### 2 再学golang

<https://golang.google.cn/> 这个应该是`golang`在国内的官网,首页比较干净！

<https://golang.google.cn/doc/install> 这是安装说明, 在"System requirements"可以看到各环境下的安装前置。比如, 在window下需要安装`MinGW`、`gcc`(如果计划使用`cgo`的话需要安装)

#### 2.1 MinGw

<http://www.mingw.org/> Minimalist GNU for Windows 

#### 2.2 windows下的下载和安装

在 <https://golang.google.cn/dl/> 下载`go1.12.5.windows-amd64.msi`文件。118MB，下载速度非常快(2019年5月11日18:04:01)。

运行`msi`文件,完成安装！一些必要的环境变量已经被设置了。在`cmd`运行`go help`已经可以被识别了。
- 我没有安装`MinGW`，可能是我的系统在安装`Git`时已经安装了。
- 或者是windows下的`msi`文件自带了
- ……
- 总之，我是跳过了`2.1 MinGw`的下载和安装了。如果在新系统上出现问题，记得检查是否缺失了`2.1`这一步。

#### 2.3 安装完成之后的设置

##### GOPATH

参考这个链接 : <https://golang.google.cn/doc/code.html#GOPATH>

`go help gopath` : 输出`GOPATH`相关的信息

`go env GOPATH` : 输出当前的`GOPATH`位置信息(默认是`%USERPROFILE%\go`, 用户目录下的"go"文件夹)。`GOPATH`必须大写.
- 可以指定到其他目录,比如,设置为: `D:\wxg104_Go`

注意`GOPATH`的目录结构，应该将自己的代码放在`%GOPATH%\src\`目录下

```
D:\wxg104_Go
|- src
    |- myhello
        |- hello.go
|- bin
```

> 应该将 `%GOPATH%\bin` 添加到环境变量中！这样就可以在任意位置运行编译成功的程序。
>
> 我这次修改的是"当前用户"的环境变量,没有修改"系统变量"

##### workspaces

<https://golang.google.cn/doc/code.html#Workspaces> , 在这个链接介绍了如何组织目录结构。

#### 2.4 helloworld

创建文件`D:\wxg104_Go\src\myhello\hello.go`, 代码如下:

```go
package main

import "fmt"

func main() {
    fmt.Printf("hello, world\n")
}
```

在当前目录运行`go build`，编译生成一个`myhello.exe`文件，运行此exe文件输出"hello, world"。

#### 2.5 go install

参考 : <https://golang.google.cn/doc/code.html#Command>

在"2.4"中，我使用`go build`命令，在当前目录下生成了`myhello.exe`文件。

如果使用`go install`命令，会在`%GOPATH%\bin`目录下生成`myhello.exe`文件。

如果是在`D:\wxg104_Go\src\myhello\`目录下可以直接运行`go install`命令，如果是在其他目录可以运行~~`go install myhello/hello`~~ `go install myhello`命令，Go会去`GOPATH`寻找`myhello`目录，并编译此目录下的`*.go`文件,将生成的`myhello.exe`放到`%GOPATH%\bin`目录下。

因为`%GOPATH%\bin`目录已经被我设置到当前用户的`path`环境变量下了。所以我可以在任意目录下运行`myhello`，这将会输出"hello, world! welcome to golang!"





















































