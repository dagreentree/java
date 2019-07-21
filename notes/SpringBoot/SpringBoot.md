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
### 连接池配置
* 在pom中，引入Druid连接池依赖
```
<dependency>
    <groupId>com.github.drtrang</groupId>
    <artifactId>druid-spring-boot2-starter</artifactId>
    <version>1.1.10</version>
</dependency>
```
* 添加数据源  
创建Jdbc.properties
```
jdbc.driverClassName=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://127.0.0.1:3306/leyou
jdbc.username=root
jdbc.password=123
```
* 配置数据源  
创建JdbcConfiguration类
```
@Configuration
@PropertySource("classpath:jdbc.properties")
public class JdbcConfiguration {

    @Value("${jdbc.url}")
    String url;
    @Value("${jdbc.driverClassName}")
    String driverClassName;
    @Value("${jdbc.username}")
    String username;
    @Value("${jdbc.password}")
    String password;

    @Bean
    public DataSource dataSource() {
        DruidDataSource dataSource = new DruidDataSource();
        dataSource.setUrl(url);
        dataSource.setDriverClassName(driverClassName);
        dataSource.setUsername(username);
        dataSource.setPassword(password);
        return dataSource;
    }
}
```

