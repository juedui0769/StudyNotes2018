# Maven

https://gitee.com/juedui0769/MavenStudy ，之前的笔记，记录在这里，但是需要再整理。应该整理得更干净、简洁一些。



## Maven实战

- Maven 实战
- 徐晓斌 著
- 2010.11
- 机械工业出版社
- 2011年1月第1版第1次印刷

## 超级POM

在《Maven实战》8.5节，“约定优于配置”中，提到了超级POM。

超级POM在文件`$MAVEN_HOME/lib/maven-model-builder-x.x.x.jar`中的`org/apache/maven/model/pom-4.0.0.xml`路径下。

我本地安装的是“Maven3.3.9”中，解压jar，打开这个文件，包括空白行和注释，共151行，内容不多，但下面这段内容比较有价值，定义了一个maven项目的基本信息：

```xml
<build>
    <directory>${project.basedir}/target</directory>
    <outputDirectory>${project.build.directory}/classes</outputDirectory>
    <finalName>${project.artifactId}-${project.version}</finalName>
    <testOutputDirectory>${project.build.directory}/test-classes</testOutputDirectory>
    <sourceDirectory>${project.basedir}/src/main/java</sourceDirectory>
    <scriptSourceDirectory>${project.basedir}/src/main/scripts</scriptSourceDirectory>
    <testSourceDirectory>${project.basedir}/src/test/java</testSourceDirectory>
    <resources>
        <resource>
            <directory>${project.basedir}/src/main/resources</directory>
        </resource>
    </resources>
    <testResources>
        <testResource>
            <directory>${project.basedir}/src/test/resources</directory>
        </testResource>
    </testResources>
    ...
</build>
```



## Maven属性

《Maven实战》 14.1 Maven属性

1. 内置属性： 两个常用内置属性，`${basedir}`，`${version}`
2. POM属性：很多，见下文介绍
3. 自定义属性：这个不用多说，比如`<spring.version>...`
4. Settings属性：没怎么在意
5. Java系统属性：和下面的一样，可以通过`mvn help:system` 查看到
6. 环境变量属性：

POM属性，列举如下：

| No.  | 属性                                   | 描述                                                         |
| ---- | -------------------------------------- | ------------------------------------------------------------ |
| 1    | `${project.artifactId}`                | 对应`<project><artifactId>`元素的值                          |
| 2    | `${project.build.sourceDirectory}`     | 项目的主源码目录                                             |
| 3    | `${project.build.testSourceDirectory}` | 项目的测试源码目录                                           |
| 4    | `${project.build.directory}`           | 项目构建输出目录                                             |
| 5    | `${project.outputDirectory}`           | 项目主代码编译输出目录                                       |
| 6    | `${project.testOutputDirectory}`       | 项目测试代码编译输出目录                                     |
| 7    | `${project.groupId}`                   | 项目的groupId                                                |
| 8    | `${project.artifactId}`                | 项目的artifactId                                             |
| 9    | `${project.version}`                   | 项目的version，与`${version}`等价                            |
| 10   | `${project.build.finalName}`           | 项目打包输出文件的名称，默认为 `${project.artifactId} - ${project.version}` |



## Maven Profile

http://maven.apache.org/guides/introduction/introduction-to-profiles.html ， 这是官方文档，在百度搜索“maven profile”，就可以定位到。

在《Maven实战》，14.4 Maven Profile，有介绍。



## Maven filter

~~参考： http://maven.apache.org/pom.html ， 搜索 `filter~~`

更详细的描述参考： http://maven.apache.org/guides/getting-started/index.html#How_do_I_filter_resource_files

```java
# filter.properties
my.filter.value=hello!
```

```java
# application.properties
application.name=${project.name}
application.version=${project.version}
message=${my.filter.value}
```

```xml
  <build>
    <filters>
      <filter>src/main/filters/filter.properties</filter>
    </filters>
    <resources>
      <resource>
        <directory>src/main/resources</directory>
        <filtering>true</filtering>
      </resource>
    </resources>
  </build>
```

执行：

```sh
mvn process-resources
```

将得到：（`taget/classes`）

```java
# application.properties
application.name=Maven Quick Start Archetype
application.version=1.0-SNAPSHOT
message=hello!
```















# End