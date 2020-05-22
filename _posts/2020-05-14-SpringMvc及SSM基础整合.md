---
   layout: post
   title: "SpringMvc及SSM基础整合摘要"                                                        
   date: 2020-05-14 09:00:00 +0530
   categories: JAVA SpringMvc SSM
---
  JAVA SpringMvc SSM基础整合


# SpringMvc及SSM基础整合


* 基础

```
javaSE

javaWeb

SSM框架
```

## Mvc

```
模型(model)			dao(数据),service(服务行为) 		显示数据和行为
						vo,dto
视图(view)			jsp    								展示界面

控制器(controller)		Servlet    (调度员) 		   	 接收前端数据  妆发和重定向
```

* 业务逻辑,数据,显示分离
* 降低了视图与业务逻辑间对的双向耦合
* mvc是一种**架构模式**而不是设计模式

### Model1时代

* 视图和模型层
* 架构简单
* jsp职责不单一, 职责过重 不便于维护(兼顾Model2时代的View和Controller)

```
客户端---->request--->jsp取请求参数(jsp)---->调用业务逻辑方法--->-业务逻辑------->

数据返回后渲染页面(jsp)---->response--->客户端
```

### 问题

* 你的项目架构, 是设计好的, 还是演进的

```
演进的

用户量增大, 需求不断扩充
```

### Model2时代

* 视图, 控制 , 模型

```
客户端---->Servlet------->业务逻辑------>返回Servlet------>jsp------->客户端
```

* 用户发请求
* Servlet接收请求数据, 并调用对应的业务逻方法
* 业务处理完毕, 放回更新后的数据给Servlet
* Servlet转向到jsp, 由jsp渲染页面
* 响应给前端更新后的页面

#### 职责分析

* Controller(Servlet) 控制器

1.  取得表单数据
2. 调用业务逻辑
3. 转向指定的页面

* Model  			模型

1.  业务逻辑
2. 保存数据的状态

* View			视图

1.  显示页面

#### 作用

* 代码复用性和扩展性
* 好维护

# 回顾Servlet

## 环境

* 导入依赖

```xml
 <!--导入依赖-->
    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.2.5.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet.jsp</groupId>
            <artifactId>jsp-api</artifactId>
            <version>2.2</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>servlet-api</artifactId>
            <version>2.5</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>jstl</artifactId>
            <version>1.2</version>
        </dependency>
    </dependencies>
```

* 创建子项目
* web项目

* 右键添加框架支持
* 导入依赖

```xml
   <dependency>
            <groupId>javax.servlet.jsp</groupId>
            <artifactId>jsp-api</artifactId>
            <version>2.2</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>servlet-api</artifactId>
            <version>2.5</version>
            <scope>provided</scope>
        </dependency>
```

* 新建Servlet类

```java
package com.cyz.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class HelloServlet extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //1. 获取前端参数
        String method = req.getParameter("method");
        if (method.equals("add")){
            req.getSession().setAttribute("msg","执行了add方法");
        }
        if (method.equals("delete")){
            req.getSession().setAttribute("msg","执行了delete方法");
        }
        //2. 调用业务层

        //3. 视图层转发或者重定向
//        resp.sendRedirect(); //重定向
        req.getRequestDispatcher("/WEB-INF/jsp/test.jsp").forward(req,resp);//转发
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        super.doPost(req, resp);
    }
}

```

* 新建jsp页面

```jsp
<%--
  Created by IntelliJ IDEA.
  User: Administrator
  Date: 2020/5/21 0021
  Time: 8:47
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
${msg}
</body>
</html>

```

* 配置web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    
    <servlet>
        <servlet-name>hello</servlet-name>
        <servlet-class>com.cyz.servlet.HelloServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>hello</servlet-name>
        <url-pattern>/hello</url-pattern>
    </servlet-mapping>

<!--    session超时时间-->
<!--    <session-config>-->
<!--        <session-timeout>15</session-timeout>-->
<!--    </session-config>-->

<!--    首页-->
<!--    <welcome-file-list>-->
<!--        <welcome-file>index.jsp</welcome-file>-->
<!--    </welcome-file-list>-->
    
</web-app>
```

* 新建表单页面

```jsp
<%--
  Created by IntelliJ IDEA.
  User: Administrator
  Date: 2020/5/21 0021
  Time: 8:53
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    <form action="/hello" method="post">
        <input type="text" name="method">
        <input type="submit">
    </form>
</body>
</html>

```

* 配置tomcat启动
* 访问<http://localhost:8080/hello?method=delete>
* <http://localhost:8080/hello?method=add>

## MVC框架需要做哪些事情

1.  将url映射到java类或Java类的犯法
2. 封装用户提交的数据
3. 处理请求---调用相关的业务处理---封装响应数据
4. 将响应的数据进行渲染.jsp/html等表示层数据

## MVVM

```
M
V
VM  数据双向绑定	
```

## SpringMvc

* 基于Java实现MVC的轻量级框架
* 高效, 基于请求响应的MVC框架
* 与Spring兼容性好
* 约定优于配置
* 功能强大: Restful 数据验证   格式化  本地化  主题
* 简洁灵活

```
底层就是围绕DispatcherServlet
```

# 初次体验

* 环境搭建
* 新module
* 添加web支持
* 配置web.xml  注册DispatcherServlet

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

<!--    1. 注册DispatcherServlet-->
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
<!--        关联一个springmvc的配置文件: [servlet-name]-servlet,xml-->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc-servlet.xml</param-value>
        </init-param>
<!--        自动级别-1-->
        <load-on-startup>1</load-on-startup>
    </servlet>

<!--    / 匹配所有请求  (不包括.jsp)-->
<!--    /* 匹配所有请求  (包括.jsp)-->
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
</web-app>
```

* 配置springmvc-servlet.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
            http://www.springframework.org/schema/beans/spring-beans.xsd"
>
    <bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"/>
    <bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"/>

    <!--    视图解析器: DispatcherServlet给他的ModelAndView-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="InternalResourceViewResolver">
        <!--        前缀-->
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <!--        后缀-->
        <property name="suffix" value=".jsp"/>
    </bean>

<!--    Handler-->
    <bean id="/hello" class="com.cyz.controller.HelloController"/>

</beans>
```

* 写Controller

```java
package com.cyz.controller;

import org.springframework.web.servlet.ModelAndView;

import org.springframework.web.servlet.mvc.Controller;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

//导入Controller接口
public class HelloController implements Controller {

//    处理请求和响应
    public ModelAndView handleRequest(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse) throws Exception {
        //ModelAndView  模型和视图
        ModelAndView mv = new ModelAndView();

        //封装对象, 放在ModelAndView中, Model    相当于存session
        mv.addObject("msg","HelloSpringMVC!");

        //封装要跳转的视图, 放在ModelAndView中    相当于请求转发
        mv.setViewName("hello");//WEB-INF/jsp/hello.jsp    配置里配置了前缀和后缀

        return mv;
    }
}

```

* hello.jsp

```jsp
<%--
  Created by IntelliJ IDEA.
  User: Administrator
  Date: 2020/5/21 0021
  Time: 9:50
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
${msg}
</body>
</html>

```

* 配置tomcat启动
* 访问<http://localhost:8080/hello>
* 404解决

```
看jar包

导入lib包

重启tomcat
```

# SpringMvc运行原理

```
请求---->DispatcherServlet----->Controller---->ModelAndView  ------->
DispatcherServlet----->相应
```

```
1. DispatcherServlet: 前置控制器 Spring控制中心  用户发出请求, DispatcherServlet拦截请求

2. HandlerMapper: 处理器映射 DispatcherServlet调用
	根据url寻找处理器(/hello)
	
3. HandlerExecution: 表示具体的Handler, 其作用是根据url查找控制器(hello)

