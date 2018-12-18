# Dubbo

开始： http://dubbo.apache.org/zh-cn/blog/dubbo-101.html

- https://github.com/apache/incubator-dubbo
- http://dubbo.apache.org/zh-cn/docs/user/quick-start.html

# Java RMI

摘抄： From http://dubbo.apache.org/zh-cn/blog/dubbo-101.html

（Remote Method Invocation）远程方法调用

Java RMI 是 Java 领域创建分布式应用的技术基石。后续的 EJB 技术，以及现代的分布式服务框架，其中的基本理念依旧是 Java RMI 的延续。



# start.dubbo.io

http://start.dubbo.io/

在 http://dubbo.apache.org/zh-cn/blog/dubbo-101.html 页面有中文介绍。

# Dubbo控制台

https://github.com/apache/incubator-dubbo-ops/blob/develop/README_ZH.md



# 快速启动

http://dubbo.apache.org/zh-cn/docs/user/quick-start.html

这个页面上的例子来源于 `git clone https://github.com/apache/incubator-dubbo.git`

但是，`pom.xml`需要调整！



## dubbo:protocol

schema配置参考，对应的官网链接： <http://dubbo.apache.org/zh-cn/docs/user/references/xml/dubbo-protocol.html>

因为，官网上`列`太多，导致出现横向滚动条，我把官网的复制过来，去除了一些列，完整的请参考上面的链接。

✓ `server`值比较重要，我在使用`com.alibaba:dubbo-config-spring:2.6.5`时，出现一个问题，`Constants.DEFAULT_REMOTING_SERVER = "netty"`，但是在debug时，发现代码中使用的是`netty4`，这是一个坑点！

- 按照以上方式修改后，provider，可以启动成功，但是，consumer，端却报错。
- 然后，在网上搜索到两篇博文，有介绍，如何修正！
  - https://blog.csdn.net/sunshine960125/article/details/82707349
    - 这篇博文采用的方式是修改`pom.xml`，不使用`com.alibaba:dubbo-remoting-netty4`，而使用`com.alibaba:dubbo-remoting-netty`。
  - https://blog.csdn.net/shangrilach/article/details/82470095
    - 这篇博文使用的方式更好，使用下面的方式，修改就OK了。
    - provider，端，`<dubbo:provider server="netty4" />`
    - consumer，端，`<dubbo:consumer client="netty4" />`
- http://dubbo.apache.org/zh-cn/docs/user/demos/netty4.html
  - dubbo 2.5.6版本新增了对netty4通信模块的支持

| 属性          | 类型           | 缺省值                                                       | 描述                                                         | 兼容性         |
| ------------- | -------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | -------------- |
| id            | string         | dubbo                                                        | 协议BeanId，可以在<dubbo:service protocol="">中引用此ID，如果ID不填，缺省和name属性值一样，重复则在name后加序号。 | 2.0.5以上版本  |
| name          | string         | dubbo                                                        | 协议名称                                                     | 2.0.5以上版本  |
| port          | int            | dubbo协议缺省端口为20880，rmi协议缺省端口为1099，http和hessian协议缺省端口为80；如果**没有**配置port，则自动采用默认端口，如果配置为**-1**，则会分配一个没有被占用的端口。Dubbo 2.4.0+，分配的端口在协议缺省端口的基础上增长，确保端口段可控。 | 服务端口                                                     | 2.0.5以上版本  |
| host          | string         | 自动查找本机IP                                               | -服务主机名，多网卡选择或指定VIP及域名时使用，为空则自动查找本机IP，-建议不要配置，让Dubbo自动获取本机IP | 2.0.5以上版本  |
| threadpool    | string         | fixed                                                        | 线程池类型，可选：fixed/cached                               | 2.0.5以上版本  |
| threads       | int            | 200                                                          | 服务线程池大小(固定大小)                                     | 2.0.5以上版本  |
| iothreads     | int            | cpu个数+1                                                    | io线程池大小(固定大小)                                       | 2.0.5以上版本  |
| accepts       | int            | 0                                                            | 服务提供方最大可接受连接数                                   | 2.0.5以上版本  |
| payload       | int            | 8388608(=8M)                                                 | 请求及响应数据包大小限制，单位：字节                         | 2.0.5以上版本  |
| codec         | string         | dubbo                                                        | 协议编码方式                                                 | 2.0.5以上版本  |
| serialization | string         | dubbo协议缺省为hessian2，rmi协议缺省为java，http协议缺省为json | 协议序列化方式，当协议支持多种序列化方式时使用，比如：dubbo协议的dubbo,hessian2,java,compactedjava，以及http协议的json等 | 2.0.5以上版本  |
| accesslog     | string/boolean |                                                              | 设为true，将向logger中输出访问日志，也可填写访问日志文件路径，直接把访问日志输出到指定文件 | 2.0.5以上版本  |
| path          | string         |                                                              | 提供者上下文路径，为服务path的前缀                           | 2.0.5以上版本  |
| transporter   | string         | dubbo协议缺省为netty                                         | 协议的服务端和客户端实现类型，比如：dubbo协议的mina,netty等，可以分拆为server和client配置 | 2.0.5以上版本  |
| server        | string         | dubbo协议缺省为netty，http协议缺省为servlet                  | 协议的服务器端实现类型，比如：dubbo协议的mina,netty等，http协议的jetty,servlet等 | 2.0.5以上版本  |
| client        | string         | dubbo协议缺省为netty                                         | 协议的客户端实现类型，比如：dubbo协议的mina,netty等          | 2.0.5以上版本  |
| dispatcher    | string         | dubbo协议缺省为all                                           | 协议的消息派发方式，用于指定线程模型，比如：dubbo协议的all, direct, message, execution, connection等 | 2.1.0以上版本  |
| queues        | int            | 0                                                            | 线程池队列大小，当线程池满时，排队等待执行的队列大小，建议不要设置，当线程程池时应立即失败，重试其它服务提供机器，而不是排队，除非有特殊需求。 | 2.0.5以上版本  |
| charset       | string         | UTF-8                                                        | 序列化编码                                                   | 2.0.5以上版本  |
| buffer        | int            | 8192                                                         | 网络读写缓冲区大小                                           | 2.0.5以上版本  |
| heartbeat     | int            | 0                                                            | 心跳间隔，对于长连接，当物理层断开时，比如拔网线，TCP的FIN消息来不及发送，对方收不到断开事件，此时需要心跳来帮助检查连接是否已断开 | 2.0.10以上版本 |
| telnet        | string         |                                                              | 所支持的telnet命令，多个命令用逗号分隔                       | 2.0.5以上版本  |
| register      | boolean        | true                                                         | 该协议的服务是否注册到注册中心                               | 2.0.8以上版本  |
| contextpath   | String         | 缺省为空串                                                   |                                                              | 2.0.6以上版本  |

## com.alibaba VS. org.apache.dubbo

我在 https://mvnrepository.com/ 上搜索不到`org.apache.dubbo`，只能搜索到`com.alibaba`，但是我看其他博文，使用的都是`org.apache.dubbo`，这里备注一下！

## 例子完整代码：

- pom.xml
- provider.xml
- consumer.xml
- 主要就是上面三个xml文件的配置，其他的代码看官网文档，复制一下就可以了。

### pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
         http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.wxg</groupId>
    <artifactId>dubbo-xml-spring</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <java_source_version>1.8</java_source_version>
        <java_target_version>1.8</java_target_version>
        <file_encoding>UTF-8</file_encoding>
    </properties>

    <dependencies>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>dubbo-config-spring</artifactId>
            <version>2.6.5</version>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>dubbo-registry-zookeeper</artifactId>
            <version>2.6.5</version>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>dubbo-registry-multicast</artifactId>
            <version>2.6.5</version>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>dubbo-rpc-dubbo</artifactId>
            <version>2.6.5</version>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>dubbo-remoting-netty4</artifactId>
            <version>2.6.5</version>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>dubbo-serialization-hessian2</artifactId>
            <version>2.6.5</version>
        </dependency>
    </dependencies>
