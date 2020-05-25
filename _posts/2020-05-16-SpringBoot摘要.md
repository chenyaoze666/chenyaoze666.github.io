---
   layout: post
   title: "SpringBoot摘要"                                                        
   date: 2020-05-16 09:00:00 +0530
   categories: JAVA SpringBoot
---
  JAVA SpringBoot


# SpringBoot

# 微服务阶段

* javase: OOP

  

* html+css+js+jquery+框架
* javaweb: 独立开发MVC三层架构
* SSM: 框架  简化开发流程

  * war:tomcat运行

* spring在简化 : SpringBoot: 微服务架构

  * SpringBoot-jar 内嵌tomcat
* springCloud
* Linux

# SpringBoot初识

* 是什么

* 配置编写  yaml

* 自动装配原理

* 集成业务开发:

* 集成数据库

* 分布式开发: Dubbo(RPC)+zookeepar

* swagger:接口

* 任务调度

* SpringSecrity/Shiro

# springcloud初识

* 微服务
* springcloud入门
* Restful
* Eureka
* Ribbon
* Feign
* HyStrix
* Zuul路由网关
* SpringCloud config : git

# SpringBoot

*  简化web项目
* 约定大于配置

# 微服务架构

* 模块化
* 功能独立

# 环境

```
jdk1.8

maven 3.6.1

springboot

idea
```

* 创建项目
* 查看主程序  底层就是一个组件
* 查看包

```xml
<dependencies>
    <!--        启动器-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>
    
<!--		web依赖 启动tomcat-->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

<!--单元测试-->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
			<exclusions>
				<exclusion>
					<groupId>org.junit.vintage</groupId>
					<artifactId>junit-vintage-engine</artifactId>
				</exclusion>
			</exclusions>
		</dependency>
	</dependencies>
```

* springboot banner

```
下载banner.txt
```

# 自动装配原理

## pom.xml

```xml
存放这依赖   父依赖中版本管理  所以我们加入依赖的时候不需要指定版本

启动器    需要什么就导入什么的启动器  不日需要web 就导入spring-boot-starter-web
						会自动导入web所需的依赖
 		<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>
```

* springboot会将所有的功能场景, 都变为一个启动器
* 我们需要使用什么功能, 就需要引入对应的启动器  starter

## 主程序

```java
@SpringBootApplication //标注这个类是一个springboot的应用
public class Springboot01HelloworldApplication {

    public static void main(String[] args) {
        //将springboot应用启动
        SpringApplication.run(Springboot01HelloworldApplication.class, args);
    }

}
```

## 注解

```java
@SpringBootConfiguration //springboot的配置
	@Configuration //spring配置类
		@Component //spring组件

@EnableAutoConfiguration //自动配置
	@AutoConfigurationPackage //自动导入包
		@Import({AutoConfigurationPackages.Registrar.class}) //自动配置 包注册器
	@Import(AutoConfigurationImportSelector.class) //自动配置 导入选择器 

//获取所有的配置

//配置文件 spring.factories

//所有的配置加载到类中
```

* springboot所有的自动配置都是在启动的扫描并加载:
  * spring.factories所有的配置类都在这里
  * 当是不一定生效, 要判断条件是否成立
  * 只要导入 对应的starter, 就有了对应的启动器
  * 有了启动器, 我们自动装配就会生效
  * 然后就配置成功了
* springboot在启动的时候, 从类路径下的/META-INF/spring.factories获取指定的值
* 将这些自动配置的类导入容器, 自动配置就会生效, 帮我们进行配置
* 以前我们需要自动配置的东西, 现在springboot帮我们做了
* 整合javaEE, 解决方案和自动配置的东西都在spring-boot-autoconfigure-2.3.0.RELEASE.jar包下
* 它会把所有需要导入的组件, 以类名的方式返回, 这些组件就会被添加到容器中
* 容器中也会存在非常多的xxxAutoConfiguration的文件(@Bean), 就是这些类给容器中导入了这个场景需要的所有组件, 并自动配置, @Configuration javaConfig
* 有了自动配置类, 免去了我们手动编写配置文件的工作

## 启动类如何运行

* run

* 开启了一个服务

### 四件事情

1. 推断应用的类型时普通的项目还是web项目
2. 查找并加载所有可用的初始化器, 设置到initializers属性中
3. 找出所有的应用程序监听器,  设置到listeners属性中
4. 推断并设置main方法的定义类, 找到运行的主类

#### 关于springboot

* 自动装配
* 判断是普通项目还是web项目
* 推断主类
* run
* run里的监听器
  * 获取上下文处理bean

# Yaml

