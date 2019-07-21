* [默认配置原理](#默认配置原理)
  * [引导类](#引导类)
  * [java配置](#java配置)
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
解读：

- @Configuration：声明JdbcConfiguration是一个配置类。
- @PropertySource：指定属性文件的路径是:classpath:jdbc.properties
- 通过@Value为属性注入值。
- 通过@Bean将 dataSource()方法声明为一个注册Bean的方法，Spring会自动调用该方法，将方法的返回值加入Spring容器中。相当于以前的bean标签

然后就可以在任意位置通过@Autowired注入DataSource了！

### SpringBoot的属性注入
java配置方式属性注入使用的是@Value注解。这种方式虽然可行，但是不够强大，因为它只能注入基本类型值。  
在SpringBoot中，提供了一种新的属性注入方式，支持各种java基本数据类型及复杂类型的注入。
* 新建JdbcProperties类，用来进行属性注入
```
@ConfigurationProperties(prefix = "jdbc")
public class JdbcProperties {
    private String url;
    private String driverClassName;
    private String username;
    private String password;
    // ... 略
    // getters 和 setters
}
```
* 在JdbcConfiguration中使用这个属性
1、Autowired注入
```
@Configuration
@EnableConfigurationProperties(JdbcProperties.class)
public class JdbcConfiguration {

    @Autowired
    private JdbcProperties jdbcProperties;

    @Bean
    public DataSource dataSource() {
        DruidDataSource dataSource = new DruidDataSource();
        dataSource.setUrl(jdbcProperties.getUrl());
        dataSource.setDriverClassName(jdbcProperties.getDriverClassName());
        dataSource.setUsername(jdbcProperties.getUsername());
        dataSource.setPassword(jdbcProperties.getPassword());
        return dataSource;
    }

}
```
2、构造函数注入
```
@Configuration
@EnableConfigurationProperties(JdbcProperties.class)
public class JdbcConfiguration {

    private JdbcProperties jdbcProperties;

    public JdbcConfiguration(JdbcProperties jdbcProperties){
        this.jdbcProperties = jdbcProperties;
    }

    @Bean
    public DataSource dataSource() {
        // 略
    }

}
```
3、@Bean方法的参数注入
```
@Configuration
@EnableConfigurationProperties(JdbcProperties.class)
public class JdbcConfiguration {

    @Bean
    public DataSource dataSource(JdbcProperties jdbcProperties) {
        // ...
    }
}
```
4、更优雅的注入  
事实上，如果一段属性只有一个Bean需要使用，我们无需将其注入到一个类（JdbcProperties）中。而是直接在需要的地方声明即可：
```
@Configuration
public class JdbcConfiguration {
    
    @Bean
    // 声明要注入的属性前缀，SpringBoot会自动把相关属性通过set方法注入到DataSource中
    @ConfigurationProperties(prefix = "jdbc")
    public DataSource dataSource() {
        DruidDataSource dataSource = new DruidDataSource();
        return dataSource;
    }
}
```
我们直接把@ConfigurationProperties(prefix = "jdbc")声明在需要使用的@Bean的方法上，然后SpringBoot就会自动调用这个Bean（此处是DataSource）的set方法，然后完成注入。使用的前提是：该类必须有对应属性的set方法！