</project>
```



### provider.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans-4.3.xsd
       http://dubbo.apache.org/schema/dubbo
       http://dubbo.apache.org/schema/dubbo/dubbo.xsd">

    <dubbo:provider server="netty4" />

    <!-- 提供方应用信息，用于计算依赖关系 -->
    <dubbo:application name="hello-world-app"  />

    <!-- 使用multicast广播注册中心暴露服务地址 -->
    <dubbo:registry address="multicast://224.5.6.7:1234" />

    <!-- 用dubbo协议在20880端口暴露服务 -->
    <!--<dubbo:protocol name="dubbo" port="20880" />-->
    <!--transporter="netty4", -->
    <!--<dubbo:protocol name="dubbo" port="20880" server="netty4" transporter="netty4"/>-->
    <dubbo:protocol name="dubbo" port="20880" />

    <!-- 声明需要暴露的服务接口 -->
    <dubbo:service interface="com.alibaba.dubbo.demo.api.DemoService" ref="demoService" />

    <!-- 和本地bean一样实现服务 -->
    <bean id="demoService" class="com.alibaba.dubbo.demo.provider.DemoServiceImpl" />
</beans>
```



### consumer.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans-4.3.xsd
       http://dubbo.apache.org/schema/dubbo
       http://dubbo.apache.org/schema/dubbo/dubbo.xsd">

    <dubbo:consumer client="netty4" />

    <!-- 消费方应用名，用于计算依赖关系，不是匹配条件，不要与提供方一样 -->
    <dubbo:application name="consumer-of-helloworld-app"  />

    <!-- 使用multicast广播注册中心暴露发现服务地址 -->
    <dubbo:registry address="multicast://224.5.6.7:1234" />
    <!--<dubbo:registry address="multicast://224.5.6.7:1234" server="netty4" transport="netty4" transporter="netty4" />-->

    <!-- 生成远程服务代理，可以和本地bean一样使用demoService -->
    <dubbo:reference id="demoService" interface="com.alibaba.dubbo.demo.api.DemoService" />
</beans>
```

### 包名：com.alibaba.dubbo.demo

api.DemoService.java

```java
public interface DemoService {
    String sayHello(String name);
}
```



provider.DemoServiceImpl.java

```java
public class DemoServiceImpl implements DemoService {
    public String sayHello(String name) {
        return "Hello " + name;
    }
}
```



provider.Provider

```java
public class Provider {
    public static void main(String[] args) throws Exception {
//        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext(new String[] {"http://10.20.160.198/wiki/display/dubbo/provider.xml"});
        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext(new String[]{"classpath:provider.xml"});
        context.start();
        System.in.read(); // 按任意键退出
    }
}
```



consumer.Consumer

```java
public class Consumer {
    public static void main(String[] args) throws Exception {
//        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext(new String[] {"http://10.20.160.198/wiki/display/dubbo/consumer.xml"});
        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext(new String[]{"classpath:consumer.xml"});
        context.start();
        DemoService demoService = (DemoService)context.getBean("demoService"); // 获取远程服务代理
        String hello = demoService.sayHello("world ~ wxg ~"); // 执行远程方法
        System.out.println( hello ); // 显示调用结果
    }
}
```

log4j.properties

```java
log4j.rootLogger=INFO,Console,File  
#定义日志输出目的地为控制台
log4j.appender.Console=org.apache.log4j.ConsoleAppender  
log4j.appender.Console.Target=System.out  
#可以灵活地指定日志输出格式，下面一行是指定具体的格式
log4j.appender.Console.layout = org.apache.log4j.PatternLayout  
log4j.appender.Console.layout.ConversionPattern=[%c] - %m%n  

