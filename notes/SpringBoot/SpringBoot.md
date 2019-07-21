# 默认配置原理
### 引导类
* springboot引导类@SpringBootApplication是多个注解共同组成  
  - @SpringBootConfiguration：声明当前类是SpringBoot应用的配置类，项目中只能有一个
  - @EnableAutoConfiguration：开启自动配置
  - @ComponentScan：开启注解扫描
### java配置
* 常用注解
  - `` @Configuration ``:声明一个类作为配置类
  - `` @Bean``:生命在方法上，将方法的返回值加入Bean容器
  - `` @Value``:属性注入
  - `` @PropertySource``:指定外部属性文件

