# Spring Cloud



## 资源

### 文档

官网文档地址： https://cloud.spring.io/spring-cloud-static/Finchley.SR2/single/spring-cloud.html

### 书籍

- Book01, 《Spring Cloud 微服务实战》
  - 作者： 翟永超 ， 电子工业出版社， 2017.05 出版
  - 版本： `Spring Cloud Brixton.SR5` , `Spring Boot 1.3.7`
- Book02, 《Spring Cloud 微服务架构 开发实战》
  - 作者：柳伟卫， 北京大学出版社， 2018.06 出版
  - 版本： `Spring Cloud Finchley.M2` , `Spring Boot 2.0.0.M4`

### Client Side Usage

<https://cloud.spring.io/spring-cloud-static/Finchley.SR2/single/spring-cloud.html#_client_side_usage>

在此链接，首次发现`pom.xml`的配置

```xml
   <parent>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-parent</artifactId>
       <version>{spring-boot-docs-version}</version>
       <relativePath /> <!-- lookup parent from repository -->
   </parent>

<dependencyManagement>
	<dependencies>
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-dependencies</artifactId>
			<version>{spring-cloud-version}</version>
			<type>pom</type>
			<scope>import</scope>
		</dependency>
	</dependencies>
</dependencyManagement>

<dependencies>
	<dependency>
		<groupId>org.springframework.cloud</groupId>
		<artifactId>spring-cloud-starter-config</artifactId>
	</dependency>
	<dependency>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-test</artifactId>
		<scope>test</scope>
	</dependency>
</dependencies>

<build>
	<plugins>
           <plugin>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-maven-plugin</artifactId>
           </plugin>
	</plugins>
</build>

   <!-- repositories also needed for snapshots and milestones -->
```

### Jersey

https://jersey.github.io/

- https://Jersey.java.net 会重定向到上面的链接
- Copyright ©2010-2018 Oracle Corporation

Jersey是JAX-RS（JSR311）开源参考实现用于构建RESTful Web service





