#文件大小到达指定尺寸的时候产生一个新的文件
log4j.appender.File = org.apache.log4j.RollingFileAppender  
#指定输出目录
log4j.appender.File.File = logs/ssm.log  
#定义文件最大大小
log4j.appender.File.MaxFileSize = 10MB  
# 输出所以日志，如果换成DEBUG表示输出DEBUG以上级别日志
log4j.appender.File.Threshold = ALL  
log4j.appender.File.layout = org.apache.log4j.PatternLayout  
log4j.appender.File.layout.ConversionPattern =[%p] [%d{yyyy-MM-dd HH\:mm\:ss}][%c]%m%n 
```

# dubbo:provider

http://dubbo.apache.org/zh-cn/docs/user/references/xml/dubbo-provider.html

| 属性            | 类型           | 缺省值                                                       | 描述                                                         | 兼容性         |
| --------------- | -------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | -------------- |
| id              | string         | dubbo                                                        | 协议BeanId，可以在<dubbo:service proivder="">中引用此ID      | 1.0.16以上版本 |
| protocol        | string         | dubbo                                                        | 协议名称                                                     | 1.0.16以上版本 |
| host            | string         | 自动查找本机IP                                               | 服务主机名，多网卡选择或指定VIP及域名时使用，为空则自动查找本机IP，建议不要配置，让Dubbo自动获取本机IP | 1.0.16以上版本 |
| threads         | int            | 200                                                          | 服务线程池大小(固定大小)                                     | 1.0.16以上版本 |
| payload         | int            | 8388608(=8M)                                                 | 请求及响应数据包大小限制，单位：字节                         | 2.0.0以上版本  |
| path            | string         |                                                              | 提供者上下文路径，为服务path的前缀                           | 2.0.0以上版本  |
| server          | string         | dubbo协议缺省为netty，http协议缺省为servlet                  | 协议的服务器端实现类型，比如：dubbo协议的mina,netty等，http协议的jetty,servlet等 | 2.0.0以上版本  |
| client          | string         | dubbo协议缺省为netty                                         | 协议的客户端实现类型，比如：dubbo协议的mina,netty等          | 2.0.0以上版本  |
| codec           | string         | dubbo                                                        | 协议编码方式                                                 | 2.0.0以上版本  |
| serialization   | string         | dubbo协议缺省为hessian2，rmi协议缺省为java，http协议缺省为json | 协议序列化方式，当协议支持多种序列化方式时使用，比如：dubbo协议的dubbo,hessian2,java,compactedjava，以及http协议的json,xml等 | 2.0.5以上版本  |
| default         | boolean        | false                                                        | 是否为缺省协议，用于多协议                                   | 1.0.16以上版本 |
| filter          | string         |                                                              | 服务提供方远程调用过程拦截器名称，多个名称用逗号分隔         | 2.0.5以上版本  |
| listener        | string         |                                                              | 服务提供方导出服务监听器名称，多个名称用逗号分隔             | 2.0.5以上版本  |
| threadpool      | string         | fixed                                                        | 线程池类型，可选：fixed/cached                               | 2.0.5以上版本  |
| accepts         | int            | 0                                                            | 服务提供者最大可接受连接数                                   | 2.0.5以上版本  |
| version         | string         | 0.0.0                                                        | 服务版本，建议使用两位数字版本，如：1.0，通常在接口不兼容时版本号才需要升级 | 2.0.5以上版本  |
| group           | string         |                                                              | 服务分组，当一个接口有多个实现，可以用分组区分               | 2.0.5以上版本  |
| delay           | int            | 0                                                            | 延迟注册服务时间(毫秒)- ，设为-1时，表示延迟到Spring容器初始化完成时暴露服务 | 2.0.5以上版本  |
| timeout         | int            | 1000                                                         | 远程服务调用超时时间(毫秒)                                   | 2.0.5以上版本  |
| retries         | int            | 2                                                            | 远程服务调用重试次数，不包括第一次调用，不需要重试请设为0    | 2.0.5以上版本  |
| connections     | int            | 0                                                            | 对每个提供者的最大连接数，rmi、http、hessian等短连接协议表示限制连接数，dubbo等长连接协表示建立的长连接个数 | 2.0.5以上版本  |
| loadbalance     | string         | random                                                       | 负载均衡策略，可选值：random,roundrobin,leastactive，分别表示：随机，轮询，最少活跃调用 | 2.0.5以上版本  |
| async           | boolean        | false                                                        | 是否缺省异步执行，不可靠异步，只是忽略返回值，不阻塞执行线程 | 2.0.5以上版本  |
| stub            | boolean        | false                                                        | 设为true，表示使用缺省代理类名，即：接口名 + Local后缀。     | 2.0.5以上版本  |
| mock            | boolean        | false                                                        | 设为true，表示使用缺省Mock类名，即：接口名 + Mock后缀。      | 2.0.5以上版本  |
| token           | boolean        | false                                                        | 令牌验证，为空表示不开启，如果为true，表示随机生成动态令牌   | 2.0.5以上版本  |
| registry        | string         | 缺省向所有registry注册                                       | 向指定注册中心注册，在多个注册中心时使用，值为<dubbo:registry>的id属性，多个注册中心ID用逗号分隔，如果不想将该服务注册到任何registry，可将值设为N/A | 2.0.5以上版本  |
| dynamic         | boolean        | true                                                         | 服务是否动态注册，如果设为false，注册后将显示后disable状态，需人工启用，并且服务提供者停止时，也不会自动取消册，需人工禁用。 | 2.0.5以上版本  |
| accesslog       | string/boolean | false                                                        | 设为true，将向logger中输出访问日志，也可填写访问日志文件路径，直接把访问日志输出到指定文件 | 2.0.5以上版本  |
| owner           | string         |                                                              | 服务负责人，用于服务治理，请填写负责人公司邮箱前缀           | 2.0.5以上版本  |
| document        | string         |                                                              | 服务文档URL                                                  | 2.0.5以上版本  |
| weight          | int            |                                                              | 服务权重                                                     | 2.0.5以上版本  |
| executes        | int            | 0                                                            | 服务提供者每服务每方法最大可并行执行请求数                   | 2.0.5以上版本  |
| actives         | int            | 0                                                            | 每服务消费者每服务每方法最大并发调用数                       | 2.0.5以上版本  |
| proxy           | string         | javassist                                                    | 生成动态代理方式，可选：jdk/javassist                        | 2.0.5以上版本  |
| cluster         | string         | failover                                                     | 集群方式，可选：failover/failfast/failsafe/failback/forking  | 2.0.5以上版本  |
| deprecated      | boolean        | false                                                        | 服务是否过时，如果设为true，消费方引用时将打印服务过时警告error日志 | 2.0.5以上版本  |
| queues          | int            | 0                                                            | 线程池队列大小，当线程池满时，排队等待执行的队列大小，建议不要设置，当线程程池时应立即失败，重试其它服务提供机器，而不是排队，除非有特殊需求。 | 2.0.5以上版本  |
| charset         | string         | UTF-8                                                        | 序列化编码                                                   | 2.0.5以上版本  |
| buffer          | int            | 8192                                                         | 网络读写缓冲区大小                                           | 2.0.5以上版本  |
| iothreads       | int            | CPU + 1                                                      | IO线程池，接收网络读写中断，以及序列化和反序列化，不处理业务，业务线程池参见threads配置，此线程池和CPU相关，不建议配置。 | 2.0.5以上版本  |
| telnet          | string         |                                                              | 所支持的telnet命令，多个命令用逗号分隔                       | 2.0.5以上版本  |
| <dubbo:service> | contextpath    | String                                                       | 服务治理                                                     |                |
| layer           | string         |                                                              | 服务提供者所在的分层。如：biz、dao、intl:web、china:acton。  | 2.0.7以上版本  |

# dubbo:consumer

http://dubbo.apache.org/zh-cn/docs/user/references/xml/dubbo-consumer.html

| 属性        | 类型           | 缺省值                 | 描述                                                         | 兼容性                     |
| ----------- | -------------- | ---------------------- | ------------------------------------------------------------ | -------------------------- |
| timeout     | int            | 1000                   | 远程服务调用超时时间(毫秒)                                   | 1.0.16以上版本             |
| retries     | int            | 2                      | 远程服务调用重试次数，不包括第一次调用，不需要重试请设为0    | 1.0.16以上版本             |
| loadbalance | string         | random                 | 负载均衡策略，可选值：random,roundrobin,leastactive，分别表示：随机，轮询，最少活跃调用 | 1.0.16以上版本             |
| async       | boolean        | false                  | 是否缺省异步执行，不可靠异步，只是忽略返回值，不阻塞执行线程 | 2.0.0以上版本              |
| connections | int            | 100                    | 每个服务对每个提供者的最大连接数，rmi、http、hessian等短连接协议支持此配置，dubbo协议长连接不支持此配置 | 1.0.16以上版本             |
| generic     | boolean        | false                  | 是否缺省泛化接口，如果为泛化接口，将返回GenericService       | 2.0.0以上版本              |
| check       | boolean        | true                   | 启动时检查提供者是否存在，true报错，false忽略                | 1.0.16以上版本             |
| proxy       | string         | javassist              | 生成动态代理方式，可选：jdk/javassist                        | 2.0.5以上版本              |
| owner       | string         |                        | 调用服务负责人，用于服务治理，请填写负责人公司邮箱前缀       | 2.0.5以上版本              |
| actives     | int            | 0                      | 每服务消费者每服务每方法最大并发调用数                       | 2.0.5以上版本              |
| cluster     | string         | failover               | 集群方式，可选：failover/failfast/failsafe/failback/forking  | 2.0.5以上版本              |
| filter      | string         |                        | 服务消费方远程调用过程拦截器名称，多个名称用逗号分隔         | 2.0.5以上版本              |
| listener    | string         |                        | 服务消费方引用服务监听器名称，多个名称用逗号分隔             | 2.0.5以上版本              |
| registry    | string         | 缺省向所有registry注册 | 向指定注册中心注册，在多个注册中心时使用，值为<dubbo:registry>的id属性，多个注册中心ID用逗号分隔，如果不想将该服务注册到任何registry，可将值设为N/A | 2.0.5以上版本              |
| layer       | string         |                        | 服务调用者所在的分层。如：biz、dao、intl:web、china:acton。  | 2.0.7以上版本              |
| init        | boolean        | false                  | 是否在afterPropertiesSet()时饥饿初始化引用，否则等到有人注入或引用该实例时再初始化。 | 2.0.10以上版本             |
| cache       | string/boolean |                        | 以调用参数为key，缓存返回结果，可选：lru, threadlocal, jcache等 | Dubbo2.1.0及其以上版本支持 |
| validation  | boolean        |                        | 是否启用JSR303标准注解验证，如果启用，将对方法参数上的注解进行校验 | Dubbo2.1.0及其以上版本支持 |

# 依赖

http://dubbo.apache.org/zh-cn/docs/user/dependencies.html

> netty-all， mina， grizzly， httpclient， hessian_lite， fastjson， zookeeper， jedis， xmemcached， hessian， jetty， hibernate-validator， zkclient， curator， cxf， thrift， servlet， validation-api， jcache， javax.el， kryo， kryo-serializers， fst， resteasy， tomcat-embed-core， slf4j， log4j


#  成熟度

http://dubbo.apache.org/zh-cn/docs/user/maturity.html

- 功能成熟度
- 策略成熟度

这个“成熟度”，一直是被我忽略的……

# 配置

- XML 配置
  - http://dubbo.apache.org/zh-cn/docs/user/configuration/xml.html
- 属性配置
  - http://dubbo.apache.org/zh-cn/docs/user/configuration/properties.html
- API配置
  - http://dubbo.apache.org/zh-cn/docs/user/configuration/api.html
- 注解配置
  - http://dubbo.apache.org/zh-cn/docs/user/configuration/annotation.html



# 示例

## 启动时检查

http://dubbo.apache.org/zh-cn/docs/user/demos/preflight-check.html



# incubator-dubbo-samples

## 01 dubbo-samples-annotation

https://github.com/apache/incubator-dubbo-samples/tree/master/dubbo-samples-annotation

```sh
src
|- main
	|- java
		|- org.apache.dubbo.samples
			|- action
				|- GreetingServiceConsumer
			|- annotaion
				|- ConsumerBootstrap
				|- ProviderBootstrap
			|- api
				|- GreetingService
			|- config
				|- ConsumerBootstrap
				|- ProviderBootstrap
			|- impl
				|- AnnotatedGreetingService
			|- support
				|- EmbeddedZooKeeper
	|- resources
		|- spring
			|- dubbo-consumer.properties
			|- dubbo-provider.properties
```

本示例，有两种启动方式，一种是`config`方式，一种是`annotation`方式。

## 02 dubbo-samples-api

参考： [./113_Dubbo_dubbo-samples-api.md](./113_Dubbo_dubbo-samples-api.md)



## 03 dubbo-samples-async

这个样例有点复杂啊，重点在三个文件：

- async-consumer.xml
- async-provider.xml
- AsyncConsumer.java

**async-provider.xml**

```xml
<beans xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
       xmlns="http://www.springframework.org/schema/beans"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
       http://dubbo.apache.org/schema/dubbo http://dubbo.apache.org/schema/dubbo/dubbo.xsd">

    <dubbo:application name="async-provider"/>

    <dubbo:registry address="zookeeper://127.0.0.1:2181"/>

    <dubbo:protocol name="dubbo" port="20880"/>

    <bean id="asyncService" class="org.apache.dubbo.samples.async.impl.AsyncServiceImpl"/>

    <dubbo:service interface="org.apache.dubbo.samples.async.api.AsyncService" ref="asyncService"/>

