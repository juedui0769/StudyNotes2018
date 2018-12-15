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

## dubbo-samples-annotation

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









# End