# SpringMVC

## Book01

《看透Spring MVC: 源代码分析与实践》

韩路彪 著

2016年1月第1版第1次印刷

机械工业出版社

### 知识点索引

1.5.4 反向代理， P14 反向代理服务器和代理服务器的区别

### uml图

#### Servlet

![](./imgs/120_interface_servlet.png)

P37 6.1 Servlet 接口， 如上图

`init()`方法在容器启动时被容器调用，只会调用一次；（当`load-on-startup`设置为负数或者不设置时会在Servlet第一次用到时被调用）

`getServletConfig()`方法用于获取ServletConfig；

`service()`方法用于具体处理一个请求；

`getServletInfo()`方法可以获取一些Servlet相关的信息，这个方法需要自己实现，默认返回空字符串；

`destroy()`方法用于在Servlet销毁（一般指关闭服务器）时释放一些资源，只会调用一次。

-----

`init()`方法被调用时会接收到一个`ServletConfig`类型的参数，是容器传进去的。比如 Spring MVC 的`contextConfigLocation`参数就保存在`ServletConfig`中，配置如下：

```xml
<servlet>
    <servlet-name>demoDispatcher</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
        <param-name>contextConfiguration</param-name>
        <param-value>demo-servlet.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
</servlet>
```





## tomcat

### 嵌入式使用

参考： http://www.javacreed.com/how-to-run-embedded-tomcat-with-maven/

```xml
<build>
	<finalName>springMvc</finalName>
	<plugins>
		<plugin>
			<groupId>org.apache.tomcat.maven</groupId>
			<artifactId>tomcat7-maven-plugin</artifactId>
			<version>2.2</version>
			<configuration>
				<port>9090</port>
				<path>/</path>
			</configuration>
		</plugin>
	</plugins>
</build>
```

如上，需要将以上maven插件添加到工程中。至于`configuration`下还能配置哪些其他的元素，我没有找到答案。现在，上面的配置是参考上面的链接得到的。

```sh
mvn clean package

mvn tomcat7:run
```

按以上的命令启动tomcat，在浏览器访问 http://localhost:9090/

使用 https://google.suanfazu.com/ 搜索谷歌！













# End