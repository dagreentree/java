
SpringBoot整合
* [整合SpringMVC](#整合SpringMVC)
* [添加拦截器](#添加拦截器)
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
