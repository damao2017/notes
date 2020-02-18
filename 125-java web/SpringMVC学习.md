# SpringMVC学习

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:p="http://www.springframework.org/schema/p"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:mvc="http://www.springframework.org/schema/mvc"
	xmlns:task="http://www.springframework.org/schema/task"
	xsi:schemaLocation="
        http://www.springframework.org/schema/beans 
        http://www.springframework.org/schema/beans/spring-beans-4.2.xsd 
        http://www.springframework.org/schema/context 
        http://www.springframework.org/schema/context/spring-context-4.2.xsd 
        http://www.springframework.org/schema/mvc 
        http://www.springframework.org/schema/mvc/spring-mvc-4.2.xsd 
        http://www.springframework.org/schema/task 
        http://www.springframework.org/schema/task/spring-task-4.2.xsd">

	<!-- 直接扫描controller包 -->
	<context:component-scan base-package="com.ntkd.usercontroller"></context:component-scan>

	<!-- mvc注解驱动，使用这个配置，可以代替上面两条注解映射器，适配器配置 
	还默认加载了很多参数的绑定方法，比如json转换解析器，默认加载了-->
	<mvc:annotation-driven></mvc:annotation-driven> 
	<!--静态资源过滤 
	<mvc:resources location="" mapping=""></mvc:resources>
	 -->
	<!-- 视图解析器 -->
	<!-- 默认可以解析jstl标签 -->
	<bean
		class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<!-- 视图解析器的前缀和后缀，这样在setview可以少写一点路径 -->
		<property name="prefix" value="/WEB-INF/jsp/" />
		<property name="suffix" value=".jsp" />
	</bean>
</beans>
```

传值

```java
@RequestMapping("demo1")
	public String demo1(HttpServletRequest req) {
		//传值1
		req.setAttribute("name", "aaa");
		return "/index.jsp";
	}
	
	@RequestMapping("demo2")
	public String demo2(Model model) {
		//传值2
		model.addAttribute("name", "aaa");
		return "/index.jsp";
	}
	
	@RequestMapping("demo3")
	public ModelAndView demo3() {
		//传值3
		ModelAndView mav = new ModelAndView("/index.jsp");
		mav.addObject("name", "jack");
		return mav;
	}
```

###  **路径问题** 

 文件放在webapps下和web-inf目录下，web-inf目录里面的jsp怎么引用。

```xml
<!--springmvc配置﻿-->
<mvc:annotation-driven/> 
<!-- 指定目录和映射的对应关系  -->
<mvc:resources location="/static/" mapping="/static/**" />
<mvc:resources location="/WEB-INF/js/" mapping="/js/**" />

<!-- 拦截器也要设置，设置放行映射路径	 -->
<mvc:interceptors>
	 <mvc:interceptor>
	 	<mvc:mapping path="/**"/>
	 	<mvc:exclude-mapping path="/static/**"/>
	 	<mvc:exclude-mapping path="/js/**"/>
	 	<bean class="com.ntkd.user.interceptor.UserInterceptor"/>
	 </mvc:interceptor>
</mvc:interceptors>

 <!-- html引用方法	 -->
<img src="/SSM/static/test.png" alt="aaa" /><br />
<img src="/SSM/js/test.png" alt="aaa" /><br />

```