</beans>
```

**async-consumer.xml**

```xml
<beans xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
       xmlns="http://www.springframework.org/schema/beans"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
       http://dubbo.apache.org/schema/dubbo http://dubbo.apache.org/schema/dubbo/dubbo.xsd">

    <dubbo:application name="async-consumer">
        <dubbo:parameter key="qos.enable" value="false"/>
    </dubbo:application>

    <dubbo:registry address="zookeeper://127.0.0.1:2181"/>

    <dubbo:reference id="asyncService" interface="org.apache.dubbo.samples.async.api.AsyncService">
        <dubbo:method name="goodbye" async="true"/>

        <dubbo:method name="invokeCallback" async="true" onreturn="notify.onreturn"/>
    </dubbo:reference>

    <bean id="notify" class="org.apache.dubbo.samples.async.api.Callback"/>
</beans>
```

**AsyncConsumer.java**

```java
public class AsyncConsumer {

    public static void main(String[] args) throws Exception {
        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext(new String[]{"spring/async-consumer.xml"});
        context.start();

        final AsyncService service = (AsyncService) context.getBean("asyncService");

        Future<String> f = RpcContext.getContext().asyncCall(new Callable<String>() {
            public String call() throws Exception {
                return service.sayHello("async call request");
            }
        });

        System.out.println("async call ret :" + f.get());


        RpcContext.getContext().asyncCall(new Runnable() {
            public void run() {
                service.sayHello("oneway call request1");
                service.sayHello("oneway call request2");
            }
        });

        service.goodbye("samples");
        Future<String> future = RpcContext.getContext().getFuture();
        String result = future.get();
        System.out.println(" << " + result);

        service.invokeCallback("kongming");

        System.in.read();
    }

}
```

**RpcContext**

`RpcContext`，这个类，值得看看源码的。



## 04 dubbo-samples-attachment

阅读了`dubbo-samples-annotation`，`dubbo-samples-async`…… ，再看`dubbo-samples-attachment`的结构，发现清晰了很多。

`dubbo-samples-attachment`也使用spring，xml，的方式来启动的。代码可前往github一观： https://github.com/apache/incubator-dubbo-samples/tree/master/dubbo-samples-attachment

> 可以通过 RpcContext 上的 setAttachment 和 getAttachment 在服务消费方和提供方之间进行参数的隐式传递。

不过，只能传递`String`类型的参数。

```java
private final Map<String, String> attachments = new HashMap<String, String>();

public RpcContext setAttachment(String key, String value) {
	if (value == null) {
		attachments.remove(key);
	} else {
		attachments.put(key, value);
	}
	return this;
}

public String getAttachment(String key) {
	return attachments.get(key);
}
```



## 05 dubbo-samples-basic



**dubbo-demo-provider.xml**

```xml
<beans xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
       xmlns="http://www.springframework.org/schema/beans"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
       http://dubbo.apache.org/schema/dubbo http://dubbo.apache.org/schema/dubbo/dubbo.xsd">

    <!-- provider's application name, used for tracing dependency relationship -->
    <dubbo:application name="demo-provider"/>

    <!-- use multicast registry center to export service -->
    <dubbo:registry group="aaa" address="zookeeper://127.0.0.1:2181"/>
    <dubbo:registry address="zookeeper://127.0.0.1:2181"/>
    <!--<dubbo:registry address="zookeeper://11.163.250.27:2181"/>-->

    <!-- use dubbo protocol to export service on port 20880 -->
    <dubbo:protocol name="dubbo" port="20890"/>

    <!-- service implementation, as same as regular local bean -->
    <bean id="demoService" class="org.apache.dubbo.samples.basic.impl.DemoServiceImpl"/>

    <!-- declare the service interface to be exported -->
    <dubbo:service interface="org.apache.dubbo.samples.basic.api.DemoService" ref="demoService"/>

</beans>
```

如果将`group="aaa"` 修改为`group="bbb"`，那么consumer就找不到，注册服务了。

**dubbo-demo-consumer.xml**

```xml
<beans xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
       xmlns="http://www.springframework.org/schema/beans"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
       http://dubbo.apache.org/schema/dubbo http://dubbo.apache.org/schema/dubbo/dubbo.xsd">

    <!-- consumer's application name, used for tracing dependency relationship (not a matching criterion),
    don't set it same as provider -->
    <dubbo:application name="demo-consumer">
        <dubbo:parameter key="qos.enable" value="false"/>
    </dubbo:application>

    <!-- use multicast registry center to discover service -->
    <dubbo:registry group="aaa" address="zookeeper://127.0.0.1:2181"/>

    <!-- generate proxy for the remote service, then demoService can be used in the same way as the
    local regular interface -->
    <dubbo:reference id="demoService" check="false" interface="org.apache.dubbo.samples.basic.api.DemoService"/>

</beans>
```



## 06 dubbo-samples-cache

https://github.com/apache/incubator-dubbo-samples/tree/master/dubbo-samples-cache

这个例子，没怎么看懂！

需要，复习，思考！

LRU

AtomicInteger

**CacheConsumer.java**

```java
public class CacheConsumer {

    public static void main(String[] args) throws Exception {
        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext(new String[]{"spring/cache-consumer.xml"});
        context.start();

        CacheService cacheService = (CacheService) context.getBean("cacheService");

        // verify cache, same result is returned for different invocations (in fact, the return value increases
        // on every invocation on the server side)
        String fix = null;
        for (int i = 0; i < 5; i++) {
            String result = cacheService.findCache("0");
            if (fix == null || fix.equals(result)) {
                System.out.println("OK: " + result);
            } else {
                System.err.println("ERROR: " + result);
            }
            fix = result;
            Thread.sleep(500);
        }

        // default cache.size is 1000 for LRU, should have cache expired if invoke more than 1001 times
        for (int n = 0; n < 1001; n++) {
            String pre = null;
            for (int i = 0; i < 10; i++) {
                String result = cacheService.findCache(String.valueOf(n));
                if (pre != null && !pre.equals(result)) {
                    System.err.println("ERROR: " + result);
                }
                pre = result;
            }
        }

        // verify if the first cache item is expired in LRU cache
        String result = cacheService.findCache("0");
        if (fix != null && !fix.equals(result)) {
            System.out.println("OK: " + result);
        } else {
            System.err.println("ERROR: " + result);
        }
    }

}
```

这段代码逻辑，没看懂~！这里的用法应该就是`cache`的含义吧？！




## 07 dubbo-samples-callback

https://github.com/apache/incubator-dubbo-samples/tree/master/dubbo-samples-callback

这个例子，没怎么看懂！

需要，复习，思考！

**callback-provider.xml**

```xml
<beans xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
       xmlns="http://www.springframework.org/schema/beans"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
       http://dubbo.apache.org/schema/dubbo http://dubbo.apache.org/schema/dubbo/dubbo.xsd">

    <dubbo:application name="callback-provider"/>

    <!--<dubbo:registry address="multicast://224.5.6.7:1234"/>-->
    <dubbo:registry address="zookeeper://127.0.0.1:2181"/>

    <dubbo:protocol name="dubbo" port="20880"/>

    <bean id="callbackService" class="org.apache.dubbo.samples.callback.impl.CallbackServiceImpl"/>

    <dubbo:service interface="org.apache.dubbo.samples.callback.api.CallbackService" ref="callbackService"
                   connections="1" callbacks="1000">
        <dubbo:method name="addListener">
            <dubbo:argument index="1" callback="true"/>
            <!--<dubbo:argument type="com.demo.CallbackListener" callback="true" />-->
        </dubbo:method>
    </dubbo:service>

</beans>
```

这里provider的配置比之前的例子，增加了很多参数。

**CallbackServiceImpl.java**

```java
public class CallbackServiceImpl implements CallbackService {

    private final Map<String, CallbackListener> listeners = new ConcurrentHashMap<String, CallbackListener>();

