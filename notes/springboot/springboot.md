# 默认配置原理
* springboot引导类@SpringBootApplication是多个注解共同组成  
  - @SpringBootConfiguration：声明当前类是SpringBoot应用的配置类，项目中只能有一个
  - @EnableAutoConfiguration：开启自动配置
  - @ComponentScan：开启注解扫描

