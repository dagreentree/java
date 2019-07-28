
### 快速入门
1、添加Zuul依赖
```java
server:
  port: 10010 #服务端口
spring:
  application:
    name: api-gateway #指定服务名
```

2、编写引导类
通过``@EnableZuulProxy``注解开启Zuul的功能

3、编写路由规则  
将符合path 规则的一切请求，都代理到 url参数指定的地址
```java
server:
  port: 10010 #服务端口
spring:
  application:
    name: api-gateway #指定服务名
zuul:
  routes:
    service-provider: # 这里是路由id，随意写
      path: /service-provider/** # 这里是映射路径
      url: http://127.0.0.1:8081 # 映射路径对应的实际url地址
```

