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