    public CallbackServiceImpl() {
        Thread t = new Thread(new Runnable() {
            public void run() {
                while (true) {
                    try {
                        for (Map.Entry<String, CallbackListener> entry : listeners.entrySet()) {
                            try {
                                entry.getValue().changed(getChanged(entry.getKey()));
                            } catch (Throwable t) {
                                listeners.remove(entry.getKey());
                            }
                        }
                        Thread.sleep(5000); // timely trigger change event
                    } catch (Throwable t) {
                        t.printStackTrace();
                    }
                }
            }
        });
        t.setDaemon(true);
        t.start();
    }

    public void addListener(String key, CallbackListener listener) {
        listeners.put(key, listener);
        listener.changed(getChanged(key)); // send notification for change
    }

    private String getChanged(String key) {
        return "Changed: " + new SimpleDateFormat("yyyy-MM-dd HH:mm:ss").format(new Date());
    }

}
```

上面这段代码虽然占了很多行，但是，总体还是比较好懂的。

- 在构造方法中，启动了一个守护线程，
- 遍历`listeners`这个map对象，来触发`changed()`方法，这个方法来自，自定义接口`CallbackListener`，
- 在构造方法中还调用了一个私有方法`getChanged()`，这个方法中的内容，是完全可以直接写在构造方法中的，抽取出来只是便于重用和理解而已！

**CallbackConsumer.java**

```java
public class CallbackConsumer {

    public static void main(String[] args) throws Exception {
        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext(new String[]{"spring/callback-consumer.xml"});
        context.start();
        CallbackService callbackService = (CallbackService) context.getBean("callbackService");
        callbackService.addListener("foo.bar", new CallbackListener() {
            public void changed(String msg) {
                System.out.println("callback1:" + msg);
            }
        });
        System.in.read();
    }

}
```

消费端的代码如上，



## 08 dubbo-samples-context



- 上下文中存放的是当前调用过程中所需的环境信息。所有配置信息都将转换为 URL 的参数。
- RpcContext 是一个 ThreadLocal 的临时状态记录器，当接收到 RPC 请求，或发起 RPC 请求时，RpcContext 的状态都会变化。比如：A 调 B，B 再调 C，则 B 机器上，在 B 调 C 之前，RpcContext 记录的是 A 调 B 的信息，在 B 调 C 之后，RpcContext 记录的是 B 调 C 的信息



> 以上是，从官网`README`中复制的

有趣的是这两个类：`ContextServiceImpl`，`ContextConsumer`

ContextServiceImpl.java

```java
public class ContextServiceImpl implements ContextService{

    @Override
    public String sayHello(String name) {

        boolean isProviderSide = RpcContext.getContext().isProviderSide();
        String clientIP = RpcContext.getContext().getRemoteHost();
        String application = RpcContext.getContext().getUrl().getParameter("application");

        return "Hello " + name + ", response from provider: " + RpcContext.getContext().getLocalAddress();
    }
}
```



ContextConsumer.java

```java
public class ContextConsumer {

    public static void main(String[] args) {
        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext(new String[]{"spring/dubbo-context-consumer.xml"});
        context.start();
        ContextService contextService = (ContextService) context.getBean("demoService"); // get remote service proxy

        String hello = contextService.sayHello("world"); // call remote method

        boolean isConsumerSide = RpcContext.getContext().isConsumerSide();
        String application = RpcContext.getContext().getUrl().getParameter("application");
        String serverIP = RpcContext.getContext().getRemoteHost();

        System.out.println(hello); // get result

    }
}
```

这个样例，就是`RpcContext`的抛砖引玉，教我们如何使用`RpcContext`的。

- `isProviderSide()`
- `isConsumerSide()`
- `getRemoteHost()`
- `getUrl().getParameter("application")`
- 

`04 dubbo-samples-attachment`也是需要使用`RpcContext`的。




## 09 dubbo-samples-direct

这个`direct`的含义是，不需要“注册”，直接与远端进行连接吗？

- 目前，我的理解是，不需要“注册”，直连，我将provider.xml修改为`<dubbo:registry address="N/A"/>`，程序仍然正常的。

**dubbo-direct-provider.xml**

```xml
<beans xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
       xmlns="http://www.springframework.org/schema/beans"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
       http://dubbo.apache.org/schema/dubbo http://dubbo.apache.org/schema/dubbo/dubbo.xsd">

    <!-- provider's application name, used for tracing dependency relationship -->
    <dubbo:application name="demo-provider"/>

    <!-- use multicast registry center to export service -->
    <!--<dubbo:registry address="zookeeper://127.0.0.1:2181"/>-->
    <dubbo:registry address="N/A"/>

    <!-- use dubbo protocol to export service on port 20880 -->
    <dubbo:protocol name="dubbo" port="20880"/>

    <!-- service implementation, as same as regular local bean -->
    <bean id="directService" class="org.apache.dubbo.samples.direct.impl.DirectServiceImpl"/>

    <!-- declare the service interface to be exported -->
    <dubbo:service interface="org.apache.dubbo.samples.direct.api.DirectService" ref="directService"/>

</beans>
```

**dubbo-direct-consumer.xml**

```xml
<beans xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
       xmlns="http://www.springframework.org/schema/beans"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
       http://dubbo.apache.org/schema/dubbo http://dubbo.apache.org/schema/dubbo/dubbo.xsd">

    <!-- consumer's application name, used for tracing dependency relationship (not a matching criterion),
    don't set it same as provider -->
    <dubbo:application name="demo-consumer">
        <dubbo:parameter key="qos.enable" value="false"/>
    </dubbo:application>

    <!-- use multicast registry center to discover service -->
    <!--<dubbo:registry address="zookeeper://127.0.0.1:2181"/>-->

    <!-- generate proxy for the remote service, then demoService can be used in the same way as the
    local regular interface -->
    <dubbo:reference id="directService" check="false" interface="org.apache.dubbo.samples.direct.api.DirectService" url="localhost:20880"/>

</beans>
```











## 10 dubbo-samples-docker



略，有计划，在docker上测试一下！





## 11 dubbo-sampels-echo

- 回声测试用于检测服务是否可用，回声测试按照正常请求流程执行，能够测试整个调用是否通畅，可用于监控。
- 所有服务自动实现 EchoService 接口，只需将任意服务引用强制转型为 EchoService，即可使用。

> 以上，是从`README`中复制过来的。

任何`service`类都可以被转义为`EchoService`，然后可以调用`$echo(Object)`方法，

**EchoConsumer.java**

```java
public class EchoConsumer {

    public static void main(String[] args) {
        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext(new String[]{"spring/echo-consumer.xml"});
        context.start();
        DemoService demoService = (DemoService) context.getBean("demoService"); // get remote service proxy

        EchoService echoService = (EchoService) demoService;
        String status = (String)echoService.$echo("OK");
        System.out.println("echo result: " + status);
    }
}
```



## 12 dubbo-samples-generic

这个和`11 dubbo-samples-echo`有些相似点： 任何`service`也都可以被转义为`GenericService`，然后调用`$invoke(String,String[],Object[])`方法

```sh
generic
|- api
	|- HelloService
	|- HiService
	|- IUserService
	|- Params
	|- User
|- impl
	|- GenericServiceImpl
	|- UserServiceImpl
|- APIGenericConsumer
|- AIPGenericProvider
|- EmbeddedZooKeeper
|- GenericConsumer
|- GenericConsumer2
|- GenericProvider
```

`generic-provider.xml`和其他的样例没什么区别，`generic-consumer.xml`有些不同，

**generic-consumer.xml**

```xml
<beans xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
       xmlns="http://www.springframework.org/schema/beans"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
       http://dubbo.apache.org/schema/dubbo http://dubbo.apache.org/schema/dubbo/dubbo.xsd">

    <dubbo:application name="generic-consumer">
        <dubbo:parameter key="qos.enable" value="false"/>
    </dubbo:application>

    <dubbo:registry address="zookeeper://127.0.0.1:2181"/>

    <dubbo:reference id="userService" interface="org.apache.dubbo.samples.generic.api.IUserService" generic="true"/>

</beans>
```

如上，`userService`被声明为`generic="true"`。



**GenericConsumer.java**

```java
public class GenericConsumer {

