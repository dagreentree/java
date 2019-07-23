* [服务调用方式](#服务调用方式)
-----
### 服务调用方式
  * RPC和HTTP  
    - RPC:Remote Produce Call远程过程调用，类似的还有RMI。自定义数据格式，基于原生TCP通信，速度快，效率高.
    - Http:http其实是一种网络传输协议，基于TCP，规定了数据传输的格式。现在客户端浏览器与服务端通信基本都是采用Http协议，也可以用来进行远程服务调用。缺点是消息封装臃肿，优势是对服务的提供和调用方没有任何技术限定，自由灵活，更符合微服务理念。
    
  * SpringCloud认识  
    官网地址：http://projects.spring.io/spring-cloud/  
    主要功能：配置管理、服务发现、智能路由、负载均衡、熔断器、控制总线、集群状态...  
    主要组件：Eureka、Zuul、Ribbon、Feign、Hystrix
    
    版本：如Angel.SR6  
    版本名：是伦敦的地铁名 
    版本号：SR（Service Releases）是固定的 ,大概意思是稳定版本。后面会有一个递增的数字。
    
  * Eureka注册中心
  原理图：
  ![image](https://github.com/dagreentree/java/blob/master/notes/SpringCloud/pic/1525597885059.png)

    
    
    

  
