* [Hystrix](#Hystrix)
* [服务降级](#服务降级)
* [服务熔断](#服务熔断)
---------------
### Hystrix
* 雪崩：服务器支持的线程和并发数有限，请求一直阻塞，会导致服务器资源耗尽，从而导致所有其它服务都不可用。
* Hustrix解决雪崩问题方法：
  - 线程隔离
  - 服务熔断
  
### 服务降级：  
  优先保证核心服务，而非核心服务不可用或弱可用
  触发Hystrix服务降级的情况：
    - 线程池已满
    - 请求超时
    
 * 实践  
 1、引入Hystrix依赖
 ```xml
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
 
 3、设置超时  
 Hystrix默认得超时时长为1，可通过设置修改这个值
 ```yml
 hystrix:
  command:
    default:
      execution:
        isolation:
          thread:
            timeoutInMilliseconds: 6000 # 设置hystrix的超时时间为6000ms
 ```           
 
 ### 服务熔断
 1、熔断的3个状态  
    - Closed:关闭状态，所有请求都正常访问  
    - Open:打开状态，所有请求都会被降级