    public static void main(String[] args) throws Exception {
        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext(new String[]{"spring/generic-consumer.xml"});
        context.start();
        GenericService userService = (GenericService) context.getBean("userService");

        // primary param and return value
        String name = (String) userService.$invoke("delete", new String[]{int.class.getName()}, new Object[]{1});
        System.out.println(name);

        String[] parameterTypes = new String[]{"org.apache.dubbo.samples.generic.api.Params"};
        // sample one
        Map<String, Object> param = new HashMap<String, Object>();
        param.put("class", "org.apache.dubbo.samples.generic.api.Params");
        param.put("query", "a=b");
        Object user = userService.$invoke("get", parameterTypes, new Object[]{param});
        System.out.println("sample one result: " + user);
        // sample two
        user = userService.$invoke("get", parameterTypes, new Object[]{new Params("a=b")});
        System.out.println("sample two result: " + user);
        System.in.read();
    }
}
```

**GenericConsumer2.java**

```java
public class GenericConsumer2 {
    public static void main(String[] args) {
        ApplicationConfig application = new ApplicationConfig();
        application.setName("api-generic-consumer");
        application.setQosEnable(false);

        RegistryConfig registry = new RegistryConfig();
        registry.setAddress("zookeeper://127.0.0.1:2181");

        application.setRegistry(registry);

        ReferenceConfig<GenericService> reference = new ReferenceConfig<GenericService>();
        // 弱类型接口名
        reference.setInterface("org.apache.dubbo.samples.generic.api.IUserService");
        // 声明为泛化接口
        reference.setGeneric(true);

        reference.setApplication(application);

        // 用org.apache.dubbo.rpc.service.GenericService可以替代所有接口引用
        GenericService genericService = reference.get();

        String name = (String) genericService.$invoke("delete", new String[]{int.class.getName()}, new Object[]{1});
        System.out.println(name);
    }
}
```

这个是没有使用`xml`的方式。

- `reference.setGeneric(true);`

**APIGenericProvider.java**

```java
public class APIGenericProvider {
    public static void main(String[] args) throws IOException {
        new EmbeddedZooKeeper(2181, false).start();

        ApplicationConfig application = new ApplicationConfig();
        application.setName("api-generic-provider");

        RegistryConfig registry = new RegistryConfig();
        registry.setAddress("zookeeper://127.0.0.1:2181");

        application.setRegistry(registry);

        GenericService genericService = new GenericServiceImpl();

        ServiceConfig<GenericService> service = new ServiceConfig<GenericService>();

        // 弱类型接口名
        service.setApplication(application);
        service.setInterface("org.apache.dubbo.samples.generic.api.HelloService");
        service.setRef(genericService);
        service.export();

        ServiceConfig<GenericService> service2 = new ServiceConfig<GenericService>();
        // 弱类型接口名
        service2.setApplication(application);
        service2.setInterface("org.apache.dubbo.samples.generic.api.HiService");
        service2.setRef(genericService);
        service2.export();
        System.in.read();
    }
}
```

如上， `provider`端，通过`ServiceConfig`配置`service`，发布两个service，`02 dubbo-samples-api`样例中也是这样做的，可以参考。

- HelloService
- HiService

并声明为`GenericeService`

- `service.setRef(genericService);`
- `service2.setRef(genericService);`



**APIGenericConsumer.java**

```java
public class APIGenericConsumer {
    public static void main(String[] args) {
        ApplicationConfig application = new ApplicationConfig();
        application.setName("api-generic-consumer");
        application.setQosEnable(false);

        RegistryConfig registry = new RegistryConfig();
        registry.setAddress("zookeeper://127.0.0.1:2181");

        application.setRegistry(registry);

        ReferenceConfig<GenericService> reference = new ReferenceConfig<GenericService>();

        // 弱类型接口名
        reference.setInterface(HiService.class);
        reference.setApplication(application);

        HiService hiService = (HiService) reference.get();
        System.out.println(hiService.hi("dubbo"));

        ReferenceConfig<GenericService> reference2 = new ReferenceConfig<GenericService>();

        // 弱类型接口名
        reference2.setInterface(HelloService.class);
        reference2.setApplication(application);

        HelloService helloService = (HelloService) reference2.get();
        System.out.println(helloService.hello("community"));
    }
}
```

上面的两个调用都使用的是`ReferenceConfig`，其实使用`ReferenceConfig`也是可以的。如下：

```java
ReferenceConfig<HelloService> reference3 = new ReferenceConfig();
reference3.setInterface(HelloService.class);
reference3.setApplication(application);

HelloService helloService1 = reference3.get();
System.out.println(helloService1.hello("wxg"));
```







## 13 dubbo-samples-group

代码很简单，因为样例中的业务很简单，现在还看不出，这个样例的作用！

```sh
group
|- api
	|- GroupService
|- impl
	|- GroupAServiceImpl
	|- GroupBServiceImpl
|- EmbeddedZooKeeper
|- GroupConsumer
|- GroupProvider

src/main/resources
|- spring
	|- group-consumer.xml
	|- group-provider.xml
```

`GroupAServiceImpl`， `GroupBServiceImpl` 都是接口`GroupService`的实现。

**group-provider.xml**

```xm l
<beans xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
       xmlns="http://www.springframework.org/schema/beans"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
       http://dubbo.apache.org/schema/dubbo http://dubbo.apache.org/schema/dubbo/dubbo.xsd">

    <!-- provider's application name, used for tracing dependency relationship -->
    <dubbo:application name="demo-provider"/>

    <!-- use multicast registry center to export service -->
    <dubbo:registry address="zookeeper://127.0.0.1:2181"/>

    <!-- use dubbo protocol to export service on port 20880 -->
    <dubbo:protocol name="dubbo" port="20880"/>

    <!-- service implementation, as same as regular local bean -->
    <bean id="groupAService" class="org.apache.dubbo.samples.group.impl.GroupAServiceImpl"/>

    <bean id="groupBService" class="org.apache.dubbo.samples.group.impl.GroupBServiceImpl"/>

    <!-- declare the service interface to be exported -->
    <dubbo:service group="groupA" interface="org.apache.dubbo.samples.group.api.GroupService" ref="groupAService"/>

    <dubbo:service group="groupB" interface="org.apache.dubbo.samples.group.api.GroupService" ref="groupBService"/>

</beans>
```

**group-consumer.xml**

```xml
<beans xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
       xmlns="http://www.springframework.org/schema/beans"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
       http://dubbo.apache.org/schema/dubbo http://dubbo.apache.org/schema/dubbo/dubbo.xsd">

    <!-- consumer's application name, used for tracing dependency relationship (not a matching criterion),
    don't set it same as provider -->
    <dubbo:application name="demo-consumer">
        <dubbo:parameter key="qos.enable" value="false"/>
    </dubbo:application>

    <!-- use multicast registry center to discover service -->
    <dubbo:registry address="zookeeper://127.0.0.1:2181"/>

    <!-- generate proxy for the remote service, then demoService can be used in the same way as the
    local regular interface -->
    <dubbo:reference group="groupA" id="groupAService" check="false" interface="org.apache.dubbo.samples.group.api.GroupService"/>

    <dubbo:reference group="groupB" id="groupBService" check="false" interface="org.apache.dubbo.samples.group.api.GroupService"/>

</beans>
```

**GroupConsumer.xml**

```java
public class GroupConsumer {

    public static void main(String[] args) {
        //Prevent to get IPV6 address,this way only work in debug mode
        //But you can pass use -Djava.net.preferIPv4Stack=true,then it work well whether in debug mode or not
        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext(new String[]{"spring/group-consumer.xml"});
        context.start();
        GroupService groupAService = (GroupService) context.getBean("groupAService"); // get remote service proxy
        GroupService groupBService = (GroupService)context.getBean("groupBService");

        while (true) {
            try {
                Thread.sleep(1000);
                String resultGroupA = groupAService.sayHello("world"); // call remote method
                System.out.println(resultGroupA); // get result

                String resultGroupB = groupBService.sayHello("world");
                System.out.println(resultGroupB); // get result

            } catch (Throwable throwable) {
                throwable.printStackTrace();
            }


        }

    }
}
```

目前，仅从样例，还看不出`group`的用途……

- 我需要进一步调研~！





## 14 dubbo-samples-http

之前的例子都是使用的`dubbo`协议，这个样例使用的是`http`协议，因此，`pom.xml`和之前的例子不同。

**pom.xml**

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
         http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>dubbo-samples-all</artifactId>
        <groupId>org.apache.dubbo</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>dubbo-samples-http</artifactId>

    <properties>
        <tomcat.version>7.0.88</tomcat.version>
        <servlet.version>3.0.1</servlet.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-web</artifactId>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>${servlet.version}</version>
        </dependency>
        <dependency>
            <groupId>org.apache.tomcat.embed</groupId>
            <artifactId>tomcat-embed-core</artifactId>
            <version>${tomcat.version}</version>
        </dependency>
        <dependency>
            <groupId>org.apache.tomcat.embed</groupId>
            <artifactId>tomcat-embed-logging-juli</artifactId>
            <version>${tomcat.version}</version>
        </dependency>
    </dependencies>

</project>
```

