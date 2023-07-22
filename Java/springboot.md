## 一.attention

##### 1.spring boot进行定时任务

**基于注解 (@Scheduled)的方式**

启动类中用注解***@EnableScheduling***进行标注，表明此类 存在定时任务。在定时执行的方法之上添加注解

```html
@Scheduled(cron = "0 0 0 * * ?") // 每天午夜执行
```

启动类需要能扫描到定时任务类，否则定时任务启动不起来。不仅需要@Component注解，也需要将启动类位置位于定时任务类之上。

##### 2.关于在拦截器上使用@component不生效的问题

实现拦截器的方式很简单，主要由以下两个步骤：

1. 自定义拦截器类实现HandlerInterceptor接口

   ```Java
   public class LoginInterceptor implements HandlerInterceptor {
   
       private StringRedisTemplate stringRedisTemplate;
   
       public LoginInterceptor(StringRedisTemplate stringRedisTemplate) {
           this.stringRedisTemplate = stringRedisTemplate;
       }
   
       @Override
       public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
           //1.获取请求头中的token
           HttpSession session = request.getSession();
           //2.获取redis中的用户
           Object user = session.getAttribute("user");
           //3.判断用户是否存在
           if (user == null) {
               //4.不存在，拦截
               response.setStatus(401);
               return false;
           }
           //将查询到的数据转为userDto对象
   
           //5.存在,保存到ThreadLocal
           UserHolder.saveUser((UserDTO) user);
   
           //TODO：刷新token有效期
           //6.放行
           return true;
       }
   
       @Override
       public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
           UserHolder.removeUser();
       }
   }
   
   ```

   

2. 自定义WebMvc配置类实现WebMvcConfigurer接口，添加自定义拦截器类

   ```Java
   @Configuration
   public class MvcConfig implements WebMvcConfigurer {
   
   
       @Resource
       private StringRedisTemplate stringRedisTemplate;
   
       @Override
       public void addInterceptors(InterceptorRegistry registry) {
           registry.addInterceptor(new LoginInterceptor(stringRedisTemplate))
                   .excludePathPatterns(
                           "/shop/**",
                           "/voucher/**",
                           "/upload/**",
                           "/blog/hot",
                           "/user/code",
                           "/user/login"
                   );
       }
   }
   ```

   网上关于拦截器的代码基本都是通过new一个拦截器进行配置的，这时候就会出现无法注入其他bean的情况。很多人想当然地直接在拦截器加@Component注解使其成为一个bean对象。这是一种错误的做法。我们需要保证的是在WebMvc配置类中添加的拦截器是Spring 的一个bean对象，也就是说我们需要将拦截器注成一个bean，同时将这个bean添加的WebMvc配置类中。

   解决方案如下：

   https://blog.csdn.net/JavaMonsterr/article/details/125805757?ops_request_misc=&request_id=&biz_id=102&utm_term=%E6%8B%A6%E6%88%AA%E5%99%A8%E7%9A%84%E7%B1%BB%E4%B8%AD%E4%B8%BA%E4%BB%80%E4%B9%88%E4%B8%8D%E8%83%BD%E6%B3%A8%E5%85%A5,%E4%B8%8E%E5%8A%A0%E4%BA%86@Component%E6%B3%A8%E8%A7%A3%E7%9A%84&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-1-125805757.142^v90^insert_down28v1,239^v2^insert_chatgpt&spm=1018.2226.3001.4187

## 注：小知识点

#### 1.ctrl+ F 快速查找命令

#### 2.打开服务services.msc

#### 3.ctrl+ R全文替换单词

#### 4.Java基础中的集合

#### 5.spring中的依赖注入

#### 6.springmvc中的modelAndView

#### 7.netstat -ano |findstr "80"查看对应的端口号

#### 8.taskkill /t /f /im ”TCP号“

#### 9.nohup java -jar your-application.jar > output.log &