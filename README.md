# SpringMVC
Spring MVC 应用的开发步骤

1、在web.xml文件中定义前端控制器DispatcherServlet来拦截用户请求
 <!-- 定义Spring MVC的前端控制器 -->
  <servlet>
  	<servlet-name>springmvc</servlet-name>
  	<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
  	<init-param>
  		<param-name>contextConfigLocation</param-name>
  		<param-value>/WEB-INF/springmvc-config.xml</param-value>
	</init-param>
	<load-on-startup>1</load-on-startup>
  </servlet>
  <!-- 让Spring MVC的前端控制器拦截所有请求 -->
  <servlet-mapping>
  	<servlet-name>springmvc</servlet-name>
  	<url-pattern>/</url-pattern>
  </servlet-mapping>
  
  2、如果需要以post方式提交请求，则定义包含表单数据的jsp页面。如果仅仅只是以get方式发送请求，则无需经过这一步。
  
  3、定义处理用户请求的Handle类。可以实现Controller接口或使用@Controller注解。
  
  4、配置Handle。
  @Controller
  public class HelloController{
    private static final Log logger = LogFactory.getLog(HelloController.class);

    @RequestMapping(value="/hello")
    public ModelAndView handleRequest(HttpServletRequest request,HttpServletResponse response){
    ......
    
  5、编写视图资源。
  
  
  <!-- 配置Handle,映射"/hello"请求 -->
	<bean name="/hello" class="org.fkit.controller.HelloController" />
	<!-- 处理映射器将bean的name作为url进行查找，需要在配置Handle时制定name（即url） -->
	<bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping" />
	<!-- SimpleControllerHandlerAdapter是一个处理器适配器，所有处理适配器都要实现HandlerAdapter接口 -->
	<bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter" />
	<!-- 视图解析器 -->
	<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" />
  
  以下方法配置springmvc的配置文件，则可以直接使用注解，不用再实现Controller接口。
  <!-- spring可以自动去扫描base-pack下面的包或者子包下面的java文件，如果扫描到有spring 的相关注解的类，则把这些类注册为spring的bean -->
  <context:component-scan base-package="org.fkit.controller" />
  
  <!-- 配置annotation类型的处理器映射器  -->
	<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping" />
  
  <!-- 配置annotation类型的处理器适配器  -->
	<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter" />
  
  <!-- 试图解析器 -->
	<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" />