然后，其他的类没什么特殊的 ，还是用的`sayHello(String) : String`这个例子，`http-provider.xml`有所不同，需要注意一下：

**http-provider.xml**

```xml
<beans xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
       xmlns="http://www.springframework.org/schema/beans"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
       http://dubbo.apache.org/schema/dubbo http://dubbo.apache.org/schema/dubbo/dubbo.xsd">

    <dubbo:application name="generic-generic"/>

    <dubbo:registry address="zookeeper://127.0.0.1:2181"/>

    <dubbo:protocol name="http" port="8080" server="tomcat"/>

    <bean id="demoService" class="org.apache.dubbo.samples.http.impl.DemoServiceImpl"/>

    <dubbo:service interface="org.apache.dubbo.samples.http.api.DemoService" ref="demoService" protocol="http"/>

</beans>
```

如上，`dubbo:protocol`和`dubbo:service`注意一下！



## 15 dubbo-samples-jetty

一开始，我以为和`14 dubbo-samples-http`一样，只需将`server="tomcat"`改为`server="jetty"`就可以了的，但是，看了代码发现完全不一样！

首先，还是`pom.xml`

**pom.xml**

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.apache.dubbo</groupId>
    <artifactId>dubbo-samples-jetty</artifactId>
    <version>1.0-SNAPSHOT</version>

    <name>dubbo-samples-jetty</name>
    <!-- FIXME change it to the project's website -->
    <url>http://www.example.com</url>

    <parent>
        <artifactId>dubbo-samples-all</artifactId>
        <groupId>org.apache.dubbo</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>1.7</maven.compiler.source>
        <maven.compiler.target>1.7</maven.compiler.target>
    </properties>

    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.11</version>
            <scope>test</scope>
        </dependency>

        <dependency>
            <groupId>org.eclipse.jetty</groupId>
            <artifactId>jetty-server</artifactId>
            <version>9.4.11.v20180605</version>
        </dependency>
        <dependency>
            <groupId>org.eclipse.jetty</groupId>
            <artifactId>jetty-servlet</artifactId>
            <version>9.4.11.v20180605</version>
        </dependency>
    </dependencies>
...
</project>
```

```
org.apache.dubbo.samples.jetty
|- api
	|- JettyService
|- impl
	|- JettyServiceImpl
|- HelloWorld
|- JettyContainer
```

**JettyService**

```java
public interface JettyService {
    void sayHello();
}
```



**JettyServiceImpl**

```java
public class JettyServiceImpl implements JettyService {

    private static JettyContainer container = new JettyContainer();

    @Override
    public void sayHello() {
        container.setServerHandler(new HelloWorld());

        container.start();

        container.stop();
    }

}
```



**HelloWorld**

```java
public class HelloWorld extends AbstractHandler
{
    public void handle(String target,
                       Request baseRequest,
                       HttpServletRequest request,
                       HttpServletResponse response)
            throws IOException, ServletException
    {
        response.setContentType("text/html;charset=utf-8");
        response.setStatus(HttpServletResponse.SC_OK);
        baseRequest.setHandled(true);
        response.getWriter().println("<h1>Hello World</h1>");
    }
}
```



**JettyContainer**

```java
public class JettyContainer implements Container {

    private static final Logger logger = LoggerFactory.getLogger(JettyContainer.class);

    private static Server server = new Server(8080);

    public void setServerHandler(Handler handler){
        server.setHandler(handler);
    }

    @Override
    public void start() {
        try {
            server.start();
            server.join();
        } catch (Exception e) {
            throw new IllegalStateException("Failed to start jetty" + e.getMessage(), e);
        }
    }

    @Override
    public void stop() {
        if (server != null) {
            try {
                server.stop();
            } catch (Exception e) {
                logger.warn(e.getMessage(), e);
            }
        }
    }
}
```



这个样例，没有看懂！





## 16 dubbo-samples-local

这个`local`是表示`in-jvm`吗？

```
src/main/
|- java/org.apache.dubbo.samples.local
	|- api
		|- DemoService
	|- impl
		|- DemoServiceImpl
	|- EmbeddedZooKeeper
	|- LocalDemo
|- resources/spring
	|- dubbo-demo.xml
```

**DemoService.java**

```java
public interface DemoService {

    String sayHello(String name);

}
```



**DemoServiceImpl**

```java
public class DemoServiceImpl implements DemoService {

    public String sayHello(String name) {
        System.out.println("[" + new SimpleDateFormat("HH:mm:ss").format(new Date()) + "] Hello " + name + ", request from consumer: " + RpcContext
            .getContext().getRemoteAddress());
        return "Hello " + name + ", response from provider: " + RpcContext.getContext().getLocalAddress();
    }

}
```



**LocalDemo**

```java
public class LocalDemo {

    public static void main(String[] args) throws Exception {
        new EmbeddedZooKeeper(2181, false).start();
        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext(new String[]{"spring/dubbo-demo.xml"});
        context.start();

        DemoService demoService = (DemoService) context.getBean("demoService"); // get remote service proxy

        while (true) {
            try {
                Thread.sleep(1000);
                String hello = demoService.sayHello("world"); // call remote method
                System.out.println(hello); // get result

            } catch (Throwable throwable) {
                throwable.printStackTrace();
            }
        }
    }
}
```

`LocalDemo.java`就是启动类，这里看不出谁是`provider`谁是`consumer`，

我还是觉得，没搞懂： 不然我不会看到这样的代码，就迷惑了……

**dubbo-demo.xml**

```xml
<beans xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
       xmlns="http://www.springframework.org/schema/beans"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
       http://dubbo.apache.org/schema/dubbo http://dubbo.apache.org/schema/dubbo/dubbo.xsd">

    <dubbo:application name="demo-provider"/>

    <dubbo:registry address="zookeeper://127.0.0.1:2181"/>

    <dubbo:protocol name="dubbo" port="20890"/>

    <bean id="demoServiceTarget" class="org.apache.dubbo.samples.local.impl.DemoServiceImpl"/>

    <dubbo:service interface="org.apache.dubbo.samples.local.api.DemoService" ref="demoServiceTarget"/>

    <dubbo:reference id="demoService" interface="org.apache.dubbo.samples.local.api.DemoService"/>

</beans>
```

只有一个配置，如上！



## 17 dubbo-samples-merge

### 分组聚合

按组合并返回结果，比如菜单服务，接口一样，但有多种实现，用group区分，现在消费方需从每种group中调用一次返回结果，合并结果返回，这样就可以实现聚合菜单项。

> 以上，是`README`中复制过来的。

这个样例和`13 dubbo-samples-group`有很深的关联。

```
src/main
|- java
	|- org.apache.dubbo.samples.merge
		|- api
			|- MergeService
		|- impl
			|- MergeServiceImpl
			|- MergeServiceImpl2
			|- MergeServiceImpl3
		|- EmbeddedZooKeeper
		|- MergeConsumer
		|- MergeConsumer2
		|- MergeProvider
		|- MergeProvider2
|- resources
	|- spring
		|- merge-consumer.xml
		|- merge-consumer2.xml
		|- merge-provider.xml
		|- merge-provider2.xml
```

**MergeService.java**

```java
public interface MergeService {

    List<String> mergeResult();

}
```

**MergeServiceImpl.java**

```java
public class MergeServiceImpl implements MergeService {

    public List<String> mergeResult() {
        List<String> menus = new ArrayList<String>();
        menus.add("group-1.1");
        menus.add("group-1.2");
        return menus;
    }

}
```

**MergeServiceImpl2.java**

```java
public class MergeServiceImpl2 implements MergeService {

    public List<String> mergeResult() {
        List<String> menus = new ArrayList<String>();
        menus.add("group-2.1");
        menus.add("group-2.2");
        return menus;
    }

}
```

**MergeServiceImpl3.java**

```java
public class MergeServiceImpl3 implements MergeService {

