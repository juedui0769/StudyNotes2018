# Dubbo

- https://github.com/apache/incubator-dubbo
- http://dubbo.apache.org/zh-cn/docs/user/quick-start.html

## Getting started

### dubbo-samples-api

按照[dubbo|github官网](https://github.com/apache/incubator-dubbo)上的Getting started，克隆官方样例。

```shell
# git clone https://github.com/dubbo/dubbo-samples.git
```

我是克隆到本地`F:\wxg118_dubbo\dubbo-samples`下，

然后，启动`IDEA`将整个工程导入。

![](F:\Docs\101_Me\imgs\113_dubbo_001.png)

子模块很多，`dubbo-samples-api`就是官方推荐的第一个样例。

按照官方的说法，在命令行中输入一下命令启动测试：

```shell
mvn -Djava.net.preferIPv4Stack=true -Dexec.mainClass=org.apache.dubbo.samples.provider.Application exec:java

mvn -Djava.net.preferIPv4Stack=true -Dexec.mainClass=org.apache.dubbo.samples.consumer.Application exec:java
```

上面的命令是启动`provider`，下面的是启动`consumer`，`provider`先启动，它会一直运行，等待`consumer`的连接。

因为我已经导入到`IDEA`中了，所以直接在`IDEA`中启动就可以看到测试结果了（没有使用`-Djava.net.preferIPv4Stack=true`，可能有些版本的IDE需要添加这个，我测试时没有添加这个参数）。

#### 简单分析

![](F:\Docs\101_Me\imgs\113_dubbo_002_samples_api.png)

如上图，`dubbo-samples-api`有三个包`api`，`provider`，`consumer`，`GreetingsService`是接口，`GreetingsServiceImpl`是实现，两个`Application`分别是`provider`和`consumer`的启动类。

1） 先看看`provider`中`Application`类的代码：

```java
public class Application {
    public static void main(String[] args) throws IOException {
        ServiceConfig<GreetingsService> service = new ServiceConfig<>();
        service.setApplication(new ApplicationConfig("first-dubbo-provider"));
        service.setRegistry(new RegistryConfig("multicast://224.5.6.7:1234"));
        service.setInterface(GreetingsService.class);
        service.setRef(new GreetingsServiceImpl());
        service.export();
        System.out.println("first-dubbo-provider is running.");
        System.in.read();
    }
}
```

2）ServiceConfig

![](F:\Docs\101_Me\imgs\113_dubbo_003_ServiceConfig.png)

`ServiceConfig`位于`com.alibaba.dubbo.config`包下，它有一系列的抽象父类，都位于`com.alibaba.dubbo.config`包下，如上图。

3） 首先来看看位于最顶层的`AbstractConfig`，方法很多看得头疼（慢慢研究吧），先贴上看得到的重要属性，下面这个表格中的属性，应该是新旧版本`Properties`的映射。

| 旧                     | 新                                 |
| ---------------------- | ---------------------------------- |
| dubbo.protocol.name    | dubbo.service.protocol             |
| dubbo.protocol.host    | dubbo.service.server.host          |
| dubbo.protocol.port    | dubbo.service.server.port          |
| dubbo.protocol.threads | dubbo.service.max.thread.pool.size |
| dubbo.consumer.timeout | dubbo.service.invoke.timeout       |
| dubbo.consumer.retries | dubbo.service.max.retry.providers  |
| dubbo.consumer.check   | dubbo.service.allow.no.provider    |
| dubbo.service.url      | dubbo.service.address              |

4） `AbstractMethodConfig`

| 属性名      | 描述                                                         |
| ----------- | ------------------------------------------------------------ |
| timeout     | timeout for remote invocation in milliseconds                |
| retries     | retry times                                                  |
| actives     | max concurrent invocations                                   |
| loadbalance | load balance                                                 |
| async       | whether to async                                             |
| sent        | whether to ack async-sent                                    |
| mock        | the name of mock class which gets called when a service fails to execute |
| merger      | merger                                                       |
| cache       | cache                                                        |
| validation  | validation                                                   |

```java
public void setLoadbalance(String loadbalance) {
	checkExtension(LoadBalance.class, "loadbalance", loadbalance);
	this.loadbalance = loadbalance;
}

@Parameter(escaped = true)
public String getMock() {
	return mock;
}

public void setMock(Boolean mock) {
	if (mock == null) {
		setMock((String) null);
	} else {
		setMock(String.valueOf(mock));
	}
}

public void setMock(String mock) {
	if (mock != null && mock.startsWith(Constants.RETURN_PREFIX)) {
		checkLength("mock", mock);
	} else {
		checkName("mock", mock);
	}
	this.mock = mock;
}

```

其他属性是正常的getter、setter，上面是添加了“调料”的getter、setter。

5） `AbstractInterfaceConfig`

| 属性                              | 描述                                                         |
| --------------------------------- | ------------------------------------------------------------ |
| String local                      | local impl class name for the service interface              |
| String stub                       | local stub class name for the service interface              |
| MonitorConfig monitor             | service monitor                                              |
| String proxy                      | proxy type                                                   |
| String cluster                    | cluster type                                                 |
| String filter                     | filter                                                       |
| String listener                   | listener                                                     |
| String owner                      | owner                                                        |
| Integer connections               | connection limits, 0 means shared connection, otherwise it defines the connections delegated to the current service |
| String layer                      | layer                                                        |
| ApplicationConfig application     | application info                                             |
| ModuleConfig module               | module info                                                  |
| `List<RegistryConfig> registries` | registry centers                                             |
| String onconnect                  | connection events                                            |
| String ondisconnect               | disconnection events                                         |
| Integer callbacks                 | callback limits                                              |
| String scope                      | the scope for referring/exporting a service, if it's local, it means searching in current JVM only. |

`AbstractInterfaceConfig`包含一些重要的逻辑，稍后慢慢品尝！

6） `AbstractServiceConfig`

| 属性                             | 描述                                                         |
| -------------------------------- | ------------------------------------------------------------ |
| String version                   | version                                                      |
| String group                     | group                                                        |
| Boolean deprecated               | whether the service is deprecated                            |
| Integer delay                    | delay service exporting                                      |
| Boolean export                   | whether to export the service                                |
| Integer weight                   | weight                                                       |
| String document                  | document center                                              |
| Boolean dynamic                  | whether to register as a dynamic service or not on register center |
| String token                     | whether to use token                                         |
| String accesslog                 | access log                                                   |
| `List<ProtocolConfig> protocols` |                                                              |
| Integer executes                 | max allowed execute times                                    |
| Boolean register                 | whether to register                                          |
| Integer warmup                   | warm up period                                               |
| String serialization             | serialization                                                |

7） 再来看看`ServiceConfig`









## 源码摘录

### AbstractConfig

com.alibaba.dubbo.config.AbstractConfig

```java
static {
	legacyProperties.put("dubbo.protocol.name", "dubbo.service.protocol");
	legacyProperties.put("dubbo.protocol.host", "dubbo.service.server.host");
	legacyProperties.put("dubbo.protocol.port", "dubbo.service.server.port");
	legacyProperties.put("dubbo.protocol.threads", "dubbo.service.max.thread.pool.size");
	legacyProperties.put("dubbo.consumer.timeout", "dubbo.service.invoke.timeout");
	legacyProperties.put("dubbo.consumer.retries", "dubbo.service.max.retry.providers");
	legacyProperties.put("dubbo.consumer.check", "dubbo.service.allow.no.provider");
	legacyProperties.put("dubbo.service.url", "dubbo.service.address");

	// this is only for compatibility
	Runtime.getRuntime().addShutdownHook(DubboShutdownHook.getDubboShutdownHook());
}

```

| 旧                     | 新                                 |
| ---------------------- | ---------------------------------- |
| dubbo.protocol.name    | dubbo.service.protocol             |
| dubbo.protocol.host    | dubbo.service.server.host          |
| dubbo.protocol.port    | dubbo.service.server.port          |
| dubbo.protocol.threads | dubbo.service.max.thread.pool.size |
| dubbo.consumer.timeout | dubbo.service.invoke.timeout       |
| dubbo.consumer.retries | dubbo.service.max.retry.providers  |
| dubbo.consumer.check   | dubbo.service.allow.no.provider    |
| dubbo.service.url      | dubbo.service.address              |

### ExtensionLoader

```java
private static <T> boolean withExtensionAnnotation(Class<T> type) {
	return type.isAnnotationPresent(SPI.class);
}
```

```java
if (!type.isInterface()) {
	throw new IllegalArgumentException("Extension type(" + type + ") is not interface!");
}

if (!withExtensionAnnotation(type)) {
	throw new IllegalArgumentException("Extension type(" + type +
			") is not extension, because WITHOUT @" + SPI.class.getSimpleName() + " Annotation!");
}
```

如果不是接口，并且没有被`@SPI`标注，就不能被`ExtensionLoader`的`getExtensionLoader()`调用。

这个类有点复杂，不太好理解。特别是`createAdaptiveExtensionClassCode()`方法，这个方法将为接口生成其对应的子类（？）

META-INF

- `META-INF/services/`
- `META-INF/dubbo/`
- `META-INF/dubbo/internal/`

在`dubbo-2.6.4.jar`中可以找到`META-INF/dubbo/internal/`，内有`31`个文件，我快速浏览了一遍它们的内容，感兴趣的类通过`IDEA`快速定位到并查看了代码内容，有一些感悟，但，需要加深加强（需要更多时间）。

### Constants

`com.alibaba.dubbo.common.Constants`

定义常量的类，……








# End