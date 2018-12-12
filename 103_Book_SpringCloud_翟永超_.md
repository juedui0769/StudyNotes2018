# 103_Book_SpringCloud_翟永超_

- 《Spring Cloud 微服务实战》
- 作者： 翟永超
- 电子工业出版社
- 2017.05
- 版本
  - 本书所有示例 `Brixton.SR5` 
  - `Spring Boot 1.3.7`
- 我使用的版本
  - `Spring Boot 2.1.0.RELEASE`

## 第2章 SpringBoot

- 使用随机数
  - 用到时，可以参考、复习一下！
- 多环境配置
  - `application-{profile}.properties`
  - `spring.profiles.active=test`
  - `java -jar xx.jar --spring.profiles.active=test`
- 原生端点
  - https://docs.spring.io/spring-boot/docs/2.1.1.RELEASE/reference/htmlsingle/#production-ready-endpoints  此URL有详细描述，可前往参考。
  - 应用配置类
    - 书中描述的`/autoconfig` 在新版本已经没有了
      - `http://localhost:8080/actuator/conditions` , `/actuator/conditions`中可以可以看到类似的信息
    - `/beans`
    - `/configprops`
    - `/env`
    - `/mappings`
    - `/info`
  - 度量指标类
    - `/metrics` : 和书上写的完全不一样了。
    - `/health`
      - `HealthIndicator`接口
    - `/dump` 已经没有了
      - `heapdump` : 点击之后，下载一个二进制文件
      - `threaddump` :  是一个可以打开的链接，非常多的内容。
    - `/trace` 已经没有了
      - `httptrace` ： 这个API挺厉害的，可以返回最近访问的http请求，相当于是Linux下的`history`了。
  - 操作控制类
    - `/shutdown`  ： 默认是关闭的，而且和书上的描述不一样，
      - 新版中需要在 `applicaiton.properties`中配置 `management.endpoint.shutdown.enabled = true`
      - `endpoints.shutdown.enabled` 已经被废弃了 `"deprecated": true`
      - 开启之后，就可以通过`http://localhost:8080/actuator/shutdown`来远程关闭应用了，但是必须是`POST`请求，`GET`请求不被接受（服务器端会有报错日志 `Request method 'GET' not supported`）。可以用"Postman"来模拟`POST`请求。
- end



## 第3章 Eureka

- `2.0.2.RELEASE`

### 官网

- https://cloud.spring.io/spring-cloud-static/spring-cloud-netflix/2.0.2.RELEASE/single/spring-cloud-netflix.html







## 第4章 Ribbon



## 第5章 Hystrix



## 第6章 Feign



## 第7章 Zuul



## 第8章 Spring Cloud Config



## 第9章 Spring Cloud Bus



## 第10章 Spring Cloud Stream



## 第11章 Spring Cloud Sleuth



























