* 基本语法
* 支持随机属性值语法(#{random.uuid},#{random.int})
* 支持松散绑定(类属性名: firstName 配置名: first-name: 旺财)
* JSR303校验

```yaml
# springboot到底能配置写什么

# 官方配置太多, 简化理解原理

# 注意空格

# 普通key-value
name: cyz

# 对象
student:
  name: cyz
  age: 18

# 行内写法
student2: {name: cyz,age: 18}

# 数组
pets:
  - cat
  - dog
  - pig

pets2: [cat,dog,pig]

server:
  port: 8081
```

* 类赋值

```java
package com.cyz.pojo;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

@Component
public class Dog {

    //原先通过注解赋值
    @Value("旺财")
    private String name;
    @Value("3")
    private Integer age;

    public Dog() {
    }

    public Dog(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "Dog{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}

```

```java
package com.cyz.pojo;

import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.stereotype.Component;

import java.util.Date;
import java.util.List;
import java.util.Map;

@Component
@ConfigurationProperties(prefix = "person")
public class Person {
    private String name;
    private Integer age;
    private Boolean happy;
    private Date birth;
    private Map<String,Object> maps;
    private List<Object> lists;
    private Dog dog;

    public Person() {
    }

    public Person(String name, Integer age, Boolean happy, Date birth, Map<String, Object> maps, List<Object> lists, Dog dog) {
        this.name = name;
        this.age = age;
        this.happy = happy;
        this.birth = birth;
        this.maps = maps;
        this.lists = lists;
        this.dog = dog;
    }


    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    public Boolean getHappy() {
        return happy;
    }

    public void setHappy(Boolean happy) {
        this.happy = happy;
    }

    public Date getBirth() {
        return birth;
    }

    public void setBirth(Date birth) {
        this.birth = birth;
    }

    public Map<String, Object> getMaps() {
        return maps;
    }

    public void setMaps(Map<String, Object> maps) {
        this.maps = maps;
    }

    public List<Object> getLists() {
        return lists;
    }

    public void setLists(List<Object> lists) {
        this.lists = lists;
    }

    public Dog getDog() {
        return dog;
    }

    public void setDog(Dog dog) {
        this.dog = dog;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", happy=" + happy +
                ", birth=" + birth +
                ", maps=" + maps +
                ", lists=" + lists +
                ", dog=" + dog +
                '}';
    }
}



```

* @ConfigurationProperties需要导入包

```xml
 <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-configuration-processor</artifactId>
            <optional>true</optional>
        </dependency>
```

* yaml

```yaml
# yaml可以给实体类配置

person:
  name: cyz
  age: 18
  happy: false
  birth: 2020/05/15
  maps: {k1: v1,k2: v2}
  lists:
    - code
    - music
    - girl
  dog:
    name: 旺财
    age: 3

```

```java
@SpringBootTest
class Springboot02ConfigApplicationTests {

    @Autowired
    private Person person;

    @Test
    void contextLoads() {
        System.out.println(person);
    }

}

//Person{name='cyz', age=18, happy=false, birth=Fri May 15 00:00:00 CST 2020, maps={k1=v1, k2=v2}, lists=[code, music, girl], dog=Dog{name='旺财', age=3}}
```

# 多环境配置

* 项目目录/config/application.yaml    //默认走这个  1
* 项目目录/application.yaml                //2走这个
* 项目目录/src/main/resources/config/application.yaml //3走这个
* 项目目录/src/main/resources/application.yaml//4 走这个  默认的配置文件 优先级最低

## 环境切换

* properties
* 配置

```properties
application.properties中配置
	server.port=8080

application-test.properties中配置
	server.port=8081

application-dev.properties中配置
	server.port=8082
```

* application.properties

```properties
# 默认环境
server.port=8080
# springboot多环境选择  只用改dev/test
spring.profiles.active=dev
```

* yml
* 配置 使用**---**分隔

```yaml
server:
  port: 8081
spring:
  profiles:
    active: dev
    
---
server:
  port: 8082
spring:
  profiles: dev

---
server:
  port: 8083
spring:
  profiles: test
```

# 配置文件

```yaml
# 和spring.factories的关系
# 规律: xxxAutoConfiguration  都存在一个xxxProperties (有默认值)  和配置文件绑定我们就能修改自定义配置 这就是yml做的
```

## 自动装配原理

1. SpringBoot自动会加载大量的自动配置类
2. 我们看我们需要的功能有没有在SpringBoot默认写好的自动配置类当中
3. 我们再来看这个自动配置类到底配置了那些组件: (只要我们要用的组件存在其中, 我们就不需要在手动配置了)
4. 给容器中自动配置类添加组件的时候, 会从properties类中获取某些属性, 我们需要在配置文件中指定这些属性的值即可

xxxAutoConfiguration: 自动配置类; 给容器中添加组件

xxxProperties封装配置文件中相关属性

```yaml
debug: true  
# 查看自动配置类生效情况
```

# Web开发

## 静态资源导入

* 分析

```java
public void addResourceHandlers(ResourceHandlerRegistry registry) {
    //判断是否手动配置了
	if (!this.resourceProperties.isAddMappings()) {
		logger.debug("Default resource handling disabled");
		return;
    }
	Duration cachePeriod = this.resourceProperties.getCache().getPeriod();
	CacheControl cacheControl = 					this.resourceProperties.getCache().getCachecontrol().toHttpCacheControl();
	//方式一 webjars  静态资源位置
    if (!registry.hasMappingForPattern("/webjars/**")) {
						customizeResourceHandlerRegistration(registry.addResourceHandler("/webjars/**")
						.addResourceLocations("classpath:/META-INF/resources/webjars/")
						.setCachePeriod(getSeconds(cachePeriod)).setCacheControl(cacheControl));
			}
    //方式二
    /*
    		private static final String[] CLASSPATH_RESOURCE_LOCATIONS = { "classpath:/META-INF/resources/",
			"classpath:/resources/", "classpath:/static/", "classpath:/public/" };
    */
	String staticPathPattern = this.mvcProperties.getStaticPathPattern();
	if (!registry.hasMappingForPattern(staticPathPattern)) {
								customizeResourceHandlerRegistration(registry.addResourceHandler(staticPathPattern)
						.addResourceLocations(getResourceLocations(this.resourceProperties.getStaticLocations()))
						.setCachePeriod(getSeconds(cachePeriod)).setCacheControl(cacheControl));
			}
		}
```

### 方式一

* webjars 方式引入架包

### 方式二

* 当前目录下

```
    //方式二
    /*
    		private static final String[] CLASSPATH_RESOURCE_LOCATIONS = { "classpath:/META-INF/resources/",
			"classpath:/resources/", "classpath:/static/", "classpath:/public/" };
```

* 优先级  resources > static > public

### 方式三

* 自定义
* 原来的自动配置将失效

```properties
spring.mvc.static-path-pattern=/hello,classpath:/cyz/
```

## 首页

* 自定义
* 静态资源位置

```java
@Bean
		public WelcomePageHandlerMapping welcomePageHandlerMapping(ApplicationContext applicationContext,
				FormattingConversionService mvcConversionService, ResourceUrlProvider mvcResourceUrlProvider) {
			WelcomePageHandlerMapping welcomePageHandlerMapping = new WelcomePageHandlerMapping(
					new TemplateAvailabilityProviders(applicationContext), applicationContext, getWelcomePage(),
					this.mvcProperties.getStaticPathPattern());
			welcomePageHandlerMapping.setInterceptors(getInterceptors(mvcConversionService, mvcResourceUrlProvider));
			welcomePageHandlerMapping.setCorsConfigurations(getCorsConfigurations());
			return welcomePageHandlerMapping;
		}

		private Optional<Resource> getWelcomePage() {
			String[] locations = getResourceLocations(this.resourceProperties.getStaticLocations());
			return Arrays.stream(locations).map(this::getIndexHtml).filter(this::isReadable).findFirst();
		}

		private Resource getIndexHtml(String location) {
			return this.resourceLoader.getResource(location + "index.html");
		}

		private boolean isReadable(Resource resource) {
			try {
				return resource.exists() && (resource.getURL() != null);
			}
			catch (Exception ex) {
				return false;
			}
		}
```

* 一般我们讲index.html放在templates
  * templates下的只能通过controller跳转
  * 需要模板引擎支持

## 图标

* ```
  <link rel="shortcut icon" href="/favicon1.ico"/>
  ```

## jsp->模板引擎Thymeleaf

* 引入

```xml
	 <!--Thymeleaf-->
        <dependency>
            <groupId>org.thymeleaf</groupId>
            <artifactId>thymeleaf-spring5</artifactId>
        </dependency>
        <dependency>
            <groupId>org.thymeleaf.extras</groupId>
            <artifactId>thymeleaf-extras-java8time</artifactId>
        </dependency>
```

* controller跳转
* html写在Thymeleaf目录下
* 导入约束

```
xmlns:th="http://www.thymeleaf.org"
```

```
变量   			${}
选择器 		   *{}
文本   			#{}
链接   			@{}
fragment 		 ~{}
基本运算
and,or
!, not
> < >= <=		gt lt ge le
== !=			eq ne
(if) ? (then)
(if) ? (then) : (else)
(value) ?: (defaultvalue)
```

```
th:insert
th:replace

th:each

th:if
th:unless
th:switch
th:case

th:object
th:with

th:attr
th:attrprepend
th:attrappend

th:value
th:href
th:src
...
th:text			不转义
th:utext		转义


th:fragment

th:remove
```

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<div th:text="${msg}"></div>
<div th:utext="${msg}"></div>

<hr>

<h3 th:each="user:${users}" th:text="${user}"></h3>
<!--<h3 th:each="user:${users}">[[ ${user} ]]</h3>-->
</body>
</html>
```

## 装配扩展SpringMVC

### 自定义视图解析器

```java
package com.cyz.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.View;
import org.springframework.web.servlet.ViewResolver;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

import java.util.Locale;


//全面扩展springmvc
@Configuration
public class MyMvcConfig implements WebMvcConfigurer {


    //ViewResolver 实现了视图解析器的接口 我们就可以把他看做视图解析器
    @Bean
    public ViewResolver myViewResolver(){
        return new MyViewResolver();
    }

//    自定义视图解析器
    public static class MyViewResolver implements ViewResolver{
        @Override
        public View resolveViewName(String s, Locale locale) throws Exception {
            return null;
        }
    }


}

```

### 视图跳转

```
package com.cyz.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.View;
import org.springframework.web.servlet.ViewResolver;
import org.springframework.web.servlet.config.annotation.ViewControllerRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

import java.util.Locale;

@Configuration
public class MyMvcConfig implements WebMvcConfigurer {

//    视图跳转
    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        registry.addViewController("/cyz").setViewName("test");
    }
}

