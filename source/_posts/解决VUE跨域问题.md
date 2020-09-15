---
title:  解决VUE跨域问题添加配置类
date: 2019.07.01 10:06
tags:
  - vue
---



## 解决VUE跨域问题添加配置类

![vue跨域.png](https://myblog-1252842020.cos.ap-guangzhou.myqcloud.com/vue%E8%B7%A8%E5%9F%9F_1574558544298.png)
代码内容
```java
@Configuration
public class WebConfigurer implements WebMvcConfigurer{
    @Autowired
    private CrossDomain crossDomain;
    @Override
    public void addInterceptors(InterceptorRegistry registry) {  registry.addInterceptor(crossDomain).addPathPatterns("/**").excludePathPatterns("/xxxxxxxxx6666");
    }
}
```
> excludePathPatterns("/xxxxxxxxx6666"): 
相对于一个靶子,除了这个路径下的文件,其他都可以进入
```java
@Component
public class CrossDomain extends HandlerInterceptorAdapter {
    public CrossDomain() {
        super();
    }
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        //是否携带cookie
        response.setHeader("Access-Control-Allow-Credentials", "true");
        //允许跨域的请求方法GET、POST、HEAD等
        response.setHeader("Access-Control-Allow-Methods", "*");
        //重新预检验跨域的缓存时间(s)Access-Control-Allow-Origin
        response.setHeader("Access-Control-Max-Age", "3600");
        //允许跨域的请求头
        response.setHeader("Access-Control-Allow-Headers", "x-requested-with,content-type");
        response.setHeader("Access-Control-Allow-Origin", request.getHeader("Origin"));
        return true;
    }
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        super.postHandle(request, response, handler, modelAndView);
    }
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        super.afterCompletion(request, response, handler, ex);
    }
    @Override
    public void afterConcurrentHandlingStarted(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        super.afterConcurrentHandlingStarted(request, response, handler);
    }
}
```