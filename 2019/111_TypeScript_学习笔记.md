> 111_TypeScript_学习笔记.md

## 1 资源

<http://www.typescriptlang.org/>

<https://www.tslang.cn/> : `TypeScript`中文网

## 2 安装

先要获取`Node.js`然后使用npm安装`TypeScript`

### 2.1 Node.js

<https://nodejs.org/en/>

<http://nodejs.cn/> : `Node.js`中文网

在`Node.js`中文网下载即可，速度很快! 16.3MB(node-v10.15.3-x64.msi,2019年5月11日20:57:02)

安装在`C:\Program Files\nodejs\`目录下, msi包含"Node.js runtime", "npm package manager", "Online documentation shortcuts", "Add to PATH"

```
node --help
node --version

npm --help
npm version
```

### 2.2 cnpm

在国内`cnpm`是必安装的。 参考 <https://npm.taobao.org/package/cnpm>

```
npm install cnpm -g --registry=https://registry.npm.taobao.org
```

```
cnpm --help

cnpm --version
```

### 2.3 安装`TypeScript`

```
cnpm install -g typescript
```

```
tsc --help
tsc --version
```

## 3 文档

### 3.1 5分钟上手

<https://www.tslang.cn/docs/handbook/typescript-in-5-minutes.html>

上手教程，将一个`JS`程序改造成`TypeScript`程序，TypeScript提供了增强类型，可以在编译提供类型检查，帮助开发者排除编译问题。这教程内容比较简单，欲看原文移步链接。

### 3.2 基础类型

<https://www.tslang.cn/docs/handbook/basic-types.html>

#### 枚举

```TypeScript
enum Color {Red, Green, Blue}
let c: Color = Color.Green;

console.log(Color)
```

`tsc TsBasic.ts`编译得出`TsBasic.js`, 运行`node TsBasic.js`输出如下内容:

```
{ '0': 'Red', '1': 'Green', '2': 'Blue', Red: 0, Green: 1, Blue: 2 }
```

来看看编译出来的`js`文件是什么样子的:

```js
var Color;
(function (Color) {
    Color[Color["Red"] = 0] = "Red";
    Color[Color["Green"] = 1] = "Green";
    Color[Color["Blue"] = 2] = "Blue";
})(Color || (Color = {}));

console.log(Color);
```

#### 字符串

在TypeScript中可以使用反引号( &#96; )来定义多行文本，支持内嵌表达式。

```TypeScript
let name: string = `Gene`;
let age: number = 37;
let sentence: string = `Hello, my name is ${name}.
I'll be ${age+1} years old next month.`;
```

> ## 如何在MarkDown文件中输出特殊字符？
> 比如输出反引号 **&#96;** ，可以写一段程序来帮助我们
> ```
> var str = "`";
> console.log(str.charCodeAt(0));
> console.log("&#" + str.charCodeAt(0) + ";");
> ```
> 上面代码的输出结果为 `96`, `&#96;`
> 
> `&#96;`就是 &#96; 的表示，其他的特殊字符也可以使用这种方式来确认！
> 
> `String.charCodeAt(index:number)` 可以参考MDN文档: <https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/charCodeAt>

#### 其他

`元组`, `any`, `void`, `undefined`, `null`, `never`, `object`

这些都比较好理解，看一遍文档就明白了，忘记了，前往文档复习巩固一下即可！


### 3.3 变量声明

<https://www.tslang.cn/docs/handbook/variable-declarations.html>

-------------------------

> 看基础很无聊…… 要仔细看完并看懂不容易，我是个心急的人，等不到看完基础，还是觉得边实战边查字典是最适合我的方式。
> 
> 所以选择从这个链接中 <https://www.tslang.cn/samples/index.html> 选择一个实战。

## 4 实战

### 4.1 TypeScript React Starter

<https://github.com/Microsoft/TypeScript-React-Starter#typescript-react-starter>






