```

## 增删改查

* 引入静态资源
* pojo

```java
package com.cyz.pojo;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@AllArgsConstructor
@NoArgsConstructor
public class Department {
    private Integer id;
    private String departmentName;
}


package com.cyz.pojo;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

import java.util.Date;

@Data
@NoArgsConstructor
public class Employee {
    private Integer id;;
    private String lastName;
    private String email;
    private Integer gender;//0: 女 1: 男
    private Department department;
    private Date birth;


    public Employee(Integer id, String lastName, String email, Integer gender, Department department) {
        this.id = id;
        this.lastName = lastName;
        this.email = email;
        this.gender = gender;
        this.department = department;
        //默认日期
        this.birth = new Date();
    }
}


```

* dao

```java
package com.cyz.dao;

import com.cyz.pojo.Department;
import org.springframework.stereotype.Repository;

import java.util.Collection;
import java.util.HashMap;
import java.util.Map;

@Repository
public class DepartmentDao {
    //模拟数据
    private static Map<Integer, Department> departments = null;

    static{
        departments = new HashMap<Integer, Department>();

        departments.put(101,new Department(101,"教学部"));
        departments.put(102,new Department(102,"市场部"));
        departments.put(103,new Department(103,"教研部"));
        departments.put(104,new Department(104,"运营部"));
        departments.put(105,new Department(105,"后勤部"));
    }

