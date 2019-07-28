
Feign可以把Rest的请求进行隐藏，伪装成类似SpringMVC的Controller一样。你不用再自己拼接url，拼接参数等等操作，一切都交给Feign去做。  
步骤：  
1、导入依赖
```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-openfeign</artifactId>
</dependency>
```

2、开启Feign  
在启动类上，添加注解  
```java 
@EnableFeignClients
```

3、编码
```java
@FeignClient(value = "service-provider") // 标注该类是一个feign接口
public interface UserClient {

    @GetMapping("user/{id}")
    User queryById(@PathVariable("id") Long id);
}
```
说明：1、首先这是一个接口，Feign会通过动态代理，生成实现类；2、@FeignClient，声明这是一个Feign客户端，类似@Mapper注解。同时通过value属性指定服务名称；3、接口中的定义方法，完全采用SpringMVC的注解，Feign会根据注解帮我们生成URL，并访问获取结果
