> 106_SpringBoot_学习记录.md

### starters

以`spring-boot-starter-tomcat`和`spring-boot-starter-web`为例

**spring-boot-starter-tomcat-2.0.2.RELEASE.jar**

```
spring-boot-starter-tomcat-2.0.2.RELEASE.jar
|- META-INF
    |- MANIFEST.MF
    |- spring.provides
```

```
# MANIFEST.MF
Manifest-Version: 1.0
Implementation-Title: Spring Boot Tomcat Starter
Automatic-Module-Name: spring.boot.starter.tomcat
Implementation-Version: 2.0.2.RELEASE
Built-By: Spring
Created-By: Apache Maven 3.5.3
Build-Jdk: 1.8.0_141

# spring.provides
provides: tomcat-embed-core
```

**spring-boot-starter-web-2.0.2.RELEASE.jar**

```
spring-boot-starter-web-2.0.2.RELEASE.jar
|- META-INF
    |- MANIFEST.MF
    |- spring.provides
```

```
# MANIFEST.MF
Manifest-Version: 1.0
Implementation-Title: Spring Boot Web Starter
Automatic-Module-Name: spring.boot.starter.web
Implementation-Version: 2.0.2.RELEASE
Built-By: Spring
Created-By: Apache Maven 3.5.3
Build-Jdk: 1.8.0_141

# spring.provides
provides: spring-webmvc,spring-web
```

> `tomcat-embed-core`, `spring-webmvc`, `spring-web` 可以在 <https://mvnrepository.com> 检索到

```xml
<dependency>
    <groupId>org.apache.tomcat.embed</groupId>
    <artifactId>tomcat-embed-core</artifactId>
    <version>9.0.19</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.1.6.RELEASE</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-web</artifactId>
    <version>5.1.6.RELEASE</version>
</dependency>
```

### spring.provides

"META-INF/spring.provides"文件的内容常常就是一行，比如 `provides: spring-webmvc,spring-web`。
但是这个文件的具体作用是什么呢？这个内容很像在<https://start.spring.io/>上填写的内容。

<https://github.com/spring-projects/spring-boot/issues/1926>
- 这个github的issue上描述的是`spring.provides`文件是为IDE服务的。

### spring-boot-parent

"D:\MavenRepository\" 目录是我本地的Maven仓库。

"D:\MavenRepository\org\springframework\boot\spring-boot-parent\2.0.2.RELEASE"
- 此目录下没有jar文件，只有`spring-boot-parent-2.0.2.RELEASE.pom`文件和另两个次要文件

`spring-boot-parent-2.0.2.RELEASE.pom`文件中的如下内容值得注意

```xml
  <parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-dependencies</artifactId>
    <version>2.0.2.RELEASE</version>
    <relativePath>../spring-boot-dependencies</relativePath>
  </parent>
```

### spring-boot-dependencies

承上，`spring-boot-parent`的parent是`spring-boot-dependencies`

"spring-boot-dependencies-2.0.2.RELEASE.pom"文件共`3023`行, "dependencyManagement"和"pluginManagement"中定义了很多组件
- "dependencyManagement"
- "pluginManagement"


------

> 2019年4月21日22:22:43， 
> 
> 初步对"META-INF/spring.provides"有了些了解
>
> 对"spring-boot-parent", "spring-boot-dependencies"的关系有了进一步的了解

------