    public Collection<Department> getDepartments(){
        return departments.values();
    }

    public Department getDepartmentById(Integer id){
        return departments.get(id);
    }
}



package com.cyz.dao;

import com.cyz.pojo.Department;
import com.cyz.pojo.Employee;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Repository;

import java.util.Collection;
import java.util.HashMap;
import java.util.Map;

@Repository
public class EmployeeDao {

    private static Map<Integer, Employee> employees = null;

    @Autowired
    private DepartmentDao departmentDao;

    static{
        employees = new HashMap<Integer, Employee>();

        employees.put(1001,new Employee(1001,"AA","2111111@qq.com",0,new Department(101,"教学部")));
        employees.put(1002,new Employee(1002,"BB","2222222@qq.com",1,new Department(102,"市场部")));
        employees.put(1003,new Employee(1003,"CC","2333333@qq.com",0,new Department(103,"教研部")));
        employees.put(1004,new Employee(1004,"DD","2444444@qq.com",1,new Department(104,"运营部")));
        employees.put(1005,new Employee(1005,"EE","2555555@qq.com",0,new Department(105,"后勤部")));
    }

    //增删改查

    //主键自增
    private static Integer initId = 1006;

    //add
    public void save(Employee employee){
        if (employee.getId() == null){
            employee.setId(initId++);
        }

        employee.setDepartment(departmentDao.getDepartmentById(employee.getDepartment().getId()));
        employees.put(employee.getId(),employee);
    }

    //query
    public Collection<Employee> getAll(){
        return employees.values();
    }

    //queryById
    public Employee getEmployeeById(Integer id){
        return employees.get(id);
    }

    //delete
    public void delete(Integer id){
        employees.remove(id);
    }
}

```

## 配置首页

```java
package com.cyz.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.View;
import org.springframework.web.servlet.ViewResolver;
import org.springframework.web.servlet.config.annotation.ViewControllerRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

import java.util.Locale;