4. HandlerExecution 将解析后的数据返回给DispatcherServlet  解析控制器映射等

5. HandlerAdapter 表示处理器适配器, 其按照特定的规则去执行Handler

6. Handler执行特定的Controller

7. Controller 将具体的执行信息返回给HandlerAdapter 
				如 ModelAndView(处理业务,携带数据,跳转视图)
				
8. HandlerAdapter 将视图逻辑名或模型传递给DispatcherServlet

9. DispatcherServlet 调用视图解析器(ViewResolver)来解析 HandlerAdapter 传递的逻辑视图名

10. ViewResolver 视图解析器 将解析的逻辑视图名传给 DispatcherServlet

11. DispatcherServlet 根据视图解析器的视图结果 调用具体的视图

12. 最终渲染视图呈现给用户
```

# 注解开发

* 导入包
* 导入lib
* 配置web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

<!--    1. 注册DispatcherServlet-->
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
<!--        关联一个springmvc的配置文件: [servlet-name]-servlet,xml-->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc-servlet.xml</param-value>
        </init-param>
<!--        自动级别-1-->
        <load-on-startup>1</load-on-startup>
    </servlet>

<!--    / 匹配所有请求  (不包括.jsp)-->
<!--    /* 匹配所有请求  (包括.jsp)-->
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
</web-app>
```

* 配置springmvc-servlet.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
            http://www.springframework.org/schema/beans/spring-beans.xsd
            http://www.springframework.org/schema/context
            http://www.springframework.org/schema/context/spring-context.xsd
            http://www.springframework.org/schema/mvc
            http://www.springframework.org/schema/mvc/spring-mvc.xsd"
>

    <!--    自动扫描包-->
    <context:component-scan base-package="com.cyz.controller"/>

<!--    让Spring MVC 不处理静态资源 .css .js .html .mp3 .mp4   -->
    <mvc:default-servlet-handler/>

<!--
    支持mvc注解驱动
        在spring中一般采用@RequestMapping注解来完成映射关系
        要想@RequestMapping生效
        必须向上下文中注册DefaultAnnotationHanderMapping
        和一个AnnotationMethodHandlerAdapter实例
        这两个实例分别在类级别个方法级别处理
        而annotation-driven胚子帮助我们自动完成了上述两个实例的注入
-->
<mvc:annotation-driven/>

    <!--    视图解析器: DispatcherServlet给他的ModelAndView-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="InternalResourceViewResolver">
        <!--        前缀-->
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <!--        后缀-->
        <property name="suffix" value=".jsp"/>
    </bean>

</beans>
```

* hello.jsp

```jsp
<%--
  Created by IntelliJ IDEA.
  User: Administrator
  Date: 2020/5/21 0021
  Time: 11:08
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
${msg}
</body>
</html>

```

* controller

```java
package com.cyz.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.ModelAndView;

/**
 * com.cyz.controller
 *
 * @author cyz
 * @create 2020/05/21 11-08
 */
@Controller
@RequestMapping("/hello")
public class HelloController {

//    localhost:8080/hello/hi
    @RequestMapping("/hi")
    public String hello(Model model ){
        //封装数据
        model.addAttribute("msg","Hello, SpringMVCAnnotation");

        return "hello";//或被视图处理器接续
    }
}

```

* 配置tomcat启动
* 访问<http://localhost:8080/hello/hi>
* 注解实现了 处理器映射器和处理器适配器  我们只需要配置视图解析器即可

# Controller

## 环境

* web支持
* lib导入

* 配置web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc-servlet.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
</web-app>
```

* 配置springmvc-servlet.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
            http://www.springframework.org/schema/beans/spring-beans.xsd
            http://www.springframework.org/schema/context
            http://www.springframework.org/schema/context/spring-context.xsd
            http://www.springframework.org/schema/mvc
            http://www.springframework.org/schema/mvc/spring-mvc.xsd"
>
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <property name="suffix" value=".jsp"/>
    </bean>
</beans>
```
## 方式一  继承Controller接口
* 写Controller 

```java
package com.cyz.controller;

import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.mvc.Controller;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

//只要实现了Controller接口类 说明这就是一个控制器
public class ControllerDemo implements Controller {
    public ModelAndView handleRequest(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse) throws Exception {
        ModelAndView mv = new ModelAndView();

        mv.addObject("msg","Hello");

        mv.setViewName("test");

        return mv;
    }
}

```

* 写jsp

```jsp
<%--
  Created by IntelliJ IDEA.
  User: Administrator
  Date: 2020/5/21 0021
  Time: 13:23
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
${msg}
</body>
</html>

```

* 配置springmvc-servlet.xml

```xml
<!--    配置-->
    <bean name="/test" class="com.cyz.controller.ControllerDemo"/>
```

* 缺点

```
一个Controller只能配置一个类
```

## 方式二  使用注解

* web.xml不变

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc-servlet.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>


</web-app>
```

* 配置springmvc-servlet.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
            http://www.springframework.org/schema/beans/spring-beans.xsd
            http://www.springframework.org/schema/context
            http://www.springframework.org/schema/context/spring-context.xsd
            http://www.springframework.org/schema/mvc
            http://www.springframework.org/schema/mvc/spring-mvc.xsd"
>
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="InternalResourceViewResolver">
    <property name="prefix" value="/WEB-INF/jsp/"/>
    <property name="suffix" value=".jsp"/>
</bean>

</beans>
```

* 写Controller
* 注意return 的作用

```java
package com.cyz.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

//使用Spring接管
//被注释类中的所有方法, 如果返回值是String 并且有具体页面可以跳转, 那么就会被视图解析器解析
@Controller
public class ControllerTest2 {

    @RequestMapping("/test2")
    public String test1(Model model) {
        model.addAttribute("msg", "ControllerTest2");
        return "test";//return //WEB-INF/jsp/test.jsp
    }
}

```

* 配置springmvc-servlet.xml

```xml
<!--注册Controller-->
    <context:component-scan base-package="com.cyz.controller"/>
```

* 可配置多个在同一个视图

```java
package com.cyz.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

//使用Spring接管
//被注释类中的所有方法, 如果返回值是String 并且有具体页面可以跳转, 那么就会被视图解析器解析
@Controller
public class ControllerTest2 {

    @RequestMapping("/test2")
    public String test1(Model model) {
        model.addAttribute("msg", "ControllerTest2");
        return "test";//return //WEB-INF/jsp/test.jsp
    }

    @RequestMapping("/test3")
    public String test3(Model model) {
        model.addAttribute("msg", "ControllerTest2");
        return "test";
    }
}

```

# RequestMapper

* 映射url到控制器类或一个特定的处理程序的方法, 可用于类或方法上, 
  * 用在类上:  表示类所有的响应请求的方法都是以该地址作为父路径

```java
@Controller
@RequestMapping("/c3")
public class ControllerTest3 {

    @RequestMapping("/t3")
    public String test1(Model model) {
        model.addAttribute("msg", "ControllerTest3");
        return "test";
    }
}
```

# RestFul风格

* 一个资源定位及资源操作的**风格**

```
POST	  新增

DELETE	  删除

PUT 	  更新

