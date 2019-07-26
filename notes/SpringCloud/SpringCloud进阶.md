
### Hystrix
* 雪崩：服务器支持的线程和并发数有限，请求一直阻塞，会导致服务器资源耗尽，从而导致所有其它服务都不可用。
* Hustrix解决雪崩问题方法：
  - 线程隔离
  - 服务熔断
  
  服务降级：优先保证核心服务，而非核心服务不可用或弱可用
  触发Hystrix服务降级的情况：
    - 线程池已满
    - 请求超时
    
 * 实践  
 1、引入Hystrix依赖
 ```
 <dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
</dependency>
 ```
 
 2、开启熔断
 在主类上添加@EnableCircuitBreaker
 
 Spring提供了一个组合注解：@SpringCloudApplication,是一下三个注解的组合：  
 @SpringBootApplication  
 @EnableDiscoveryClient  
 @EnableCircuitBreaker
 
 