@Configuration
public class MyMvcConfig implements WebMvcConfigurer {

//    视图跳转
    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        registry.addViewController("/").setViewName("index");
        registry.addViewController("/index.html").setViewName("index");
    }
}
```

* 使用模板引擎改造资源

```
xmlns:th="http://www.thymeleaf.org"
```

* 关闭模板引擎缓存

```properties
# 关闭模板引擎的缓存
spring.thymeleaf.cache=false
```

## 国际化

* 保证setting -> file encodings 的编码为utf-8	
* 在i18n目录下新建login.properties和login_zh_CN.properties

```properties
login.btn=登录
login.password=密码
login.remember=记住我
login.tip=请登录
login.username=用户名
```
```properties
login.btn=Sign in
login.password=Password
login.remember=Remember me
login.tip=Please sign in
login.username=Username
```
* 合并后可以通过idea添加
* idea可以可视化配置
* 配置文件

```properties
# 配置国际化 绑定配置文件位置
spring.messages.basename=i18n.login
```

* 修改代码

```html
<h1 class="h3 mb-3 font-weight-normal" th:text="#{login.tip}"></h1>

<a class="btn btn-sm" th:href="@{/index.html(l='zh_CN')}">中文</a>
<a class="btn btn-sm" th:href="@{/index.html(l='en_US')}">English</a>
```

* 自定义国际化解析器

```java
package com.cyz.config;

import org.springframework.web.servlet.LocaleResolver;
import org.thymeleaf.util.StringUtils;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.util.Locale;

public class MyLocaleResolver implements LocaleResolver {

    //解析请求
    @Override
    public Locale resolveLocale(HttpServletRequest httpServletRequest) {
        //获取请求中的参数连接
        String language = httpServletRequest.getParameter("l");

        Locale locale = Locale.getDefault();//如果没有就使用默认的

        //如果不为空
        if(!StringUtils.isEmpty(language)){
            //zh_CN
            String[] split = language.split("_");
            //国家, 地区
            new Locale(split[0],split[1]);
        }

        return locale;
    }

    @Override
    public void setLocale(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Locale locale) {

    }
}

```

* 使其生效

```java
package com.cyz.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.LocaleResolver;
import org.springframework.web.servlet.View;
import org.springframework.web.servlet.ViewResolver;
import org.springframework.web.servlet.config.annotation.ViewControllerRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

import java.util.Locale;

@Configuration
public class MyMvcConfig implements WebMvcConfigurer {

//    视图跳转
    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        registry.addViewController("/").setViewName("index");
        registry.addViewController("/index.html").setViewName("index");
    }

    //自定义的国际化组件生效
    @Bean
    public LocaleResolver localeResolver(){
        return new MyLocaleResolver();
    }
}

```

## 登录

* index.html
* 表达式

```html
<p style="color:red" th:text="${msg}" th:if="${not #strings.isEmpty(msg)}"></p>
```

```html
<form class="form-signin" th:action="@{/user/login}" method="post">
			<img class="mb-4" th:src="@{/img/bootstrap-solid.svg}" alt="" width="72" height="72">
			<h1 class="h3 mb-3 font-weight-normal" th:text="#{login.tip}"></h1>
    
<!--			如果msg的值为空显示-->
			<p style="color:red" th:text="${msg}" th:if="${not #strings.isEmpty(msg)}"></p>
    
			<input type="text" name="username" class="form-control" th:placeholder="#{login.username}" required="" autofocus="">
			<input type="password" name="password" class="form-control" th:placeholder="#{login.password}" required="">
    
			<div class="checkbox mb-3">
				<label>
          <input type="checkbox" value="remember-me" th:text="#{login.remember}">
        </label>
			</div>
			<button class="btn btn-lg btn-primary btn-block" type="submit" th:text="#{login.btn}">Sign in</button>
			<p class="mt-5 mb-3 text-muted">© 2017-2018</p>
			<a class="btn btn-sm" th:href="@{/index.html(l='zh_CN')}">中文</a>
			<a class="btn btn-sm" th:href="@{/index.html(l='en_US')}">English</a>
		</form>
```

* controller
* 使用重定向

```java
 return "redirect:/main.html";
```

```java
package com.cyz.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.ResponseBody;
import org.thymeleaf.util.StringUtils;

import javax.servlet.http.HttpSession;

@Controller
public class LoginController {

    @RequestMapping("/user/login")
    public String login(
            @RequestParam("username") String username,
            @RequestParam("password") String password,
            Model model,
            HttpSession session){

        //业务
        if(!StringUtils.isEmpty(username) && "123456".equals(password)){
            //存放session
            session.setAttribute("loginUser",username);

            return "redirect:/main.html";
        }else{
            //登录失败消息
            model.addAttribute("msg","用户名或密码错误!");
            return "index";
        }
    }
}