GET       查询
```

* 原先

```java
@Controller
public class RestFulController {

//原来   http://localhost:8080/add?a=1&b=2
    @RequestMapping("/add")
    public String test1(int a, int b, Model model){
        int res = a + b;
        model.addAttribute("msg",res);
        return "test";
    }
}
```

* restful
* 使用@PathVariable   路径变量

```java
@Controller
public class RestFulController {
    //restful    http://localhost:8080/add/2/4
    @RequestMapping("/add/{a}/{b}")
    public String test1(@PathVariable int a, @PathVariable int b, Model model) {
        int res = a + b;
        model.addAttribute("msg", res);
        return "test";
    }
}
```

* 切换方法

```java
@Controller
public class RestFulController {
    //restful    http://localhost:8080/add/2/4
    @RequestMapping(value = "/add/{a}/{b}",method = RequestMethod.GET)
    public String test1(@PathVariable int a, @PathVariable String b, Model model) {
        String res = a + b;
        model.addAttribute("msg", res);
        return "test";
    }
}
```

* 等价于

```java
@Controller
public class RestFulController {
    //restful    http://localhost:8080/add/2/4
    @GetMapping("/add/{a}/{b}")
    public String test1(@PathVariable int a, @PathVariable String b, Model model) {
        String res = a + b;
        model.addAttribute("msg", res);
        return "test";
    }
}
```

```
 @GetMapping
 
 DeleteMapping
 
 PutMapping
 
 PostMapping
```

# 重定向和转发

## ModelAndView

* 根据view的名称, 和视图解析器跳转到指定页面
* 页面:  视图解析器前缀 +  viewName + 视图解析器后缀

```xml
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="InternalResourceViewResolver">
    <property name="prefix" value="/WEB-INF/jsp/"/>
    <property name="suffix" value=".jsp"/>
</bean>
```

```java
//只要实现了Controller接口类 说明这就是一个控制器
public class ControllerDemo implements Controller {
    
    public ModelAndView handleRequest(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse) throws Exception {
        ModelAndView mv = new ModelAndView();

        mv.addObject("msg","Hello");

        mv.setViewName("test");

        return mv;
    }
}
```

## ServletAPI

* 不需要视图解析器
* 通过HttpServletResponse进行输出
* 通过HttpServletResponse进行重定向
* 通过HttpServletResponse进行转发

```java
@Controller
public class ModelTest1 {

    @RequestMapping("/m1/t1")
    public String test(HttpServletRequest request, HttpServletResponse response){
        HttpSession session = request.getSession();
        System.out.println(session.getId());
        return "test";
    }

    @RequestMapping("/m1/t2")
    public void test2(HttpServletRequest request, HttpServletResponse response) throws IOException {
      response.getWriter().println("Hello  t2");
    }

    @RequestMapping("/m1/t3")
    public void test3(HttpServletRequest request, HttpServletResponse response) throws IOException {
        response.sendRedirect("/index.jsp");
    }

    @RequestMapping("/m1/t4")
    public void test4(HttpServletRequest request, HttpServletResponse response) throws IOException, ServletException {
        request.setAttribute("msg","hello t4");
        request.getRequestDispatcher("/WEB-INF/jsp/test.jsp").forward(request,response);
    }

}
```

## SpringMvc

* 不使用视图解析器

```java
    @RequestMapping("/m1/t1")
    public String test(Model model){
//        转发
        model.addAttribute("msg","不使用视图解析器");
        return "/WEB-INF/jsp/test.jsp";
    }
```

```java
@Controller
public class ModelTest1 {

    @RequestMapping("/m1/t1")
    public String test(Model model){
//        转发
        model.addAttribute("msg","不使用视图解析器");
        return "forward:/WEB-INF/jsp/test.jsp";
    }
}
```

```
@Controller
public class ModelTest1 {

    @RequestMapping("/m1/t1")
    public String test(){
//        重定向
        return "redirect:/index.jsp";
    }
}
```

* 使用视图解析器
  * 会将地址拼接

# 接收请求参数及数据回显

* 参数与url一致
* 参数与url不一致      @RequestParam
* 参数为对象

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class User {
    private int id;
    private String name;
    private int age;
}
```

```java
http://localhost:8080/user/t1?name=sss


@Controller
@RequestMapping("/user")
public class UserController {

    @GetMapping("/t1")
    public String test1(String name, Model model) {
        //接收前端参数
        System.out.println("接收到的参数为: " + name);
        //将返回的结果床底给前端
        model.addAttribute("msg", name);
//                视图跳转
        return "test";
    }
}
```

* 参数不一致

```java
http://localhost:8080/user/t1?username=sss



@Controller
@RequestMapping("/user")
public class UserController {

    @GetMapping("/t1")
    public String test1(@RequestParam("username") String name, Model model) {
        //接收前端参数
        System.out.println("接收到的参数为: " + name);
        //将返回的结果床底给前端
        model.addAttribute("msg", name);
//                视图跳转
        return "test";
    }
}
```

* 参数为对象

```java
//    接收一个对象
    /*
        1. 接收前端用户传递的参数 判断参数的名字  假如名字直接在方法上, 可以直接使用
        2. 假设传递的是一个对象User 匹配User对象中的字段名: 如果名字一致则ok 否则匹配不到 返回null
     */
//    http://localhost:8080/user/t2?id=1&name=jack&age=18
    @GetMapping("/t2")
    public String test2(User user){
        System.out.println(user);//User(id=1, name=jack, age=18)
        return "test";
    }
```

## ModelMap

```java
//    Model    精简版
//    ModelMap 继承至 LinkedHashMap
    @GetMapping("/t3")
    public String test3(ModelMap map){


        return "test";
    }
```

# 乱码问题

* 配置web.xml
* 注意是`/*`不是`/`

```xml
<!--    配置SpringMvc的乱码过滤-->
    <filter>
        <filter-name>encoding</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>utf-8</param-value>
        </init-param>
    </filter>
   <filter-mapping>
       <filter-name>encoding</filter-name>
       <url-pattern>/*</url-pattern>
   </filter-mapping>
```

* 配置tomcat 的 service.xml

```xml
    <Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" 
	URIEncoding="UTF-8"/>
```

* 终极方案 自定义filter

```java
package com.cyz.filter;

import javax.servlet.*;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletRequestWrapper;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.UnsupportedEncodingException;
import java.util.Map;

/**
 * com.cyz.controller
 *
 * @author cyz
 * @create 2020/05/21 15-39
 */
public class GenericEncodingFilter implements Filter {


    public void init(FilterConfig filterConfig) throws ServletException {
    }

    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        //处理response的字符编码
        HttpServletResponse myResponse=(HttpServletResponse) servletResponse;
        myResponse.setContentType("text/html;charset=UTF-8");

        // 转型为与协议相关对象
        HttpServletRequest httpServletRequest = (HttpServletRequest) servletRequest;
        // 对request包装增强
        HttpServletRequest myrequest = new MyRequest(httpServletRequest);
        filterChain.doFilter(myrequest, servletResponse);
    }

    public void destroy() {

    }
}

//自定义request对象，HttpServletRequest的包装类
class MyRequest extends HttpServletRequestWrapper {

    private HttpServletRequest request;
    //是否编码的标记
    private boolean hasEncode;
    //定义一个可以传入HttpServletRequest对象的构造函数，以便对其进行装饰
    public MyRequest(HttpServletRequest request) {
        super(request);// super必须写
        this.request = request;
    }

    // 对需要增强方法 进行覆盖
    @Override
    public Map getParameterMap() {
        // 先获得请求方式
        String method = request.getMethod();
        if (method.equalsIgnoreCase("post")) {
            // post请求
            try {
                // 处理post乱码
                request.setCharacterEncoding("utf-8");
                return request.getParameterMap();
            } catch (UnsupportedEncodingException e) {
                e.printStackTrace();
            }
        } else if (method.equalsIgnoreCase("get")) {
            // get请求
            Map<String, String[]> parameterMap = request.getParameterMap();
            if (!hasEncode) { // 确保get手动编码逻辑只运行一次
                for (String parameterName : parameterMap.keySet()) {
                    String[] values = parameterMap.get(parameterName);
                    if (values != null) {
                        for (int i = 0; i < values.length; i++) {
                            try {
                                // 处理get乱码
                                values[i] = new String(values[i]
                                        .getBytes("ISO-8859-1"), "utf-8");
                            } catch (UnsupportedEncodingException e) {
                                e.printStackTrace();
                            }
                        }
                    }
                }
                hasEncode = true;
            }
            return parameterMap;
        }
        return super.getParameterMap();
    }

    //取一个值
    @Override
    public String getParameter(String name) {
        Map<String, String[]> parameterMap = getParameterMap();
        String[] values = parameterMap.get(name);
        if (values == null) {
            return null;
        }
        return values[0]; // 取回参数的第一个值
    }

    //取所有值
    @Override
    public String[] getParameterValues(String name) {
        Map<String, String[]> parameterMap = getParameterMap();
        String[] values = parameterMap.get(name);
        return values;
    }

}

```