    public List<String> mergeResult() {
        List<String> menus = new ArrayList<String>();
        menus.add("group-3.1");
        menus.add("group-3.2");
        return menus;
    }

}
```

**MergeConsumer.java**

```java
public class MergeConsumer {

    public static void main(String[] args) throws Exception {
        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext(new String[]{"spring/merge-consumer.xml"});
        context.start();
        MergeService mergeService = (MergeService) context.getBean("mergeService");
        for (int i = 0; i < Integer.MAX_VALUE; i++) {
            try {
                List<String> result = mergeService.mergeResult();
                System.out.println("(" + i + ") " + result);
                Thread.sleep(1000);
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }

}
```

**MergeConsumer2.java**

```java
public class MergeConsumer2 {

    public static void main(String[] args) throws Exception {
        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext(new String[]{"spring/merge-consumer2.xml"});
        context.start();
        MergeService mergeService = (MergeService) context.getBean("mergeService");
        for (int i = 0; i < Integer.MAX_VALUE; i++) {
            try {
                List<String> result = mergeService.mergeResult();
                System.out.println("(" + i + ") " + result);
                Thread.sleep(1000);
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }

}
```

**MergeProvider.java**

```java
public class MergeProvider {

    public static void main(String[] args) throws Exception {
        new EmbeddedZooKeeper(2181, false).start();
        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext(new String[]{"spring/merge-provider.xml"});
        context.start();
        System.in.read();
    }

}
```

**MergeProvider2.java**

```java
public class MergeProvider2 {

    public static void main(String[] args) throws Exception {
        new EmbeddedZooKeeper(2181, false).start();
        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext(new String[]{"spring/merge-provider2.xml"});
        context.start();
        System.in.read();
    }

}
```

**merge-consumer.xml**

```xml
<beans xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
       xmlns="http://www.springframework.org/schema/beans"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
       http://dubbo.apache.org/schema/dubbo http://dubbo.apache.org/schema/dubbo/dubbo.xsd">

    <dubbo:application name="merge-consumer">
        <dubbo:parameter key="qos.enable" value="false"/>
    </dubbo:application>

    <dubbo:registry address="zookeeper://127.0.0.1:2181"/>

    <dubbo:reference id="mergeService" interface="org.apache.dubbo.samples.merge.api.MergeService" group="*"/>

</beans>
```

`merge-consumer.xml`对应于`MergeConsumer.java`，这里`group="*"`，那说明所有的`group`都满足，

- 当启动`MergeProvider`，输出的是： `[group-1.1, group-1.2]`
- 当启动`MergeProvider2`，输出的是： `[group-3.1, group-3.2]`
- `[group-2.1, group-2.2]`看不到输出。因为，`[group-1.1, group-1.2]`会优先输出！

**merge-consumer2.xml**

```xml
<beans xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
       xmlns="http://www.springframework.org/schema/beans"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
       http://dubbo.apache.org/schema/dubbo http://dubbo.apache.org/schema/dubbo/dubbo.xsd">

    <dubbo:application name="merge-consumer2">
        <dubbo:parameter key="qos.enable" value="false"/>
    </dubbo:application>

    <dubbo:registry address="zookeeper://127.0.0.1:2181"/>

    <dubbo:reference id="mergeService" interface="org.apache.dubbo.samples.merge.api.MergeService"
                     group="merge2,merge3"/>

</beans>
```

`group="merge2,merge3"`

- 在这里，和上面不一样，`[group-1.1, group-1.2]`没有输出机会，因为它对应的是`group=merge`，而上面只有`merge2,merge3`。

**merge-provider.xml**

```xml
<beans xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
       xmlns="http://www.springframework.org/schema/beans"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
       http://dubbo.apache.org/schema/dubbo http://dubbo.apache.org/schema/dubbo/dubbo.xsd">

    <dubbo:application name="merge-provider"/>

    <dubbo:registry address="zookeeper://127.0.0.1:2181"/>

    <dubbo:protocol name="dubbo" port="20880"/>

    <bean id="mergeService" class="org.apache.dubbo.samples.merge.impl.MergeServiceImpl"/>

    <dubbo:service group="merge" interface="org.apache.dubbo.samples.merge.api.MergeService" ref="mergeService"/>

    <bean id="mergeService2" class="org.apache.dubbo.samples.merge.impl.MergeServiceImpl2"/>

    <dubbo:service group="merge2" interface="org.apache.dubbo.samples.merge.api.MergeService" ref="mergeService2"/>

</beans>
```

**merge-provider2.xml**

```xml
<beans xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
       xmlns="http://www.springframework.org/schema/beans"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
       http://dubbo.apache.org/schema/dubbo http://dubbo.apache.org/schema/dubbo/dubbo.xsd">

    <dubbo:application name="merge-provider2"/>

    <dubbo:registry address="zookeeper://127.0.0.1:2181"/>

    <dubbo:protocol name="dubbo" port="20881"/>

    <bean id="mergeService3" class="org.apache.dubbo.samples.merge.impl.MergeServiceImpl3"/>

    <dubbo:service group="merge3" interface="org.apache.dubbo.samples.merge.api.MergeService" ref="mergeService3"/>

</beans>
```



## 18 dubbo-samples-mock









## 19 dubbo-samples-monitor







## 20 dubbo-samples-multi-registry







## 21 dubbo-samples-notify







## 22 dubbo-samples-rest







## 23 dubbo-samples-scala







## 24 dubbo-samples-spring-boot-hystrix







## 25 dubbo-samples-stub







## 26 dubbo-samples-switch-serialization-thread







## 27 dubbo-samples-validation







## 28 dubbo-samples-version







## 29 dubbo-samples-zipkin







## 30 dubbo-samples-zookeeper











# 我的发现

## qosEnable

`dubbo-samples-annotation`中，provider和consumer两者都启动后，总有一方会报错：

> [14/12/18 11:27:05:005 CST] main ERROR server.Server:  [DUBBO] qos-server can not bind localhost:22222, dubbo version: 2.6.4, current host: 192.168.56.102
> java.net.BindException: Address already in use: bind

如果是使用配置文件，可以修改`dubbo-consumer.properties`：

```sh
# dubbo.application.qos-enable=false
# dubbo.application.qos.enable=false
dubbo.application.qosEnable=false
```

只能使用`qosEnable`，`qos-enable`和`qos.enable`都是无效的。

在`dubbo-spring-boot-autoconfigure-0.1.0.jar`jar包中有一个`spring-configuration-metadata.json`文件，可以搜索到`dubbo.application.qos-enable`。

在这篇 https://blog.csdn.net/u013202238/article/details/81432784 文章中也提到了`qos-server`端口冲突的问题。作者寻找问题的方式值得借鉴，修改log4j以达到，定位问题，的目的，属实高明！



http://dubbo.apache.org/zh-cn/docs/user/references/qos.html



以上链接是官方关于“QOS”的文档，上面详细的描述了“QOS”的作用及配置方式，不用到处去找博客，找答案了。上面的链接非常详细。

文档上描述了配置“QOS”的四种方式：

- 系统属性
- dubbo.properties
- XML
- spring-boot

**系统属性**

```sh
-Ddubbo.application.qos.enable=true
-Ddubbo.application.qos.port=33333
-Ddubbo.application.qos.accept.foreign.ip=false
```

**dubbo.properties**

```java
dubbo.application.qos.enable=true
dubbo.application.qos.port=33333
dubbo.application.qos.accept.foreign.ip=false
```

**XML**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://dubbo.apache.org/schema/dubbo http://dubbo.apache.org/schema/dubbo/dubbo.xsd">
  <dubbo:application name="demo-provider">
    <dubbo:parameter key="qos.enable" value="true"/>
    <dubbo:parameter key="qos.accept.foreign.ip" value="false"/>
    <dubbo:parameter key="qos.port" value="33333"/>
  </dubbo:application>
  <dubbo:registry address="multicast://224.5.6.7:1234"/>
  <dubbo:protocol name="dubbo" port="20880"/>
  <dubbo:service interface="org.apache.dubbo.demo.provider.DemoService" ref="demoService"/>
  <bean id="demoService" class="org.apache.dubbo.demo.provider.DemoServiceImpl"/>
</beans>
```

**spring-boot**

```sh
dubbo.application.qosEnable=true
dubbo.application.qosPort=33333
dubbo.application.qosAcceptForeignIp=false
```







# End