```

* 配置地址映射

```java
//    视图跳转
    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        registry.addViewController("/").setViewName("index");
        registry.addViewController("/index.html").setViewName("index");
        registry.addViewController("/main.html").setViewName("dashboard");
    }
```

## 拦截器

```java
package com.cyz.config;

import org.springframework.web.servlet.HandlerInterceptor;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class LoginHandlerIntercepter implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        //登录成功之后 用该有用户的session

        Object loginUser = request.getSession().getAttribute("loginUser");

        if(loginUser==null){
            request.setAttribute("msg","没有权限, 清先登录");
            request.getRequestDispatcher("/index.html").forward(request,response);
            return false;
        }else{
            return true;
        }
    }
}

```

* 配置

```java
  //配置拦截器
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new LoginHandlerIntercepter())
                .addPathPatterns("/**")
                .excludePathPatterns("/index.html","/","/user/login","/css/*","/js/**","/img/**");
    }
```

## 增删改查

* 抽取相同的模块

```html
<nav class="col-md-2 d-none d-md-block bg-light sidebar" th:fragment="sidebar">
    ...
</nav>
```

* 插入模块

```html
<div th:insert="~{dashboard::sidebar}"></div>
```

* 单独封装

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">

<!--头部导航栏-->
<nav class="navbar navbar-dark sticky-top bg-dark flex-md-nowrap p-0" th:fragment="topbar">
    <a class="navbar-brand col-sm-3 col-md-2 mr-0" href="http://getbootstrap.com/docs/4.0/examples/dashboard/#" th:text="${session.loginUser}"></a>
    <input class="form-control form-control-dark w-100" type="text" placeholder="Search" aria-label="Search">
    <ul class="navbar-nav px-3">
        <li class="nav-item text-nowrap">
            <a class="nav-link" href="http://getbootstrap.com/docs/4.0/examples/dashboard/#">注销</a>
        </li>
    </ul>
</nav>


<!-- 侧边栏 -->
<nav class="col-md-2 d-none d-md-block bg-light sidebar" th:fragment="sidebar">
    <div class="sidebar-sticky">
        <ul class="nav flex-column">
            <li class="nav-item">
                <a class="nav-link active" th:href="@{/index.html}">
                    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-home">
                        <path d="M3 9l9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"></path>
                        <polyline points="9 22 9 12 15 12 15 22"></polyline>
                    </svg>
                    首页 <span class="sr-only">(current)</span>
                </a>
            </li>
            <li class="nav-item">
                <a class="nav-link" href="http://getbootstrap.com/docs/4.0/examples/dashboard/#">
                    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-file">
                        <path d="M13 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V9z"></path>
                        <polyline points="13 2 13 9 20 9"></polyline>
                    </svg>
                    Orders
                </a>
            </li>
            <li class="nav-item">
                <a class="nav-link" href="">
                    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-shopping-cart">
                        <circle cx="9" cy="21" r="1"></circle>
                        <circle cx="20" cy="21" r="1"></circle>
                        <path d="M1 1h4l2.68 13.39a2 2 0 0 0 2 1.61h9.72a2 2 0 0 0 2-1.61L23 6H6"></path>
                    </svg>
                    Products
                </a>
            </li>
            <li class="nav-item">
                <a class="nav-link" th:href="@{/emps}">
                    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-users">
                        <path d="M17 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"></path>
                        <circle cx="9" cy="7" r="4"></circle>
                        <path d="M23 21v-2a4 4 0 0 0-3-3.87"></path>
                        <path d="M16 3.13a4 4 0 0 1 0 7.75"></path>
                    </svg>
                    员工管理
                </a>
            </li>
            <li class="nav-item">
                <a class="nav-link" href="http://getbootstrap.com/docs/4.0/examples/dashboard/#">
                    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-bar-chart-2">
                        <line x1="18" y1="20" x2="18" y2="10"></line>
                        <line x1="12" y1="20" x2="12" y2="4"></line>
                        <line x1="6" y1="20" x2="6" y2="14"></line>
                    </svg>
                    Reports
                </a>
            </li>
            <li class="nav-item">
                <a class="nav-link" href="http://getbootstrap.com/docs/4.0/examples/dashboard/#">
                    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-layers">
                        <polygon points="12 2 2 7 12 12 22 7 12 2"></polygon>
                        <polyline points="2 17 12 22 22 17"></polyline>
                        <polyline points="2 12 12 17 22 12"></polyline>
                    </svg>
                    Integrations
                </a>
            </li>
        </ul>

        <h6 class="sidebar-heading d-flex justify-content-between align-items-center px-3 mt-4 mb-1 text-muted">
            <span>Saved reports</span>
            <a class="d-flex align-items-center text-muted" href="http://getbootstrap.com/docs/4.0/examples/dashboard/#">
                <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-plus-circle"><circle cx="12" cy="12" r="10"></circle><line x1="12" y1="8" x2="12" y2="16"></line><line x1="8" y1="12" x2="16" y2="12"></line></svg>
            </a>
        </h6>
        <ul class="nav flex-column mb-2">
            <li class="nav-item">
                <a class="nav-link" href="http://getbootstrap.com/docs/4.0/examples/dashboard/#">
                    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-file-text">
                        <path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"></path>
                        <polyline points="14 2 14 8 20 8"></polyline>
                        <line x1="16" y1="13" x2="8" y2="13"></line>
                        <line x1="16" y1="17" x2="8" y2="17"></line>
                        <polyline points="10 9 9 9 8 9"></polyline>
                    </svg>
                    Current month
                </a>
            </li>
            <li class="nav-item">
                <a class="nav-link" href="http://getbootstrap.com/docs/4.0/examples/dashboard/#">
                    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-file-text">
                        <path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"></path>
                        <polyline points="14 2 14 8 20 8"></polyline>
                        <line x1="16" y1="13" x2="8" y2="13"></line>
                        <line x1="16" y1="17" x2="8" y2="17"></line>
                        <polyline points="10 9 9 9 8 9"></polyline>
                    </svg>
                    Last quarter
                </a>
            </li>
            <li class="nav-item">
                <a class="nav-link" href="http://getbootstrap.com/docs/4.0/examples/dashboard/#">
                    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-file-text">
                        <path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"></path>
                        <polyline points="14 2 14 8 20 8"></polyline>
                        <line x1="16" y1="13" x2="8" y2="13"></line>
                        <line x1="16" y1="17" x2="8" y2="17"></line>
                        <polyline points="10 9 9 9 8 9"></polyline>
                    </svg>
                    Social engagement
                </a>
            </li>
            <li class="nav-item">
                <a class="nav-link" href="http://getbootstrap.com/docs/4.0/examples/dashboard/#">
                    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-file-text">
                        <path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"></path>
                        <polyline points="14 2 14 8 20 8"></polyline>
                        <line x1="16" y1="13" x2="8" y2="13"></line>
                        <line x1="16" y1="17" x2="8" y2="17"></line>
                        <polyline points="10 9 9 9 8 9"></polyline>
                    </svg>
                    Year-end sale
                </a>
            </li>
        </ul>
    </div>
</nav>


</html>
```

