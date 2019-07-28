
Feign可以把Rest的请求进行隐藏，伪装成类似SpringMVC的Controller一样。你不用再自己拼接url，拼接参数等等操作，一切都交给Feign去做。  
步骤：  
### 简单实例
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
说明：1、首先这是一个接口，Feign会通过动态代理，生成实现类；  
2、``@FeignClient``，声明这是一个Feign客户端，类似@Mapper注解。同时通过value属性指定服务名称；  
3、接口中的定义方法，完全采用SpringMVC的注解，Feign会根据注解帮我们生成URL，并访问获取结果

### 负载均衡
Feign中本身已经集成了Ribbon依赖和自动配置
### Hystrix支持
Feign默认也有对Hystrix的集成，只不过，默认情况下是关闭的，如下开启
```java 
feign:
  hystrix:
    enabled: true # 开启Feign的熔断功能
```

Feign中Fallback定义
定义一个类UserClientFallback,实现上面的UserClient,作为fallback的处理类  
```java
@Component
public class UserClientFallback implements UserClient {

    @Override
    public User queryById(Long id) {
        User user = new User();
        user.setUserName("服务器繁忙，请稍后再试！");
        return user;
    }
}
```

改造UserFeignClient接口
```java
@FeignClient(value = "service-provider", fallback = UserClientFallback.class) // 标注该类是一个feign接口
public interface UserClient {

    @GetMapping("user/{id}")
    User queryUserById(@PathVariable("id") Long id);
}
```
