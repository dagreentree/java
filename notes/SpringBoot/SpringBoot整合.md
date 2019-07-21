
SpringBoot整合
* [整合SpringMVC](#整合SpringMVC)
* [添加拦截器](#添加拦截器)
* [整合连接池](#整合连接池)
* [整合mybatis](#整合mybatis)
----------------------
### 整合SpringMVC
* 访问静态资源  
默认的静态资源路径为：
  - classpath:/META-INF/resources/
  - classpath:/resources/
  - classpath:/static/
  - classpath:/public/
### 添加拦截器
* 定义一个拦截器
```
@Component
public class MyInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("preHandle method is running!");
        return true;
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("postHandle method is running!");
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("afterCompletion method is running!");
    }
}
```
  
  
* 定义配置类，注册拦截器
```
@Configuration
public class MvcConfiguration implements WebMvcConfigurer {

    @Autowired
    private HandlerInterceptor myInterceptor;

    /**
     * 重写接口中的addInterceptors方法，添加自定义拦截器
     * @param registry
     */
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(myInterceptor).addPathPatterns("/**");
    }
}
```
 springMVC记录的log级别是debug，springboot默认是显示info以上，我们需要进行配置。  
 SpringBoot通过logging.level.*=debug来配置日志级别，*填写包名
 
 ### 整合连接池
 * 在pom.xml中引入jdbc的启动器
 ```
 <!--jdbc的启动器，默认使用HikariCP连接池-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>
<!--不要忘记数据库驱动，因为springboot不知道我们使用的什么数据库，这里选择mysql-->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
</dependency>
 ```
 * 指定连接池参数
 ```
 # 连接四大参数
spring.datasource.url=jdbc:mysql://localhost:3306/heima
spring.datasource.username=root
spring.datasource.password=root
# 可省略，SpringBoot自动推断
spring.datasource.driverClassName=com.mysql.jdbc.Driver

spring.datasource.hikari.idle-timeout=60000
spring.datasource.hikari.maximum-pool-size=30
spring.datasource.hikari.minimum-idle=10
 ```
 
 * Druid连接池
 ```
 <!-- Druid连接池 -->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid-spring-boot-starter</artifactId>
    <version>1.1.6</version>
</dependency>
 ```
 
 ```
 #初始化连接数
spring.datasource.druid.initial-size=1
#最小空闲连接
spring.datasource.druid.min-idle=1
#最大活动连接
spring.datasource.druid.max-active=20
#获取连接时测试是否可用
spring.datasource.druid.test-on-borrow=true
#监控页面启动
spring.datasource.druid.stat-view-servlet.allow=true
 ```
 
 ### 整合mybatis
 ```
 <!--mybatis -->
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>1.3.2</version>
</dependency>
 ```
 
 ```
 # mybatis 别名扫描
mybatis.type-aliases-package=cn.itcast.pojo
# mapper.xml文件位置,如果没有映射文件，请注释掉
mybatis.mapper-locations=classpath:mappers/*.xml
 ```
 
 
 