```xml
    <filter>
        <filter-name>encoding</filter-name>
        <filter-class>com.cyz.filter.GenericEncodingFilter</filter-class>
    </filter>
   <filter-mapping>
       <filter-name>encoding</filter-name>
       <url-pattern>/*</url-pattern>
   </filter-mapping>
```

# JSON

* 数据格式

## JSON和JavaScript对象的转换

```javascript
 	<script type="text/javascript">
        var user = {
            name:"jack",
            age:3,
            sex:"男"
        }

        //js 转json
        var json = JSON.stringify(user)

        console.log(json);

        //json转js
        var obj = JSON.parse(json);
        console.log(obj)

        console.log(user)

    </script>
```

# Jackson的使用

```xml
<!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-databind -->
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.10.0</version>
</dependency>


   <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.12</version>
            <scope>provided</scope>
        </dependency>

```

* web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc-servlet.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>

    <!--    配置SpringMvc的乱码过滤-->
    <filter>
        <filter-name>encoding</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>utf-8</param-value>
        </init-param>
    </filter>
   <filter-mapping>
       <filter-name>encoding</filter-name>
       <url-pattern>/*</url-pattern>
   </filter-mapping>

</web-app>
```

* springmvc-servlet.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
            http://www.springframework.org/schema/beans/spring-beans.xsd
            http://www.springframework.org/schema/context
            http://www.springframework.org/schema/context/spring-context.xsd
            http://www.springframework.org/schema/mvc
            http://www.springframework.org/schema/mvc/spring-mvc.xsd"
>
    <context:component-scan base-package="com.cyz.controller"/>

    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <property name="suffix" value=".jsp"/>
    </bean>

</beans>
```

* JSON乱码配置springmvc-servlet.xml

```xml
<!--    JSON乱码问题配置-->
    <mvc:annotation-driven>
        <mvc:message-converters>
            <bean class="org.springframework.http.converter.StringHttpMessageConverter">
                <constructor-arg value="UTF-8"/>
            </bean>
            <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">
                <property name="objectMapper">
                    <bean class="org.springframework.http.converter.json.Jackson2ObjectMapperFactoryBean">
                        <property name="failOnEmptyBeans" value="false"/>
                    </bean>
                </property>
            </bean>
        </mvc:message-converters>
    </mvc:annotation-driven>
```

* pojo

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class User {
    private String name;
    private int age;
    private String sex;
}
```

* controller

```java
//@Controller
@RestController //结合ResponseBody
public class UserController {

    //原生解决json乱码     现在: spring直接配置
    //@RequestMapping(value = "/j1",produces = "application/json;charset=utf-8")

    //@ResponseBody// 不走视图解析器  直接返回字符串
    @RequestMapping("/j1")
    public String json1() throws JsonProcessingException {

        //jackson
        ObjectMapper mapper = new ObjectMapper();

        //创建一个对象
        User user = new User("jack",19,"男");

        String str =  mapper.writeValueAsString(user);

        return str;
    }
}
```

* 返回集合

```java
 @RequestMapping("/j2")
    public String json2() throws JsonProcessingException {

        //jackson
        ObjectMapper mapper = new ObjectMapper();

        List<User> userList = new ArrayList<User>();

        //创建一个对象
        User user1 = new User("jack1",19,"男");
        User user2 = new User("jack2",19,"男");
        User user3 = new User("jack3",19,"男");
        User user4 = new User("jack4",19,"男");

        userList.add(user1);
        userList.add(user2);
        userList.add(user3);
        userList.add(user4);

        String str =  mapper.writeValueAsString(userList);
        return str;
    }
```

* java实现格式化时间

```java
    @RequestMapping("/j3")
    public String json3() throws JsonProcessingException {

        //jackson
        ObjectMapper mapper = new ObjectMapper();

        //返回时间戳
        Date date = new Date();

        //格式化
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");


        String str =  mapper.writeValueAsString(sdf.format(date));
        return str;
    }
```

* jackson实现格式化时间

```java
   @RequestMapping("/j3")
    public String json3() throws JsonProcessingException {

        //jackson
        ObjectMapper mapper = new ObjectMapper();

        //使用ObjectMapper 来格式化
        //关闭以时间戳的方式
        mapper.configure(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS,false);

        //自定义格式化
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");

        mapper.setDateFormat(sdf);

        //返回时间戳
        Date date = new Date();

        String str =  mapper.writeValueAsString(date);
        return str;
    }
```

* 工具类JsonUtils

```java
public class JsonUtils {

    public static String getJson(Object object){
        return getJson(object,"yyyy-MM-dd HH:mm:ss");
    }

    public static String getJson(Object object, String dateFormat){
        ObjectMapper mapper = new ObjectMapper();

        //不使用时间戳的方式
        mapper.configure(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS,false);

        //自定义日期格式
        SimpleDateFormat sdf = new SimpleDateFormat(dateFormat);
        mapper.setDateFormat(sdf);

        try {
            return mapper.writeValueAsString(object);
        }catch (JsonProcessingException e){
            e.printStackTrace();
        }
        return null;
    }
}
```

* 使用

```java
package com.cyz.controller;

import com.cyz.pojo.User;
import com.cyz.utils.JsonUtils;
import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.databind.SerializationFeature;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;
import org.springframework.web.bind.annotation.RestController;

import java.text.SimpleDateFormat;
import java.util.ArrayList;
import java.util.Date;
import java.util.List;

//@Controller
@RestController //结合ResponseBody
public class UserController {

    //原生解决json乱码     现在: spring直接配置
    //@RequestMapping(value = "/j1",produces = "application/json;charset=utf-8")

    //@ResponseBody// 不走视图解析器  直接返回字符串
    @RequestMapping("/j1")
    public String json1() throws JsonProcessingException {
        //创建一个对象
        User user = new User("jack",19,"男");
        return JsonUtils.getJson(user);
    }


    @RequestMapping("/j2")
    public String json2() throws JsonProcessingException {

        List<User> userList = new ArrayList<User>();

        //创建一个对象
        User user1 = new User("jack1",19,"男");
        User user2 = new User("jack2",19,"男");
        User user3 = new User("jack3",19,"男");
        User user4 = new User("jack4",19,"男");

        userList.add(user1);
        userList.add(user2);
        userList.add(user3);
        userList.add(user4);

        return JsonUtils.getJson(userList);
    }

    @RequestMapping("/j3")
    public String json3() throws JsonProcessingException {
        Date date = new Date();

        return JsonUtils.getJson(date,"yyyy-MM-dd HH:mm:ss");
    }
}

```

# FastJson

* 导包

```java
<!-- https://mvnrepository.com/artifact/com.alibaba/fastjson -->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
    <version>1.2.68</version>
</dependency>

```

* 导入lib

* 使用

```java
 @RequestMapping("/j4")
    public String json4() throws JsonProcessingException {
        List<User> userList = new ArrayList<User>();

        //创建一个对象
        User user1 = new User("jack1",19,"男");
        User user2 = new User("jack2",19,"男");
        User user3 = new User("jack3",19,"男");
        User user4 = new User("jack4",19,"男");

        userList.add(user1);
        userList.add(user2);
        userList.add(user3);
        userList.add(user4);

        return JSON.toJSONString(userList);
    }
```

* 方法

```
@RequestMapping("/j4")
    public String json4() throws JsonProcessingException {
        List<User> userList = new ArrayList<User>();

        //创建一个对象
        User user1 = new User("jack1",19,"男");
        User user2 = new User("jack2",19,"男");
        User user3 = new User("jack3",19,"男");
        User user4 = new User("jack4",19,"男");

        userList.add(user1);
        userList.add(user2);
        userList.add(user3);
        userList.add(user4);

//        Java对象转JSON字符串
        String str1 = JSON.toJSONString(userList);
        String str2 = JSON.toJSONString(user1);


//        JSON字符串转java对象
        User jp_user1 = JSON.parseObject(str2,User.class);

//        java对象转 JSON 对象
        JSONObject jsonObject1 = (JSONObject)JSON.toJSON(user2);

//        JSON对象  转 java对象
        User to_java_user = JSON.toJavaObject(jsonObject1,User.class);

        return "hello";
    }
```

# SSM整合

## 数据库

```sql
CREATE DATABASE ssmbuild;

USE ssmbuild;

CREATE TABLE `books`(
`bookID` INT NOT NULL AUTO_INCREMENT COMMENT '书id',
`bookName` VARCHAR(100) NOT NULL COMMENT '书名',
`bookCounts` INT NOT NULL COMMENT '数量',
`detail` VARCHAR(200) NOT NULL COMMENT '描述',
KEY `bookID`(`bookID`)
)ENGINE=INNODB DEFAULT CHARSET=utf8

USE ssmbuild;

INSERT INTO `books`(`bookID`,`bookName`,`bookCounts`,`detail`)VALUES
(1,'Java',1,'从入门到放弃'),
(2,'MySQL',10,'从删库到跑路'),
(3,'Linux',5,'从进门到进牢');

```

## 依赖

```xml
<!--    依赖-->
    <dependencies>
        <!--    mysql驱动-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.47</version>
        </dependency>

        <!--    junit-->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
        </dependency>

<!--        数据库连接池c3p0-->
        <dependency>
            <groupId>com.mchange</groupId>
            <artifactId>c3p0</artifactId>
            <version>0.9.5.2</version>
        </dependency>

<!--        Servlet - JSP-->
        <dependency>
            <groupId>javax.servlet.jsp</groupId>
            <artifactId>jsp-api</artifactId>
            <version>2.2</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>servlet-api</artifactId>
            <version>2.5</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>jstl</artifactId>
            <version>1.2</version>
        </dependency>

<!--        Spring -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.2.5.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>5.2.5.RELEASE</version>
        </dependency>
        
        <!--mybatis整合-->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.4.6</version>
        </dependency>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis-spring</artifactId>
            <version>2.0.4</version>
        </dependency>

<!--        Lombok-->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.12</version>
            <scope>provided</scope>
        </dependency>
    </dependencies>
```

## 静态资源导出问题

```xml
 <build>
        <resources>
            <resource>
                <directory>src/main/resources</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>false</filtering>
            </resource>
            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>false</filtering>
            </resource>
        </resources>
    </build>
```

## 数据库连接

* ssmbuild

## 搭建基础项目包结构

* controller
* dao
* pojo
* service

## 配置文件

* mybatis-config.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<!--MyBatis核心配置文件-->
<configuration>

</configuration
```

* applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
            http://www.springframework.org/schema/beans/spring-beans.xsd"
>
    
</beans>
```

## 数据库文件

* database.properties

```properties
jdbc.driver=com.mysql.jdbc.Driver
# 如果使用的是MySQL8.0+, 增加一个时区的配置 &serverTimezone=Asia/Shanghai
jdbc.url=jdbc:mysql://localhost:3306/ssmbuild?useSSL=false&useUnicode=true&characterEncoding=UTF-8
jdbc.username=root
jdbc.password=123456
```

## 编写Mybatis配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<!--MyBatis核心配置文件-->
<configuration>
<!--    配置数据源, 交给Spring去做-->

<!--    起别名-->
    <typeAliases>
        <package name="com.cyz.pojo"/>
    </typeAliases>

</configuration>
```

## 新建实体类

```java
package com.cyz.pojo;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@AllArgsConstructor
@NoArgsConstructor
public class Books {
    private int bookID;
    private String bookName;
    private int bookCounts;
    private String detail;
}

```

## 写接口测试

```java
package com.cyz.dao;

import com.cyz.pojo.Books;
import org.apache.ibatis.annotations.Param;

import java.util.List;

public interface BookMapper {
    //新增一本书
    int addBook(Books books);

    //删除一本书
    int deleteBookById(@Param("bookID") int id);

    //更新一本书
    int updateBook(Books books);

    //查询一本书
    Books queryBookById(@Param("bookID") int id);

    //查询全部的书
    List<Books> queryAllBook();
}

```

## 写Mapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.cyz.dao.BookMapper">
    <insert id="addBook" parameterType="books">
        insert into ssmbuild.books (bookName, bookCounts, detail)
        values (#{bookName},#{bookCounts},#{detail})
    </insert>
    <update id="updateBook" parameterType="books">
        update ssmbuild.books
        set bookName=#{bookName},bookCounts=#{bookCounts},detail=#{detail}
        where bookID=#{bookID}
    </update>
    <delete id="deleteBookById" parameterType="int">
        delete from ssmbuild.books where bookID = #{bookID}
    </delete>
    <select id="queryBookById" resultType="books">
        select * from ssmbuild.books
        where bookID=#{bookID}
    </select>
    <select id="queryAllBook" resultType="books">
        select * from ssmbuild.books
    </select>
</mapper>
```

## 将Mapper.xml绑定到Mybatis配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<!--MyBatis核心配置文件-->
<configuration>
    <!--    配置数据源, 交给Spring去做-->

    <!--    起别名-->
    <typeAliases>
        <package name="com.cyz.pojo"/>
    </typeAliases>

<!--    绑定mapper-->
    <mappers>
        <mapper class="com.cyz.dao.BookMapper"/>
    </mappers>

</configuration>
```

## 处理业务层

### 新建Service接口

```java
package com.cyz.service;

import com.cyz.pojo.Books;
import java.util.List;

public interface BookService {
    //新增一本书
    int addBook(Books books);

    //删除一本书
    int deleteBookById(int id);

    //更新一本书
    int updateBook(Books books);

    //查询一本书
    Books queryBookById(int id);

    //查询全部的书
    List<Books> queryAllBook();
}

```

### 新建Service实现类

```
package com.cyz.service;

import com.cyz.dao.BookMapper;
import com.cyz.pojo.Books;

import java.util.List;

public class BookServiceImpl implements BookService {

    //service调dao层:  组合Dao
    private BookMapper bookMapper;
    public void setBookMapper(BookMapper bookMapper) {
        this.bookMapper = bookMapper;
    }

    public int addBook(Books books) {
        return bookMapper.addBook(books);
    }

    public int deleteBookById(int id) {
        return bookMapper.deleteBookById(id);
    }

    public int updateBook(Books books) {
        return bookMapper.updateBook(books);
    }

    public Books queryBookById(int id) {
        return bookMapper.queryBookById(id);
    }

    public List<Books> queryAllBook() {
        return bookMapper.queryAllBook();
    }
}

```

# MyBatis层完成

## 配置文件spring-dao.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
            http://www.springframework.org/schema/beans/spring-beans.xsd
            http://www.springframework.org/schema/context
            http://www.springframework.org/schema/context/spring-context.xsd"
>
<!--    1.关联数据库配置文件-->
    <context:property-placeholder location="classpath:database.properties"/>

<!--    2. 连接池
    dbcp:       半自动化操作,  不能自动连接
    c3p0:       自动化操作(自动化的加载配置文件, 并且可以自动设置到对象中)
    druid:
    hikari:
-->
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="driverClass" value="${jdbc.driver}"/>
        <property name="jdbcUrl" value="${jdbc.url}"/>
        <property name="user" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>

<!--        c3p0的私有属性-->
        <property name="maxPoolSize" value="30"/>
        <property name="minPoolSize" value="10"/>
<!--        关闭连接后不知道commit-->
        <property name="autoCommitOnClose" value="false"/>
<!--        获取连接超时时间-->
        <property name="checkoutTimeout" value="10000"/>
<!--        当获取连接失败重试次数-->
        <property name="acquireRetryAttempts" value="2"/>
    </bean>

<!--    3. sqlSessionFactory-->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"/>
<!--        绑定Mybatis的配置文件-->
        <property name="configLocation" value="classpath:mybatis-config.xml"/>
    </bean>

<!--    4. 配置dao接口扫描包, 动态的实现了Dao接口可以注入到Spring容器中-->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
<!--        注入sqlSessionFactory-->
        <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>
<!--        扫描的dao包-->
        <property name="basePackage" value="com.cyz.dao"/>
    </bean>
</beans>
```

## 配置文件spring-service.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
            http://www.springframework.org/schema/beans/spring-beans.xsd
            http://www.springframework.org/schema/context
            http://www.springframework.org/schema/context/spring-context.xsd"
>
    <!--   1. 扫描service下的包-->
    <context:component-scan base-package="com.cyz.service"/>


<!--    注意这里的配置文件需要在一个包下   通过import引入 或 idea自动处理在modules下-->
<!--    2. 将我们的所有业务类, 注入到Spring 可以通过配置, 或者注解实现 @Service和@Autowired-->
    <bean id="BookServiceImpl" class="com.cyz.service.BookServiceImpl">
        <property name="bookMapper" ref="bookMapper"/>
</bean>

<!--    3. 声明式事务配置-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
<!--        注入数据源-->
        <property name="dataSource" ref="dataSource"/>
    </bean>

<!--    4. aop事务支持-->

</beans>
```

# Spring层完成

## 添加web支持

## 配置web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

<!--    DispatcherServlet-->
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:applicationContext.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>

<!--    乱码过滤-->
    <filter>
        <filter-name>encodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>utf-8</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>encodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>

<!--    Session -->
    <session-config>
        <session-timeout>15</session-timeout>
    </session-config>

</web-app>
```

## 新建spring-mvc.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
            http://www.springframework.org/schema/beans/spring-beans.xsd
            http://www.springframework.org/schema/context
            http://www.springframework.org/schema/context/spring-context.xsd
            http://www.springframework.org/schema/mvc
            http://www.springframework.org/schema/mvc/spring-mvc.xsd"
>
<!--    1. 注解驱动-->
    <mvc:annotation-driven/>
<!--    2. 静态资源过滤-->
    <mvc:default-servlet-handler/>
<!--    3. 扫描包 controller-->
    <context:component-scan base-package="com.cyz.controller"/>
<!--    4. 视图解析器-->
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    <property name="prefix" value="/WEB-INF/jsp/"/>
    <property name="suffix" value=".jsp"/>
</bean>

</beans>
```

## idea没有自动配置需要关联包

* applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
            http://www.springframework.org/schema/beans/spring-beans.xsd"
>
<!--    关联-->
    <import resource="classpath:spring-dao.xml"/>
    <import resource="classpath:spring-service.xml"/>
    <import resource="classpath:spring-mvc.xml"/>
</beans>
```

# SpringMvc层完成

# 搭建项目业务

## 创建Controller

```java
package com.cyz.controller;

import com.cyz.pojo.Books;
import com.cyz.service.BookService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

import java.util.List;

@Controller
@RequestMapping("/book")
public class BookController {
    //controller 调 service 层
    @Autowired
    @Qualifier("BookServiceImpl")
    private BookService bookService;

    //查询全部的书籍, 并且返回到一个数据展示页面
    @RequestMapping("/allBook")
    public String list(Model model){
        List<Books> list = bookService.queryAllBook();

        model.addAttribute("list",list);
        return "allBook";
    }

}

```

## 新建allBook.jsp

```jsp
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%--
  Created by IntelliJ IDEA.
  User: Administrator
  Date: 2020/5/10 0021
  Time: 21:06
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>书籍展示</title>
    <link href="https://cdn.bootcdn.net/ajax/libs/twitter-bootstrap/4.5.0/css/bootstrap.css" rel="stylesheet">
</head>
<body>
<div class="container">
    <div class="row clearfix">
        <div class="col-md-12 column">
            <div class="page-header">
                <h1>
                    <small>书籍列表——————————显示所有书籍</small>
                </h1>
            </div>
        </div>
    </div>
    <div class="row">
        <div class="col-md-4 column">
            <a class="btn btn-primary" href="${pageContext.request.contextPath}/book/toAddBook">新增书籍</a>
             <a class="btn btn-primary" href="${pageContext.request.contextPath}/book/allBook">显示全部书籍</a>
        </div>
        <div class="col-md-4 column">
            <span style="color: red;font-weight: bold;">${error}</span>
        </div>
        <div class="col-md-4 column">
            <form action="${pageContext.request.contextPath}/book/queryBook" method="post">
                <div style="display: flex">
                    <input style="flex: 1" type="text" name="queryBookName" class="form-control" placeholder="请输入需要查询的书籍名称">
                    <input style="margin-left: 15px;" class="btn btn-primary" type="submit" value="查询">
                </div>
            </form>
        </div>
    </div>

    <div class="row clearfix">
        <div class="col-md-12 column">
            <table class="table table-hover table-striped">
                <thead>
                <tr>
                    <th>书籍编号</th>
                    <th>书籍名称</th>
                    <th>书籍数量</th>
                    <th>书籍详情</th>
                    <th>操作</th>
                </tr>
                </thead>
                <tbody>
                <c:forEach var="book" items="${list}">
                    <tr>
                        <td>${book.bookID}</td>
                        <td>${book.bookName}</td>
                        <td>${book.bookCounts}</td>
                        <td>${book.detail}</td>
                        <td>
                            <a href="${pageContext.request.contextPath}/book/toUpdateBook?id=${book.bookID}">修改</a>
                            &nbsp; | &nbsp;
                             <a href="${pageContext.request.contextPath}/book/deleteBook/${book.bookID}">删除</a>
                        </td>
                    </tr>
                </c:forEach>
                </tbody>
            </table>
        </div>
    </div>
</div>
</body>
</html>

```

## 编写index.jsp

```jsp
<%--
  Created by IntelliJ IDEA.
  User: Administrator
  Date: 2020/5/10 0021
  Time: 20:47
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>首页</title>
</head>
<style>
    a {
        text-decoration: none;
        color: black;
        font-size: 18px;
    }

    h3 {
        width: 120px;
        height: 38px;
        margin: 100px auto;
        text-align: center;
        line-height: 38px;
        background-color: deepskyblue;
        border-radius: 5px;
    }
</style>
<body>
<h3>
    <a href="${pageContext.request.contextPath}/book/allBook">进入书籍页面</a>
</h3>
</body>
</html>

```

## 配置tomcat并启动

* 排错

```java
查看bean是否注入

单元测试

 @Test
    public void test(){
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        BookService bookServiceImpl = (BookService)context.getBean("BookServiceImpl");
        for (Books books : bookServiceImpl.queryAllBook()){
            System.out.println(books);
        }

    }

spring问题

springmvc

lib配置
```

* 访问<http://localhost:8080/>

## 新增书籍

* addBook.jsp

```jsp
<%--
  Created by IntelliJ IDEA.
  User: Administrator
  Date: 2020/5/11 0022
  Time: 9:08
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
    <link href="https://cdn.bootcdn.net/ajax/libs/twitter-bootstrap/4.5.0/css/bootstrap.css" rel="stylesheet">
</head>
<body>
<div class="container">
    <div class="row clearfix">
        <div class="col-md-12 column">
            <div class="page-header">
                <h1>
                    <small>新增书籍</small>
                </h1>
            </div>
        </div>
    </div>
    <form action="${pageContext.request.contextPath}/book/addBook" method="post">
        <div class="form-group">
            <label>书籍名称: </label>
            <input type="text" name="bookName" class="form-control" required>
        </div>
        <div class="form-group">
            <label>书籍数量: </label>
            <input type="number" name="bookCounts" class="form-control" required>
        </div>
        <div class="form-group">
            <label>书籍描述: </label>
            <input type="text" name="detail" class="form-control" required>
        </div>
        <div class="form-group">
            <input type="submit" class="form-control" value="添加">
        </div>
    </form>

</div>
</body>
</html>

```

* controller

```java
package com.cyz.controller;

import com.cyz.pojo.Books;
import com.cyz.service.BookService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

import java.util.List;

@Controller
@RequestMapping("/book")
public class BookController {
    //controller 调 service 层
    @Autowired
    @Qualifier("BookServiceImpl")
    private BookService bookService;

    //查询全部的书籍, 并且返回到一个数据展示页面
    @RequestMapping("/allBook")
    public String list(Model model){
        List<Books> list = bookService.queryAllBook();

        model.addAttribute("list",list);
        return "allBook";
    }

//    跳转到增加书籍页面
    @RequestMapping("/toAddBook")
    public String toAddPaper(){

        return "addBook";
    }

    //添加书籍的请求
    @RequestMapping("/addBook")
    public String addBook(Books books){
        System.out.println(books);
        bookService.addBook(books);
        return "redirect:/book/allBook";
    }
}

```

## 修改书籍

* updateBook

```jsp
<%--
  Created by IntelliJ IDEA.
  User: Administrator
  Date: 2020/5/11 0022
  Time: 9:31
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>修改书籍</title>
    <link href="https://cdn.bootcdn.net/ajax/libs/twitter-bootstrap/4.5.0/css/bootstrap.css" rel="stylesheet">
</head>
<body>
<div class="container">
    <div class="row clearfix">
        <div class="col-md-12 column">
            <div class="page-header">
                <h1>
                    <small>修改书籍</small>
                </h1>
            </div>
        </div>
    </div>
    <form action="${pageContext.request.contextPath}/book/updateBook" method="post">
<%--      前端隐藏域--%>
        <input type="hidden" name="bookID" value="${QBook.bookID}">
        <div class="form-group">
            <label>书籍名称: </label>
            <input type="text" name="bookName" class="form-control" required value="${QBook.bookName}">
        </div>
        <div class="form-group">
            <label>书籍数量: </label>
            <input type="text" name="bookCounts" class="form-control" required value="${QBook.bookCounts}">
        </div>
        <div class="form-group">
            <label>书籍描述: </label>
            <input type="text" name="detail" class="form-control" required value="${QBook.detail}">
        </div>
        <div class="form-group">
            <input type="submit" class="form-control" value="修改">
        </div>
    </form>

</div>
</body>
</html>

```

* controller

```java
//    跳转到修改书籍页面
    @RequestMapping("/toUpdateBook")
    public String toUpdatePaper(int id, Model model){
        Books books = bookService.queryBookById(id);
        System.out.println("toUpdatePaper==="+books);
        model.addAttribute("QBook",books);
        return "updateBook";
    }

    //修改书籍的请求
    @RequestMapping("/updateBook")
    public String updateBook(Books books){
        System.out.println("updateBook==="+books);
        bookService.updateBook(books);
        return "redirect:/book/allBook";
    }
```



### 提交事务

* 导入包

```xml
<dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.9.5</version>
        </dependency>
```

* 配置spring-service.xml

```xml
 <!--    4. aop事务支持-->
    <!--    结合AOP实现事务的织入-->
    <!--    配置事务通知-->
    <tx:advice id="txAdvice" transaction-manager="transactionManager">
        <!--        给哪些方法配置事务-->
        <!--        配置事务的传播特性 new propagation-->
        <tx:attributes>
            <tx:method name="*" propagation="REQUIRED"/>
        </tx:attributes>
    </tx:advice>

    <!--    配置事务切入-->
    <aop:config>
        <aop:pointcut id="txPointCut" expression="execution(* com.cyz.dao.*.*(..))"/>
        <aop:advisor advice-ref="txAdvice" pointcut-ref="txPointCut" />
    </aop:config>
```

* 导入lib

## 添加日志

* mybatis-config.xml

```xml
 <settings>
        <setting name="logImpl" value="STDOUT_LOGGING"/>
    </settings>
```

## 删除书籍

* controller

```java
//删除书籍 restful
    @RequestMapping("/deleteBook/{bookID}")
    public String updateBook(@PathVariable("bookID") int id){
        bookService.deleteBookById(id);
        return "redirect:/book/allBook";
    }
```

## 搜索功能

### dao层

* 接口

```java
//通过书名查询书籍
    List<Books> queryBookByName(@Param("bookName") String bookName);

```

* Mapper.xml

```xml
 <select id="queryBookByName" resultType="books">
        select * from ssmbuild.books
        where bookName = #{bookName}
    </select>
```

### service层

* 接口

```java
  //通过书名查询书籍
    List<Books> queryBookByName(String bookName);

```

* 实现类

```java
  public List<Books> queryBookByName(String bookName) {
        return bookMapper.queryBookByName(bookName);
    }
```

### controller层

```java
 //查询书籍
    @RequestMapping("/queryBook")
    public String queryBook(String queryBookName,Model model){
        List<Books> list = bookService.queryBookByName(queryBookName);

        if(list.size()==0){
            list = bookService.queryAllBook();
            model.addAttribute("error","查询不到");
        }

        model.addAttribute("list",list);
        return "allBook";
    }
```

# 项目基本流程走完

# 拦截器

* SpringMvc拦截器
* AOP思想
* 只会拦截访问的控制器的方法

* 实现HandlerInterceptor

```java
package com.cyz.config;

import org.springframework.web.servlet.HandlerInterceptor;
import org.springframework.web.servlet.ModelAndView;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class MyIntercepetor implements HandlerInterceptor {
    //return true 执行下一个拦截器 放行
    //return false 拦截
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("========处理前=================");

        return true;
    }


// 比如执行一些  拦截日志
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("========处理后=================");

    }

    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {

        System.out.println("========清理=================");
    }
}

```

* 配置applicationContext.xml

```xml
<!--    拦截器配置-->
    <mvc:interceptors>
        <mvc:interceptor>
<!--           包括这个请求下面的所有的请求-->
            <mvc:mapping path="/**"/>
            <bean class="com.cyz.config.MyIntercepetor"/>
        </mvc:interceptor>
    </mvc:interceptors>

```

* controller

```java
package com.cyz.controller;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class TsetController {

    @GetMapping("/t1")
    public String test(){
        System.out.println("test执行");
        return "hello";
    }
}

```

* 执行顺序

```
========处理前=================
test执行
========处理后=================
========清理=================
```

## 登录拦截

* controller

```java
package com.cyz.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

import javax.servlet.http.HttpSession;

@Controller
@RequestMapping("/user")
public class LoginController {

    @RequestMapping("/main")
    public String main(){
        return "main";
    }

    @RequestMapping("/goLogin")
    public String login(){
        return "login";
    }


    @RequestMapping("/login")
    public String login(HttpSession session, String username, String password, Model model){
        //把用户信息存在session中
        session.setAttribute("userLoginInfo",username);
        model.addAttribute("username",username);
        return "main";
    }

    @RequestMapping("/goOut")
    public String goOut(HttpSession session){
        session.removeAttribute("userLoginInfo");
        return "login";
    }
}

```

* index.jsp

```jsp
<%--
  Created by IntelliJ IDEA.
  User: Administrator
  Date: 2020/5/11 0022
  Time: 13:40
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>$Title$</title>
  </head>
  <body>
    <h1><a href="${pageContext.request.contextPath}/user/goLogin">登录页</a></h1>
    <h1><a href="${pageContext.request.contextPath}/user/main">首页</a></h1>
  </body>
</html>

```

* login.jsp

```jsp
<%--
  Created by IntelliJ IDEA.
  User: Administrator
  Date: 2020/5/11 0022
  Time: 13:56
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<h1>登录</h1>


<form action="${pageContext.request.contextPath}/user/login" method="post">
    用户名: <input type="text" name="username">
    密码: <input type="password" name="password">
    <input type="submit" value="提交">
</form>
</body>
</html>

```

* main.jsp

```jsp
<%--
  Created by IntelliJ IDEA.
  User: Administrator
  Date: 2020/5/11 0022
  Time: 13:56
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<h1>首页</h1>
<h3>${username}</h3>
<a href="${pageContext.request.contextPath}/user/goOut">注销</a>
</body>
</html>

```

* 拦截器

```java
package com.cyz.config;

import org.springframework.web.servlet.HandlerInterceptor;
import org.springframework.web.servlet.ModelAndView;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

public class LoginIntercepetor implements HandlerInterceptor {
    //return true 执行下一个拦截器 放行
    //return false 拦截
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        HttpSession session = request.getSession();
        //判断登录页
        if(request.getRequestURI().contains("goLogin")){
            return true;
        }
//提交登录
        if(request.getRequestURI().contains("login")){
            return true;
        }

        //判断session
        if(session.getAttribute("userLoginInfo")!=null){
            return true;
        }

        //拦截
        request.getRequestDispatcher("/WEB-INF/jsp/login.jsp").forward(request,response);
        return false;
    }

}

```

* 配置

```xml
<!--    拦截器配置-->
    <mvc:interceptors>
        <mvc:interceptor>
<!--           包括这个请求下面的所有的请求-->
            <mvc:mapping path="/**"/>
            <bean class="com.cyz.config.MyIntercepetor"/>
        </mvc:interceptor>
        <mvc:interceptor>
            <!--           包括这个请求下面的所有的请求-->
            <mvc:mapping path="/user/**"/>
            <bean class="com.cyz.config.LoginIntercepetor"/>
        </mvc:interceptor>
    </mvc:interceptors>
```

# 文件上传

* 前端配置  enctype="multipart/form-data"
* 导包

```xml
<!-- https://mvnrepository.com/artifact/commons-fileupload/commons-fileupload -->
<dependency>
    <groupId>commons-fileupload</groupId>
    <artifactId>commons-fileupload</artifactId>
    <version>1.4</version>
</dependency>
<!-- https://mvnrepository.com/artifact/javax.servlet/javax.servlet-api -->
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <version>4.0.1</version>
    <scope>provided</scope>
</dependency>

```

* 配置applicationContxt.xml

```xml
<!--    文件上传配置-->
<bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
<!--    请求的编码格式, 必须和jsp的pageEncoding属性一致 以便正确读取表单的内容, 默认ISO-8859-1-->
    <property name="defaultEncoding" value="utf-8"/>
    <!--    文件上传大小上限 10M-->
    <property name="maxUploadSize" value="10485760"/>
    <property name="maxInMemorySize" value="40960"/>
</bean>
```

* index.jsp

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>$Title$</title>
  </head>
  <body>
  <h2>方式一</h2>
  <form action="${pageContext.request.contextPath}/upload" enctype="multipart/form-data" method="post">
    <input type="file" name="file">
    <input type="submit" value="upload">
  </form>
  <h2>方式二</h2>
  <form action="${pageContext.request.contextPath}/upload2" enctype="multipart/form-data" method="post">
    <input type="file" name="file">
    <input type="submit" value="upload">
  </form>
  </body>
</html>

```

* controller

```java
package com.cyz.controller;

import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.multipart.commons.CommonsMultipartFile;

import javax.servlet.http.HttpServletRequest;
import java.io.*;

@RestController
public class FileController {

    //@RequestParam("file") 将name=file控件得到的文件封装成CommonsMultipartFile 对象
    //批量上传CommonsMultipartFile则为数组即可
    @RequestMapping("/upload")
    public String fileUpload(@RequestParam("file") CommonsMultipartFile file , HttpServletRequest request) throws IOException {

        //获取文件名 : file.getOriginalFilename();
        String uploadFileName = file.getOriginalFilename();

        //如果文件名为空，直接回到首页！
        if ("".equals(uploadFileName)){
            return "文件名为空！！";
        }
        System.out.println("上传文件名 : "+uploadFileName);

        //上传路径保存设置
        String path = request.getServletContext().getRealPath("/upload");
        //如果路径不存在，创建一个
        File realPath = new File(path);
        if (!realPath.exists()){
            realPath.mkdir();
        }
        System.out.println("上传文件保存地址："+realPath);

        InputStream is = file.getInputStream(); //文件输入流
        OutputStream os = new FileOutputStream(new File(realPath,uploadFileName)); //文件输出流

        //读取写出
        int len=0;
        byte[] buffer = new byte[1024];
        while ((len=is.read(buffer))!=-1){
            os.write(buffer,0,len);
            os.flush();
        }
        os.close();
        is.close();
        return "上传成功！！";
    }


    /*
     * 采用file.Transto 来保存上传的文件
     */
    @RequestMapping("/upload2")
    public String  fileUpload2(@RequestParam("file") CommonsMultipartFile file, HttpServletRequest request) throws IOException {

        //上传路径保存设置
        String path = request.getServletContext().getRealPath("/upload");
        File realPath = new File(path);
        if (!realPath.exists()){
            realPath.mkdir();
        }
        //上传文件地址
        System.out.println("上传文件保存地址："+realPath);

        //通过CommonsMultipartFile的方法直接写文件（注意这个时候）
        file.transferTo(new File(realPath +"/"+ file.getOriginalFilename()));

        return "上传成功！！";
    }

}