* 引用

```html
<div th:replace="~{commons/commons::topbar}"></div>

<div th:replace="~{commons/commons::sidebar}"></div>
```

* 传递参数

```html
 <!-- 传递参数 -->
 <div th:replace="~{commons/commons::sidebar(active='main.html')}"></div>

<div th:replace="~{commons/commons::sidebar(active='list.html')}"></div>

```

* 动态表达式

```xml
<a th:class="${active=='main.html' ? 'nav-link active' : 'nav-link'}" th:href="@{/index.html}">
    
<a th:class="${active=='list.html' ? 'nav-link active' : 'nav-link'}" th:href="@{/emps}">
                
```

* 显示员工 遍历和日期转换

```html
<tr th:each="emp:${emps}">
	<td th:text="${emp.getId()}"></td>
	<td th:text="${emp.getLastName()}"></td>
	<td th:text="${emp.getEmail()}"></td>
	<td th:text="${emp.getGender()==0 ? '女' : '男'}"></td>
	<td th:text="${emp.department.getDepartmentName()}"></td>
	<td th:text="${#dates.format(emp.getBirth(),'yyyy-MM-dd HH:mm:ss')}">	</td>
	<td>
		<button class="btn btn-sm btn-primary">编辑</button>
		<button class="btn btn-sm btn-danger">删除</button>
	</td>
</tr>
```

### 添加员工7

* add.html

```html
...
	<body>
	<div th:replace="~{commons/commons::topbar}"></div>

	<div class="container-fluid">
			<div class="row">
				<div th:replace="~{commons/commons::sidebar(active='list.html')}"></div>

					<main role="main" class="col-md-9 ml-sm-auto col-lg-10 pt-3 px-4">
						<form th:action="@{/emp}" method="post">
							<div class="form-group">
								<label>LastName</label>
								<input type="text" name="lastName" class="form-control" placeholder="cyz">
							</div>
							<div class="form-group">
								<label>Email</label>
								<input type="email" name="email" class="form-control" placeholder="12212@qq.com">
							</div>
							<div class="form-group">
								<label>Gender</label><br/>
								<div class="form-check form-check-inline">
									<input class="form-check-input" type="radio" name="gender" value="1">
									<label class="form-check-label">男</label>
								</div>
								<div class="form-check form-check-inline">
									<input class="form-check-input" type="radio" name="gender" value="0">
									<label class="form-check-label">女</label>
								</div>
							</div>
							<div class="form-group">
								<label>Department</label>
								<select class="form-control" name="department.id">
									<option
											th:each="dept:${departments}"
											th:text="${dept.getDepartmentName()}"
											th:value="${dept.getId()}">
									</option>
								</select>
							</div>
							<div class="form-group">
								<label>Birth</label>
								<input type="text" name="birth" class="form-control" placeholder="yyyy-MM-dd">
							</div>
							<button type="submit" class="btn btn-primary">添加</button>
						</form>
				</main>
			</div>
		</div>
...
	</body>
</html>
```

