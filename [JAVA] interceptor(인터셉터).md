Interceptor.java

```java

@Component
public class CertificationInterceptor implements HandlerInterceptor{
	

	
	@Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
            throws Exception {
        HttpSession session = request.getSession();
        System.out.println(session.getId());
        System.out.println("interceptor동작확인");
        if(true){
            return true;
        }else{
            session.setMaxInactiveInterval(30*60);
            return true;
        }
    }
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler,
            ModelAndView modelAndView) throws Exception {
        // TODO Auto-generated method stub
        
    }
 
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex)
            throws Exception {
        // TODO Auto-generated method stub
        
    }
}

```

webConfig.java

```java
@Configuration
public class WebMvcConfig implements WebMvcConfigurer{
	
	@Autowired
	@Qualifier(value = "certificationInterceptor")
	private HandlerInterceptor interceptor;
	
	@Override
	public void addInterceptors(InterceptorRegistry registry) {
		registry.addInterceptor(interceptor)
		.addPathPatterns("/**");
	}
}

```