```

## 文件下载

### 文件下载步骤：

1. ### 设置 response 响应头

2. ### 读取文件 -- InputStream

3. ### 写出文件 -- OutputStream

4. ### 执行操作

5. ### 关闭流 （先开后关）



* 前端

```jsp
<a href="${pageContext.request.contextPath}/download">点击下载</a>
```

* controller

```java
@RequestMapping(value="/download")
public String downloads(HttpServletResponse response , HttpServletRequest request) throws Exception{
    //要下载的图片地址
    String  path = request.getServletContext().getRealPath("/upload");
    String  fileName = "代码图片.png";

    //1、设置response 响应头
    response.reset(); //设置页面不缓存,清空buffer
    response.setCharacterEncoding("UTF-8"); //字符编码
    response.setContentType("multipart/form-data"); //二进制传输数据
    //设置响应头
    response.setHeader("Content-Disposition",
            "attachment;fileName="+ URLEncoder.encode(fileName, "UTF-8"));

    File file = new File(path,fileName);
    //2、 读取文件--输入流
    InputStream input=new FileInputStream(file);
    //3、 写出文件--输出流
    OutputStream out = response.getOutputStream();

    byte[] buff =new byte[1024];
    int index=0;
    //4、执行 写出操作
    while((index= input.read(buff))!= -1){
        out.write(buff, 0, index);
        out.flush();
    }
    out.close();
    input.close();
    return null;
}
```