* controller

```java
   @GetMapping("/emp")
    public String toAddpage(Model model){
        //查出所有部门信息
        Collection<Department> departments = departmentDao.getDepartments();
        model.addAttribute("departments",departments);
        return "emp/add";
    }

    @PostMapping("/emp")
    public String addEmp(Employee employee){
       System.out.println("e"+employee);
        employeeDao.save(employee);//保存员工信息

        //forward
        //redirect
        return "redirect:emps";
    }
```

* 配置

```properties
# 时间日期格式
spring.mvc.date-format=yyyy-MM-dd
```

### 修改和删除员工

```html
<td>
										<a class="btn btn-sm btn-primary" th:href="@{'/emp/'+${emp.getId()}}">编辑</a>
										<a class="btn btn-sm btn-danger" th:href="@{'/emp/'+${emp.getId()}}">删除</a>
									</td>
```

* controller

```java
//去修改
    @GetMapping("/emp/{id}")
    public String toUpdateEmp(@PathVariable("id") Integer id, Model model){
        //查数据
        Employee employee = employeeDao.getEmployeeById(id);

        Collection<Department> departments = departmentDao.getDepartments();
        model.addAttribute("departments",departments);

        model.addAttribute("emp",employee);
        return "emp/update";
    }

    //修改
    @PostMapping("/updateEmp")
    public String updateEmp(Employee employee){
        employeeDao.save(employee);
        return "redirect:/emps";
    }


    //删除
    @GetMapping("/delemp/{id}")
    public String delemp(@PathVariable int id){
        employeeDao.delete(id);
        return "redirect:/emps";
    }
```

* update.html

```html
...
	<body>
	<div th:replace="~{commons/commons::topbar}"></div>

	<div class="container-fluid">
			<div class="row">
				<div th:replace="~{commons/commons::sidebar(active='list.html')}"></div>

					<main role="main" class="col-md-9 ml-sm-auto col-lg-10 pt-3 px-4">
						<form th:action="@{/updateEmp}" method="post">
							<input type="hidden" name="id" th:value="${emp.getId()}">
							<div class="form-group">
								<label>LastName</label>
								<input th:value="${emp.getLastName()}" type="text" name="lastName" class="form-control" placeholder="cyz">
							</div>
							<div class="form-group">
								<label>Email</label>
								<input th:value="${emp.getEmail()}" type="email" name="email" class="form-control" placeholder="12212@qq.com">
							</div>
							<div class="form-group">
								<label>Gender</label><br/>
								<div class="form-check form-check-inline">
									<input th:checked="${emp.getGender()==1}" class="form-check-input" type="radio" name="gender" value="1">
									<label class="form-check-label">男</label>
								</div>
								<div class="form-check form-check-inline">
									<input th:checked="${emp.getGender()==0}" class="form-check-input" type="radio" name="gender" value="0">
									<label class="form-check-label">女</label>
								</div>
							</div>
							<div class="form-group">
								<label>Department</label>
								<select class="form-control" name="department.id">
									<option
											th:selected="${dept.getId()==emp.getDepartment().getId()}"
											th:each="dept:${departments}"
											th:text="${dept.getDepartmentName()}"
											th:value="${dept.getId()}">
									</option>
								</select>
							</div>
							<div class="form-group">
								<label>Birth</label>
								<input th:value="${#dates.format(emp.getBirth(),'yyyy-MM-dd')}" type="text" name="birth" class="form-control" placeholder="yyyy-MM-dd">
							</div>
							<button type="submit" class="btn btn-primary">修改</button>
						</form>
				</main>
			</div>
		</div>
	....

	</body>

</html>
```

### 注销

```html
<a class="nav-link" th:href="@{/user/logout}">注销</a>
```

* controller

```java
@RequestMapping("/user/logout")
    public String logout(HttpSession session){
        session.invalidate();
        return "redirect:/index.html";
    }
```

### 404页面

* 直接在/static/error下放404.html即可 
* 会自动跳转到该目录下

# 快速搭建网页

* 前端页面  数据
* 设计数据库
* 前端独立化
* 数据接口对接
* 前后端联调
* 
* 后台模板
* 前端页面