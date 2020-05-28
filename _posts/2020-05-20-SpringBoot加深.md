---
   layout: post
   title: "SpringBoot加深"                                                        
   date: 2020-05-20 09:00:00 +0530
   categories: JAVA SpringBoo tSpringSecurity Shiro Swagger Dubbo Zookeeper
---
  JAVA SpringBoo tSpringSecurity Shiro Swagger Dubbo Zookeeper


# SpringBoot加深

# 数据库交互

## Spring Data

* 依赖

```xml
 <dependencies>
        <!--Web-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <!--JDBC-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jdbc</artifactId>
        </dependency>


        <!--MySQL-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <scope>runtime</scope>
        </dependency>

        <!--junit-->
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

* 配置数据源

```yaml
spring:
  datasource:
    username: root
    password: 123456
    url: jdbc:mysql://localhost:3306/mybatis?serverTimezone=UTC&useUncode=true&characterEncoding=utf-8
    driver-class-name: com.mysql.cj.jdbc.Driver
```

* 测试类

```java
package com.cyz;

import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

import javax.sql.DataSource;
import java.sql.Connection;
import java.sql.SQLException;

@SpringBootTest
class Springboot04DataApplicationTests {

    @Autowired
    DataSource dataSource;;

    @Test
    void contextLoads() throws SQLException {
        //默认数据源  springboot自动完成com.zaxxer.hikari.HikariDataSource
        System.out.println(dataSource.getClass());

        //获得连接
        Connection connection = dataSource.getConnection();
        System.out.println(connection);

        //xxxTemplate  springboot 已经配置好的bean   直接使用

        //关闭
        connection.close();
    }

}

```

# JDBC实现

* controller

```java
package com.cyz.controller;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;

import java.util.List;
import java.util.Map;

@RestController
public class JDBCController {

    @Autowired
    JdbcTemplate jdbcTemplate;

    //查询数据库所有信息
    //没有实体类, 数据库中的东西 怎么获取 Map
    @GetMapping("/userList")
    public List<Map<String,Object>> userList(){
        String sql = "select * from user";
        List<Map<String,Object>> list_maps = jdbcTemplate.queryForList(sql);

        return list_maps;
    }

    //新增
    @GetMapping("/addUser")
    public String addUser(){
        String sql = "insert into mybatis.user(id,name,pwd) values (4,'小米','123456')";
        jdbcTemplate.update(sql);
        //事务会自动提交
        return "update-ok";
    }

    //修改
    @GetMapping("/updateUser/{id}")
    public String updateUser(@PathVariable("id") int id){
        String sql = "update mybatis.user set name=?, pwd=? where id="+id;

        //封装
        Object[] objects = new Object[2];
        objects[0] = "小米2";
        objects[1] = "xxxxx";
        jdbcTemplate.update(sql,objects);
        return "updateUser-ok";
    }

    //删除
    @GetMapping("/deleteUser/{id}")
    public String deleteUser(@PathVariable("id") int id){
        String sql = "delete from mybatis.user where id =?";
        jdbcTemplate.update(sql,id);
        //事务会自动提交
        return "deleteUser-ok";
    }

}

```

# DRUID

* 依赖

```xml
<!-- https://mvnrepository.com/artifact/com.alibaba/druid -->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.1.22</version>
</dependency>

```

* 配置数据源

```yaml
spring:
  datasource:
    username: root
    password: 123456
    url: jdbc:mysql://localhost:3306/mybatis?serverTimezone=UTC&useUncode=true&characterEncoding=utf-8
    driver-class-name: com.mysql.cj.jdbc.Driver
    type: com.alibaba.druid.pool.DruidDataSource
```

* 运行测试类查看数据源是否变化

* 基础配置

```yaml
spring:
  datasource:
    username: root
    password: 123456
    url: jdbc:mysql://localhost:3306/mybatis?serverTimezone=UTC&useUncode=true&characterEncoding=utf-8
    driver-class-name: com.mysql.cj.jdbc.Driver
    type: com.alibaba.druid.pool.DruidDataSource

    #    SpringBoot默认是不注入这些属性值的
    #    druid 数据源专有配置
    initialSize: 5
    minIdle: 5
    maxActive: 20
    maxWait: 20
    timeBetweenEvictionRunsMillis: 60000
    minEvictableIdleTimeMillis: 300000
    validationQuery: SELECT 1 FROM DUAL
    testWhileIdle: true
    testOnBorrow: false
    testOnReturn: false
    poolPreparedStatements: true
  
    #  配置监控统计拦截的filters, stat:监控统计, log4j: 日志记录, wall 防止sql注入
    #  如果运行报错 java.lang.ClassNotFoundException: org.apache.log4j.Priority
    #  则导入log4j依赖即可
    filters: stat,wall,log4j
    maxpoolPreparedStatmentPerConnectionSize: 20
    useGlobalDataSourceStat: true
    connectionProperties: 			   			    druid.stat,mergSql=true;druid.stat.slowSqlMillis=500
```

```xml
<!--        log4j-->
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.17</version>
        </dependency>
```

* 配置后台监控

```java
package com.cyz.config;

import com.alibaba.druid.pool.DruidDataSource;
import com.alibaba.druid.support.http.StatViewServlet;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.boot.web.servlet.ServletRegistrationBean;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import javax.servlet.Servlet;
import javax.servlet.ServletRegistration;
import javax.sql.DataSource;
import java.util.HashMap;

@Configuration
public class DruidConfig {

//    绑定配置属性

    @ConfigurationProperties(prefix = "spring.datasource")
    @Bean
    public DataSource druidDataSource(){
        return new DruidDataSource();
    }

    //后台监控 web.xml ServletRegistrationBean
    //因为SpringBoot 内置了 servlet容器 所以没有web.xml
    //
    @Bean
    public ServletRegistrationBean statViewServlet(){
        //获取后台监控
        ServletRegistrationBean<StatViewServlet> bean = new ServletRegistrationBean<>(new StatViewServlet(),"/druid/*");

        //后台需要登录
        HashMap<String, String> initParmeters = new HashMap<>();
        //增加配置 loginUsername loginPassword 固定参数
        initParmeters.put("loginUsername","admin");
        initParmeters.put("loginPassword","123456");

        //允许访问
        initParmeters.put("allow","");//所有
        //禁止访问
//        initParmeters.put("chen","192.168.5.5");

        bean.setInitParameters(initParmeters);
        return bean;
    }

}
```

* 访问<http://localhost:8080/druid>

* 过滤器

```java
//filter
    @Bean
    public FilterRegistrationBean webStatFilter(){
        FilterRegistrationBean bean = new FilterRegistrationBean();
        bean.setFilter(new WebStatFilter());

        //可以过滤那些请求
        HashMap<String, String> initParameters = new HashMap<>();
        //不进行过滤
        initParameters.put("exclusions","*.js,*.css,/druid/*");

        bean.setInitParameters(initParameters);
        return bean;
    }
```

# 整合MyBatis

* 包

```xml
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>2.1.2</version>
        </dependency>
```

* 依赖

```xml
 <dependencies>
<!--        mybatis-->
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>2.1.2</version>
        </dependency>
  		<!--lombok-->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jdbc</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <scope>runtime</scope>
        </dependency>
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

* 数据源

```properties
spring.datasource.username=root
spring.datasource.password=123456
spring.datasource.url=jdbc:mysql://localhost:3306/mybatis?serverTimezone=UTC&useUnicode=true&characterEncoding=utf-8
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
```

* pojo

```java
package com.cyz.pojo;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@AllArgsConstructor
@NoArgsConstructor
public class User {
    private int id;
    private String name;
    private String pwd;
}

```

* Mapper接口

```java
package com.cyz.mapper;

import com.cyz.pojo.User;
import org.apache.ibatis.annotations.Mapper;
import org.springframework.stereotype.Repository;

import java.util.List;

//@Mapper 表示这是一个 mybatis 的 mapper类
    //相当于不用在启动类上加扫描的注解了@MapperScan
@Mapper
@Repository
public interface UserMapper {
    List<User> queryUserList();

    User queryUserById(int id);

    int addUser(User user);

    int updateUser(User user);

    int deleteUser(int id);
}

```

* 写mapper.xml

```xml

    <insert id="addUser" parameterType="User">
        insert into mybatis.user (id, name, pwd)
        values (#{id},#{name},#{pwd})
    </insert>
    <update id="updateUser" parameterType="int">
        update mybatis.user set name = #{name},pwd = #{pwd}
        where id = #{id}
    </update>
    <delete id="deleteUser" parameterType="int">
        delete from mybatis.user
        where id = #{id}
    </delete>
    <select id="queryUserList" resultType="User">
        select * from mybatis.user
    </select>
    <select id="queryUserById" parameterType="int" resultType="User">
        select * from mybatis.user
        where id = #{id}
    </select>
```

* 配置别名和注册mapper

```properties
spring.datasource.username=root
spring.datasource.password=123456
spring.datasource.url=jdbc:mysql://localhost:3306/mybatis?serverTimezone=UTC&useUnicode=true&characterEncoding=utf-8
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver

# 整合mybatis
mybatis.type-aliases-package=com.cyz.pojo
mybatis.mapper-locations=classpath:mybatis/mapper/*.xml
```

* 省去service层
* controller

```java
package com.cyz.controller;

import com.cyz.mapper.UserMapper;
import com.cyz.pojo.User;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
public class Controller {

    @Autowired
    private UserMapper userMapper;

    @GetMapping("/queryUserList")
    public List<User> queryUserList() {
        List<User> userList = userMapper.queryUserList();
        return userList;
    }

    @GetMapping("queryUserById/{id}")
    public User queryUserById(@PathVariable("id") int id) {
        User user = userMapper.queryUserById(id);
        return user;
    }

    @GetMapping("/addUser")
    public int addUser(User user) {
        //模拟
        user.setId(4);
        user.setName("ces");
        user.setPwd("12135");
        int row = userMapper.addUser(user);
        return row;
    }

    @GetMapping("/updateUser")
    public int updateUser(User user){
        //模拟
        user.setId(4);
        user.setName("学习");
        user.setPwd("45621s");
        int row = userMapper.updateUser(user);
        return row;
    }

    @GetMapping("/deleteUser/{id}")
    public int deleteUser(@PathVariable("id") int id){
        int row = userMapper.deleteUser(id);
        return row;
    }
}
```

* 可以去整合员工管理系统

# SpringSecurity

* 安全
* 认证和授权

## 环境搭建

* 素材, 测试
* 导入依赖

```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

### 登录认证注销

* controller

```java
@Controller
public class RouterController {

    @RequestMapping({"/","/index"})
    public String index(){
        return "index";
    }

    @RequestMapping("/toLogin")
    public String toLogin(){
        return "views/login";
    }

    @RequestMapping("/level1/{id}")
    public String level1(@PathVariable("id") int id){
        return "views/level1/"+id;
    }

    @RequestMapping("/level2/{id}")
    public String level2(@PathVariable("id") int id){
        return "views/level2/"+id;
    }

    @RequestMapping("/level3/{id}")
    public String level3(@PathVariable("id") int id){
        return "views/level3/"+id;
    }

}
```

* 配置认证和权限

```java
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    //http安全策略
    //授权
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        //首页所有页, 功能也需要权限
        //请求授权的规则
        http.authorizeRequests()
                .antMatchers("/").permitAll()
                .antMatchers("/level1/**").hasRole("vip1")
                .antMatchers("/level2/**").hasRole("vip2")
                .antMatchers("/level3/**").hasRole("vip3");

        //没有权限  跳转到登录页  /login 如果错误  认证失败走/login?error
        http.formLogin();

        //注销  /logout   成功跳转到/login?success //看原码
//        http.logout();
        //修改跳转首页 //清空session
        http.logout()
//        .deleteCookies("remove").invalidateHttpSession(true)
        .logoutSuccessUrl("/");
    }

    //认证
    //密码需要加密
    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {

        //正常情况从数据库读取
//        auth.jdbcAuthentication()

        //从内存中读取
        auth.inMemoryAuthentication().passwordEncoder(new BCryptPasswordEncoder())
                .withUser("cyz").password(new BCryptPasswordEncoder().encode("123456")).roles("vip2","vip3")
                .and().withUser("chen").password(new BCryptPasswordEncoder().encode("111111")).roles("vip1","vip2","vip3")
                .and().withUser("jack").password(new BCryptPasswordEncoder().encode("222222")).roles("vip1");
    }
}


```

```html
 			<!--注销-->
                <a class="item" th:href="@{/logout}">
                    <i class="sign-out icon"></i> 注销
                </a>
```

### 权限控制

* 导包

```xml
<dependency>
            <groupId>org.thymeleaf.extras</groupId>
            <artifactId>thymeleaf-extras-springsecurity5</artifactId>
            <version>3.0.4.RELEASE</version>
        </dependency>
```

* 导入

```html
xmlns:sec="http://www.thymeleaf.org/thymeleaf-extras-springsecurity4"
```

* 权限控制

```java
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    //http安全策略
    //授权
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        //首页所有页, 功能也需要权限
        //请求授权的规则
        http.authorizeRequests()
                .antMatchers("/").permitAll()
                .antMatchers("/level1/**").hasRole("vip1")
                .antMatchers("/level2/**").hasRole("vip2")
                .antMatchers("/level3/**").hasRole("vip3");

        //没有权限  跳转到登录页  /login 如果错误  认证失败走/login?error
        http.formLogin();

        //注销  /logout   成功跳转到/login?success //看原码
//        http.logout();
        //修改跳转首页 //清空session
        http.logout()
//        .deleteCookies("remove").invalidateHttpSession(true)
        .logoutSuccessUrl("/");

        //安全问题
//        http.csrf().disable();//关闭csrf功能  因为我们的是get不安全按

    }

    //认证
    //密码需要加密
    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {

        //正常情况从数据库读取
//        auth.jdbcAuthentication()

        //从内存中读取
        auth.inMemoryAuthentication().passwordEncoder(new BCryptPasswordEncoder())
                .withUser("cyz").password(new BCryptPasswordEncoder().encode("123456")).roles("vip2","vip3")
                .and().withUser("chen").password(new BCryptPasswordEncoder().encode("111111")).roles("vip1","vip2","vip3")
                .and().withUser("jack").password(new BCryptPasswordEncoder().encode("222222")).roles("vip1");
    }
}
```

```html
   <!--登录注销-->
            <div class="right menu">
                <div sec:authorize="!isAuthenticated()">
                    <!--未登录-->
                    <a class="item" th:href="@{/toLogin}">
                        <i class="address card icon"></i> 登录
                    </a>
                </div>
                <div sec:authorize="isAuthenticated()">
                    <!--已登录-->
                  <a class="item">
                      用户名: <span sec:authentication="name"></span>
                      角色: <span sec:authentication="principal.authorities"></span>
                  </a>
                    <!--注销-->
                    <a class="item" th:href="@{/logout}">
                        <i class="sign-out icon"></i> 注销
                    </a>
                </div>
            </div>

   <!--不同权限显示不同模块-->
  <div class="column" sec:authorize="hasRole('vip1')">
                <div class="ui raised segment">
                    <div class="ui">
                        <div class="content">
                            <h5 class="content">Level 1</h5>
                            <hr>
                            <div><a th:href="@{/level1/1}"><i class="bullhorn icon"></i> Level-1-1</a></div>
                            <div><a th:href="@{/level1/2}"><i class="bullhorn icon"></i> Level-1-2</a></div>
                            <div><a th:href="@{/level1/3}"><i class="bullhorn icon"></i> Level-1-3</a></div>
                        </div>
                    </div>
                </div>
            </div>
```

### 记住我功能

```java
  //开启记住我功能  cookie 2周
        http.rememberMe();
```

```html
<div class="field">
	<input type="checkbox" name="remember"> 记住我
</div>
```

```java
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    //http安全策略
    //授权
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        //首页所有页, 功能也需要权限
        //请求授权的规则
        http.authorizeRequests()
                .antMatchers("/").permitAll()
                .antMatchers("/level1/**").hasRole("vip1")
                .antMatchers("/level2/**").hasRole("vip2")
                .antMatchers("/level3/**").hasRole("vip3");

        //没有权限  跳转到登录页  /login 如果错误  认证失败走/login?error
//        http.formLogin();
        //定制登录页  注意前端必须传入的是username和password 只识别这个,自定义
        http.formLogin().loginPage("/toLogin").usernameParameter("user").passwordParameter("pwd").loginProcessingUrl("/login");

        //注销  /logout   成功跳转到/login?success //看原码
//        http.logout();
        //修改跳转首页 //清空session
        http.logout().logoutSuccessUrl("/");
//        .deleteCookies("remove").invalidateHttpSession(true)


        //安全问题
//        http.csrf().disable();//关闭csrf功能  因为我们的是get不安全按

        //开启记住我功能  cookie 2周  自定义接收
        http.rememberMe().rememberMeParameter("remember");


    }

    //认证
    //密码需要加密
    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {

        //正常情况从数据库读取
//        auth.jdbcAuthentication()

        //从内存中读取
        auth.inMemoryAuthentication().passwordEncoder(new BCryptPasswordEncoder())
                .withUser("cyz").password(new BCryptPasswordEncoder().encode("123456")).roles("vip2","vip3")
                .and().withUser("chen").password(new BCryptPasswordEncoder().encode("111111")).roles("vip1","vip2","vip3")
                .and().withUser("jack").password(new BCryptPasswordEncoder().encode("222222")).roles("vip1");
    }
}


```

# Shiro

* apache的  java安全权限框架
* 认证, 授权, 加密, 会话管理, Web集成, 缓存

#### 依赖

```xml
 <dependencies>
        <dependency>
            <groupId>org.apache.shiro</groupId>
            <artifactId>shiro-core</artifactId>
            <version>1.4.1</version>
        </dependency>
        <!-- Shiro uses SLF4J for logging.  We'll use the 'simple' binding
             in this example app.  See http://www.slf4j.org for more info. -->
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-simple</artifactId>
            <version>1.7.21</version>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>jcl-over-slf4j</artifactId>
            <version>1.7.21</version>
        </dependency>


        <!-- https://mvnrepository.com/artifact/log4j/log4j -->
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <scope>runtime</scope>
            <version>1.2.17</version>
        </dependency>
    </dependencies>
```

```ini
[users]
# user 'root' with password 'secret' and the 'admin' role
root = secret, admin
# user 'guest' with the password 'guest' and the 'guest' role
guest = guest, guest
# user 'presidentskroob' with password '12345' ("That's the same combination on
# my luggage!!!" ;)), and role 'president'
presidentskroob = 12345, president
# user 'darkhelmet' with password 'ludicrousspeed' and roles 'darklord' and 'schwartz'
darkhelmet = ludicrousspeed, darklord, schwartz
# user 'lonestarr' with password 'vespa' and roles 'goodguy' and 'schwartz'
lonestarr = vespa, goodguy, 

# -----------------------------------------------------------------------------
# Roles with assigned permissions
#
# Each line conforms to the format defined in the
# org.apache.shiro.realm.text.TextConfigurationRealm#setRoleDefinitions JavaDoc
# -----------------------------------------------------------------------------
[roles]
# 'admin' role has all permissions, indicated by the wildcard '*'
admin = *
# The 'schwartz' role can do anything (*) with any lightsaber:
schwartz = lightsaber:*
# The 'goodguy' role is allowed to 'drive' (action) the winnebago (type) with
# license plate 'eagle5' (instance specific id)
goodguy = winnebago:drive:eagle5
```

```java

import org.apache.shiro.SecurityUtils;
import org.apache.shiro.authc.*;
import org.apache.shiro.config.IniSecurityManagerFactory;
//import org.apache.shiro.ini.IniSecurityManagerFactory;
import org.apache.shiro.mgt.SecurityManager;
import org.apache.shiro.session.Session;
import org.apache.shiro.subject.Subject;
//import org.apache.shiro.lang.util.Factory;
import org.apache.shiro.util.Factory;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class Quickstart {

    private static final transient Logger log = LoggerFactory.getLogger(Quickstart.class);


    public static void main(String[] args) {

        Factory<SecurityManager> factory = new IniSecurityManagerFactory("classpath:shiro.ini");
        SecurityManager securityManager = factory.getInstance();
        SecurityUtils.setSecurityManager(securityManager);

        // 获取当前的用户对象 Subject
        Subject currentUser = SecurityUtils.getSubject();

        // 通过当前用户拿到session
        Session session = currentUser.getSession();
        session.setAttribute("someKey", "aValue");
        String value = (String) session.getAttribute("someKey");
        if (value.equals("aValue")) {
            log.info("Subject的session [" + value + "]");
        }

        // 判断当前用户是否认证
        if (!currentUser.isAuthenticated()) {
            //令牌
            UsernamePasswordToken token = new UsernamePasswordToken("lonestarr", "vespa");
            //记住我
            token.setRememberMe(true);

            try {
                currentUser.login(token);//登录操作
            } catch (UnknownAccountException uae) {//用户名
                log.info("There is no user with username of " + token.getPrincipal());
            } catch (IncorrectCredentialsException ice) {//密码
                log.info("Password for account " + token.getPrincipal() + " was incorrect!");
            } catch (LockedAccountException lae) {//用户锁定
                log.info("The account for username " + token.getPrincipal() + " is locked.  " +
                        "Please contact your administrator to unlock it.");
            }
            // ... catch more exceptions here (maybe custom ones specific to your application?
            catch (AuthenticationException ae) {
                //unexpected condition?  error?
            }
        }

        //say who they are:当前用户
        //print their identifying principal (in this case, a username):
        log.info("User [" + currentUser.getPrincipal() + "] logged in successfully.");
        
   		//拥有角色
        //test a role:
        if (currentUser.hasRole("schwartz")) {
            log.info("May the Schwartz be with you!");
        } else {
            log.info("Hello, mere mortal.");
        }

        //简单 粗力度
        //test a typed permission (not instance-level)
        if (currentUser.isPermitted("lightsaber:wield")) {
            log.info("You may use a lightsaber ring.  Use it wisely.");
        } else {
            log.info("Sorry, lightsaber rings are for schwartz masters only.");
        }

        //更细力度
        //a (very powerful) Instance Level permission:
        if (currentUser.isPermitted("winnebago:drive:eagle5")) {
            log.info("You are permitted to 'drive' the winnebago with license plate (id) 'eagle5'.  " +
                    "Here are the keys - have fun!");
        } else {
            log.info("Sorry, you aren't allowed to drive the 'eagle5' winnebago!");
        }

        //注销
        //all done - log out!
        currentUser.logout();

        //结束
        System.exit(0);
    }
}
```

# SpringBoot集成Shiro

```
Subject             用户
SecurityManager		管理用户
Realm				获取数据
```

## 环境搭建

* 新建模板
* 导入环境

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
<!--        整合shiro-->
        <dependency>
            <groupId>org.apache.shiro</groupId>
            <artifactId>shiro-spring</artifactId>
            <version>1.5.3</version>
        </dependency>

```

* 自定义UserRealm

```java
package com.cyz.config;

import org.apache.shiro.authc.AuthenticationException;
import org.apache.shiro.authc.AuthenticationInfo;
import org.apache.shiro.authc.AuthenticationToken;
import org.apache.shiro.authz.AuthorizationInfo;
import org.apache.shiro.realm.AuthorizingRealm;
import org.apache.shiro.subject.PrincipalCollection;

import javax.security.sasl.AuthorizeCallback;

//自定义的UserRealm 继承AuthorizingRealm
public class UserRealm extends AuthorizingRealm {
    //授权
    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principalCollection) {
        System.out.println("执行了=>授权doGetAuthorizationInfo");
        return null;
    }

    //认证
    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken authenticationToken) throws AuthenticationException {
        System.out.println("执行了=>doGetAuthenticationInfo");
        return null;
    }
}

```

* 编写配置类

```java
package com.cyz.config;

import org.apache.catalina.User;
import org.apache.shiro.spring.web.ShiroFilterFactoryBean;
import org.apache.shiro.web.mgt.DefaultWebSecurityManager;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class ShiroConfig {

    //ShiroFilterFactoryBean
    @Bean
    public ShiroFilterFactoryBean getShiroFilterFactoryBean(@Qualifier("securityManager") DefaultWebSecurityManager securityManager){
        ShiroFilterFactoryBean bean = new ShiroFilterFactoryBean();
        //设置安全管理器
        bean.setSecurityManager(securityManager);
        return bean;
    }

    //DefaultWebSecurityManager
    @Bean(name = "securityManager")
    public DefaultWebSecurityManager getDefaultWebSecurityManager(@Qualifier("userRealm") UserRealm userRealm){
        DefaultWebSecurityManager securityManager = new DefaultWebSecurityManager();
        //关联UserRealm
        securityManager.setRealm(userRealm);
        return securityManager;
    }

    //创建realm对象 需要自定义类
    @Bean
    public UserRealm userRealm(){
        return new UserRealm();
    }

}
```

* 分别有三个页面index,add,update

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>首页</h1>
<p th:text="${msg}"></p>
<a th:href="@{/user/add}">add</a>|
<a th:href="@{/user/update}">update</a>
</body>
</html>


<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>add</h1>
</body>
</html>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>update</h1>
</body>
</html>

```

* 测试

```java
package com.cyz.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class MyController {

    @RequestMapping({"/","/index"})
    public String index(Model model){
        model.addAttribute("msg","helle");
        return "index";
    }

    @RequestMapping("/user/add")
    public String add(){
        return "/user/add";
    }

    @RequestMapping("/user/update")
    public String update(){
        return "/user/update";
    }
}

```

## 登录拦截

* controller

```java
  @RequestMapping("/toLogin")
    public String toLogin(){
        return "login";
    }
```

* 配置

```java
@Configuration
public class ShiroConfig {
    //ShiroFilterFactoryBean
    @Bean
    public ShiroFilterFactoryBean getShiroFilterFactoryBean(@Qualifier("securityManager") DefaultWebSecurityManager securityManager){
        ShiroFilterFactoryBean bean = new ShiroFilterFactoryBean();
        //设置安全管理器
        bean.setSecurityManager(securityManager);


        //添加shiro的内置过滤器
        /*
            anon: 无需认证就可以访问
            authc: 必须认证了才能访问
            user: 必须拥有 记住我 功能才能访问
            perms: 拥有某个资源的权限才能访问
            role: 拥有某个角色权限才能访问
         */
        LinkedHashMap<String, String> filterChainDefinitionMap = new LinkedHashMap<>();

        filterChainDefinitionMap.put("/user/add","authc");
        filterChainDefinitionMap.put("/user/update","authc");
//        filterChainDefinitionMap.put("/user/*","authc");

        bean.setFilterChainDefinitionMap(filterChainDefinitionMap);

        //设置登录页面请求
        bean.setLoginUrl("/toLogin");
        return bean;
    }
...
}
```

## 认证

```java
package com.cyz.config;

import org.apache.shiro.SecurityUtils;
import org.apache.shiro.authc.*;
import org.apache.shiro.authz.AuthorizationInfo;
import org.apache.shiro.authz.SimpleAuthorizationInfo;
import org.apache.shiro.realm.AuthorizingRealm;
import org.apache.shiro.subject.PrincipalCollection;
import org.apache.shiro.subject.Subject;

import javax.security.sasl.AuthorizeCallback;

//自定义的UserRealm 继承AuthorizingRealm
public class UserRealm extends AuthorizingRealm {
    //授权
    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principalCollection) {
        System.out.println("执行了=>授权doGetAuthorizationInfo");
        return null;
    }

    //认证
    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken authenticationToken) throws AuthenticationException {
        System.out.println("执行了=>doGetAuthenticationInfo");

        //用不好吗 密码  数据库取
        String name = "root";
        String password = "123456";

        UsernamePasswordToken userToken = (UsernamePasswordToken) authenticationToken;

        if (!userToken.getUsername().equals(name)){
            return null;//抛出异常 UnknownAccountException 自动抛出
        }

        //密码认证 shiro做
        return new SimpleAuthenticationInfo("", password, "");
    }
}

```

* controller

```java
 @RequestMapping("/login")
    public String login(String username,String password,Model model){
        //获取当前的用户
        Subject subject = SecurityUtils.getSubject();

        //封装用户的登录数据
        UsernamePasswordToken token = new UsernamePasswordToken(username,password);

        try {
            subject.login(token);//执行登录的方法
            return "index";
        } catch (UnknownAccountException e) {//用户名不存在
            model.addAttribute("msg","用户名错误");
            return "login";
        } catch (IncorrectCredentialsException e) {//密码错误
            model.addAttribute("msg","密码错误");
            return "login";
        }
    }
```

* login

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>登录</h1>
<hr>
<p th:text="${msg}" style="color: red;"></p>
<form th:action="@{/login}">
    <p>用户名: <input type="text" name="username"></p>
    <p>密码: <input type="password" name="password"></p>
    <p><input type="submit" value="登录"></p>
</form>
</body>
</html>
```

## 连接数据库

* 依赖

```xml
<!--        lombok-->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>

<!--        mysql-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>

<!--        log4j-->
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.17</version>
        </dependency>

<!--        druid-->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.1.22</version>
        </dependency>

<!--        mybatis-->
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>2.1.2</version>
        </dependency>

```

* 配置application.yml

```yaml
spring:
  datasource:
    username: root
    password: 123456
    url: jdbc:mysql://localhost:3306/mybatis?serverTimezone=UTC&useUncode=true&characterEncoding=utf-8
    driver-class-name: com.mysql.cj.jdbc.Driver
    type: com.alibaba.druid.pool.DruidDataSource

    #    SpringBoot默认是不注入这些属性值的
    #    druid 数据源专有配置
    initialSize: 5
    minIdle: 5
    maxActive: 20
    maxWait: 20
    timeBetweenEvictionRunsMillis: 60000
    minEvictableIdleTimeMillis: 300000
    validationQuery: SELECT 1 FROM DUAL
    testWhileIdle: true
    testOnBorrow: false
    testOnReturn: false
    poolPreparedStatements: true

    #  配置监控统计拦截的filters, stat:监控统计, log4j: 日志记录, wall 防止sql注入
    #  如果运行报错 java.lang.ClassNotFoundException: org.apache.log4j.Priority
    #  则导入log4j依赖即可
    filters: stat,wall,log4j
    maxpoolPreparedStatmentPerConnectionSize: 20
    useGlobalDataSourceStat: true
    connectionProperties: 			    druid.stat,mergSql=true;druid.stat.slowSqlMillis=500
```

* application.properties

```properties
spring.thymeleaf.cache=false

mybatis.type-aliases-package=com.cyz.pojo
mybatis.mapper-locations=classpath:mapper/*.xml
```

* pojo

```java
package com.cyz.pojo;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@AllArgsConstructor
@NoArgsConstructor
public class User {
    private int id;
    private String name;
    private String pwd;
}

```

* Mapper接口

```java
package com.cyz.mapper;

import com.cyz.pojo.User;
import org.apache.ibatis.annotations.Mapper;
import org.springframework.stereotype.Repository;

@Mapper
@Repository
public interface UserMapper {

    public User queryUserByName(String name);
}

```

* mapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.cyz.mapper.UserMapper">
    <select id="queryUserByName" parameterType="String" resultType="User">
         select * from mybatis.user
         where name = #{name}
    </select>
</mapper>
```

* service接口	

```java
package com.cyz.service;

import com.cyz.pojo.User;

public interface UserService {
    public User queryUserByName(String name);
}

```

* service实现类

```java
package com.cyz.service;

import com.cyz.mapper.UserMapper;
import com.cyz.pojo.User;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class UserServiceImpl implements UserService {

    @Autowired
    UserMapper userMapper;

    @Override
    public User queryUserByName(String name) {
        return userMapper.queryUserByName(name);
    }
}

```

* 测试

```jav
package com.cyz;

import com.cyz.pojo.User;
import com.cyz.service.UserService;
import com.cyz.service.UserServiceImpl;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

@SpringBootTest
class ShiroSpringbootApplicationTests {

    @Autowired
    UserServiceImpl userService;

    @Test
    void contextLoads() {
        User user = userService.queryUserByName("Hello");
        System.out.println(user);
    }

}

```

* UserRealm

```java
package com.cyz.config;

import com.cyz.pojo.User;
import com.cyz.service.UserService;
import org.apache.shiro.SecurityUtils;
import org.apache.shiro.authc.*;
import org.apache.shiro.authz.AuthorizationInfo;
import org.apache.shiro.authz.SimpleAuthorizationInfo;
import org.apache.shiro.realm.AuthorizingRealm;
import org.apache.shiro.subject.PrincipalCollection;
import org.apache.shiro.subject.Subject;
import org.springframework.beans.factory.annotation.Autowired;

import javax.security.sasl.AuthorizeCallback;

//自定义的UserRealm 继承AuthorizingRealm
public class UserRealm extends AuthorizingRealm {

    @Autowired
    UserService userService;

    //授权
    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principalCollection) {
        System.out.println("执行了=>授权doGetAuthorizationInfo");
        return null;
    }

    //认证
    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken authenticationToken) throws AuthenticationException {
        System.out.println("执行了=>doGetAuthenticationInfo");

        UsernamePasswordToken userToken = (UsernamePasswordToken) authenticationToken;
 
        //连接真实数据库
        User user = userService.queryUserByName(userToken.getUsername());

        if (user == null){
            return null;//UnknownAccountException
        }

        //密码认证 shiro做  加密 MD5 MD5盐值加密
        return new SimpleAuthenticationInfo("", user.getPwd(), "");
    }
}

```

* 加密   数据库存储加密后的密码

```java
package com.cyz.config;

import org.apache.shiro.authc.credential.HashedCredentialsMatcher;
import org.apache.shiro.spring.web.ShiroFilterFactoryBean;
import org.apache.shiro.web.mgt.DefaultWebSecurityManager;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import java.util.LinkedHashMap;

@Configuration
public class ShiroConfig {

    @Bean(name = "hashedCredentialsMatcher")
    public HashedCredentialsMatcher hashedCredentialsMatcher() {
        HashedCredentialsMatcher credentialsMatcher = new HashedCredentialsMatcher();
        //指定加密方式为MD5
        credentialsMatcher.setHashAlgorithmName("MD5");
        //加密次数
        credentialsMatcher.setHashIterations(1024);
        credentialsMatcher.setStoredCredentialsHexEncoded(true);
        return credentialsMatcher;
    }

    //ShiroFilterFactoryBean
    @Bean
    public ShiroFilterFactoryBean getShiroFilterFactoryBean(@Qualifier("securityManager") DefaultWebSecurityManager securityManager){
        ShiroFilterFactoryBean bean = new ShiroFilterFactoryBean();
        //设置安全管理器
        bean.setSecurityManager(securityManager);


        //添加shiro的内置过滤器
        /*
            anon: 无需认证就可以访问
            authc: 必须认证了才能访问
            user: 必须拥有 记住我 功能才能访问
            perms: 拥有某个资源的权限才能访问
            role: 拥有某个角色权限才能访问
         */
        LinkedHashMap<String, String> filterChainDefinitionMap = new LinkedHashMap<>();

        filterChainDefinitionMap.put("/user/add","authc");
        filterChainDefinitionMap.put("/user/update","authc");
//        filterChainDefinitionMap.put("/user/*","authc");

        bean.setFilterChainDefinitionMap(filterChainDefinitionMap);

        //设置登录页面请求
        bean.setLoginUrl("/toLogin");
        return bean;
    }

    //DefaultWebSecurityManager
    @Bean(name = "securityManager")
    public DefaultWebSecurityManager getDefaultWebSecurityManager(@Qualifier("userRealm") UserRealm userRealm){
        DefaultWebSecurityManager securityManager = new DefaultWebSecurityManager();
        //关联UserRealm
        securityManager.setRealm(userRealm);
        return securityManager;
    }

    //创建realm对象 需要自定义类
    @Bean
    public UserRealm userRealm(@Qualifier("hashedCredentialsMatcher") HashedCredentialsMatcher hashedCredentialsMatcher){
        return new UserRealm();
    }

}

```

* 工具类

```java
package com.cyz.util;

import org.apache.shiro.crypto.hash.SimpleHash;

public class ShiroUtil {

    /**
     * 加盐加密
     * @param srcPwd    原始密码
     * @param saltValue 盐值
     */
    public static String salt(Object srcPwd, String saltValue){
        return new SimpleHash("MD5", srcPwd, saltValue, 1024).toString();
    }

}
```

* UserRealm

```java
package com.cyz.config;

import com.cyz.pojo.User;
import com.cyz.service.UserService;
import com.cyz.util.ShiroUtil;
import org.apache.shiro.SecurityUtils;
import org.apache.shiro.authc.*;
import org.apache.shiro.authz.AuthorizationInfo;
import org.apache.shiro.authz.SimpleAuthorizationInfo;
import org.apache.shiro.realm.AuthorizingRealm;
import org.apache.shiro.subject.PrincipalCollection;
import org.apache.shiro.subject.Subject;
import org.apache.shiro.util.ByteSource;
import org.springframework.beans.factory.annotation.Autowired;

import javax.security.sasl.AuthorizeCallback;

//自定义的UserRealm 继承AuthorizingRealm
public class UserRealm extends AuthorizingRealm {

    @Autowired
    UserService userService;

    //授权
    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principalCollection) {
        System.out.println("执行了=>授权doGetAuthorizationInfo");
        return null;
    }

    //认证
    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken authenticationToken) throws AuthenticationException {
        System.out.println("执行了=>doGetAuthenticationInfo");

        UsernamePasswordToken userToken = (UsernamePasswordToken) authenticationToken;

        //连接真实数据库
        User user = userService.queryUserByName(userToken.getUsername());

        if (user == null){
            return null;//UnknownAccountException
        }

        //盐值
        ByteSource salt = ByteSource.Util.bytes(userToken.getUsername());
        String saltPassword = ShiroUtil.salt(userToken.getPassword(), salt.toString());

        if (!user.getPwd().equals(saltPassword)){
            return null;
        }



        //密码认证 shiro做  加密 MD5 MD5盐值加密
        return new SimpleAuthenticationInfo("", userToken.getPassword(),salt, "");
    }
}

```

## 授权及注销

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org"
      xmlns:shiro="http://www.thymeleaf.org/thymeleaf-extras-shiro">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>首页</h1>
<p th:text="${msg}"></p>
<a th:href="@{/user/add}">add</a>|
<a th:href="@{/user/update}">update</a>
<p th:if="${session.loginUser!=null}">
    <span th:text="${(session.loginUser).getName()}"></span>
    <a th:href="@{/logout}">logout</a>
</p>
<p th:if="${session.loginUser==null}">
    <a th:href="@{/toLogin}">login</a>
</p>

</body>
</html>
```

* controller

```java
   @RequestMapping("/login")
    public String login(String username,String password,Model model){
        //获取当前的用户
        Subject subject = SecurityUtils.getSubject();

        //封装用户的登录数据
        UsernamePasswordToken token = new UsernamePasswordToken(username,password);

        try {
            subject.login(token);//执行登录的方法
            return "index";
        } catch (UnknownAccountException e) {//用户名不存在
            model.addAttribute("msg","用户名错误");
            return "login";
        } catch (IncorrectCredentialsException e) {//密码错误
            model.addAttribute("msg","密码错误");
            return "login";
        }
    }

	@RequestMapping("/unauthorized")
    @ResponseBody
    public String unauthorized(){
        return "您的权限不够";
    }

    @RequestMapping("/logout")
    public String logout(){
        //获取当前的用户
        Subject subject = SecurityUtils.getSubject();
        subject.logout();
        return "index";
    }
```

* 数据库增加权限字段

```sql
7	root	570364ca222663f3482fcf90054009ef	user:update
```

* pojo

```java
package com.cyz.pojo;

import com.cyz.util.ShiroUtil;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;
import org.apache.shiro.util.ByteSource;

@Data
@AllArgsConstructor
@NoArgsConstructor
public class User {
    private int id;
    private String name;
    private String pwd;
    private String perms;
}
```

* UserRealm

```java
package com.cyz.config;

import com.cyz.pojo.User;
import com.cyz.service.UserService;
import com.cyz.util.ShiroUtil;
import org.apache.shiro.SecurityUtils;
import org.apache.shiro.authc.*;
import org.apache.shiro.authz.AuthorizationInfo;
import org.apache.shiro.authz.SimpleAuthorizationInfo;
import org.apache.shiro.realm.AuthorizingRealm;
import org.apache.shiro.subject.PrincipalCollection;
import org.apache.shiro.subject.Subject;
import org.apache.shiro.util.ByteSource;
import org.springframework.beans.factory.annotation.Autowired;

import javax.security.sasl.AuthorizeCallback;

//自定义的UserRealm 继承AuthorizingRealm
public class UserRealm extends AuthorizingRealm {

    @Autowired
    UserService userService;

    //授权
    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principalCollection) {
        System.out.println("执行了=>授权doGetAuthorizationInfo");

        //给用户授权
        SimpleAuthorizationInfo info = new SimpleAuthorizationInfo();
//        info.addStringPermission("user:add");

        //拿到当前对象   需要在认证中传递user
        Subject subject = SecurityUtils.getSubject();
        User currentUser = (User) subject.getPrincipal();

        info.addStringPermission(currentUser.getPerms());

        return info     ;
    }

    //认证
    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken authenticationToken) throws AuthenticationException {
        System.out.println("执行了=>doGetAuthenticationInfo");

        UsernamePasswordToken userToken = (UsernamePasswordToken) authenticationToken;

        //连接真实数据库
        User user = userService.queryUserByName(userToken.getUsername());

        if (user == null){
            return null;//UnknownAccountException
        }

        //盐值
        ByteSource salt = ByteSource.Util.bytes(userToken.getUsername());
        String saltPassword = ShiroUtil.salt(userToken.getPassword(), salt.toString());

        if (!user.getPwd().equals(saltPassword)){
            return null;
        }
        
         Subject currentSubject = SecurityUtils.getSubject();
        Session session = currentSubject.getSession();
        session.setAttribute("loginUser",user);

        //密码认证 shiro做  加密 MD5 MD5盐值加密  传递user
        return new SimpleAuthenticationInfo(user, userToken.getPassword(),salt, "");
    }
}

```

* 配置

```java
 //ShiroFilterFactoryBean
    @Bean
    public ShiroFilterFactoryBean getShiroFilterFactoryBean(@Qualifier("securityManager") DefaultWebSecurityManager securityManager){
        ShiroFilterFactoryBean bean = new ShiroFilterFactoryBean();
        //设置安全管理器
        bean.setSecurityManager(securityManager);


        //添加shiro的内置过滤器
        /*
            anon: 无需认证就可以访问
            authc: 必须认证了才能访问
            user: 必须拥有 记住我 功能才能访问
            perms: 拥有某个资源的权限才能访问
            role: 拥有某个角色权限才能访问
         */
        //拦截
        LinkedHashMap<String, String> filterChainDefinitionMap = new LinkedHashMap<>();


        //授权  跳转到未授权页面
        filterChainDefinitionMap.put("/user/add","perms[user:add]");
        filterChainDefinitionMap.put("/user/update","perms[user:update]");

//        filterChainDefinitionMap.put("/user/add","authc");
//        filterChainDefinitionMap.put("/user/update","authc");
        filterChainDefinitionMap.put("/user/*","authc");

        bean.setFilterChainDefinitionMap(filterChainDefinitionMap);

        //设置登录页面请求
        bean.setLoginUrl("/toLogin");
        //未授权页面
        bean.setUnauthorizedUrl("/unauthorized");

        return bean;
    }
```

## 整合thymeleaf

* 依赖

```xml
<dependency>
            <groupId>com.github.theborakompanioni</groupId>
            <artifactId>thymeleaf-extras-shiro</artifactId>
            <version>2.0.0</version>
        </dependency>
```

* 配置

```java
  //整合ShiroDialect 用来整合shiro 和 thymeleaf
    @Bean
    public ShiroDialect getShiroDialect(){
        return new ShiroDialect();
    }
```

* html
* 命名空间

```html
xmlns:shiro ="http://www.thymeleaf.org/thymeleaf-extras-shiro"
```

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org"
      xmlns:shiro="http://www.thymeleaf.org/thymeleaf-extras-shiro">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>首页</h1>
<p th:text="${msg}"></p>
<div shiro:hasPermission="user:add">
    <a th:href="@{/user/add}">add</a>|
</div>
<div shiro:hasPermission="user:update">
    <a th:href="@{/user/update}">update</a>
</div>
<p th:if="${session.loginUser!=null}">
    <span th:text="${(session.loginUser).getName()}"></span>
    <a th:href="@{/logout}">logout</a>
</p>
<p th:if="${session.loginUser==null}">
    <a th:href="@{/toLogin}">login</a>
</p>

</body>
</html>
```

# Swagger

* API框架
* RestFul Api文档在线自动生成工具=>**Api文档与Api定义同步更新**

* 直接运行, 在线测试接口

## 使用 需要springbox

* swagger2
* ui

## Springboot继承Swagger

* 新增项目
* 测试项目运行
* 依赖

```xml
<!-- https://mvnrepository.com/artifact/io.springfox/springfox-swagger-ui -->
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>2.9.2</version>
</dependency>
<!-- https://mvnrepository.com/artifact/io.springfox/springfox-swagger2 -->
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>2.9.2</version>
</dependency>

```

* 配置config

```java
package com.cyz.swagger.config;

import org.springframework.context.annotation.Configuration;
import springfox.documentation.swagger2.annotations.EnableSwagger2;

@Configuration
@EnableSwagger2 //开启Swagger2
public class SwggerConfig {
    
}

```

* 测试运行访问http://localhost:8080/swagger-ui.html

* 基础配置

```java
package com.cyz.swagger.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import springfox.documentation.service.ApiInfo;
import springfox.documentation.service.Contact;
import springfox.documentation.spi.DocumentationType;
import springfox.documentation.spring.web.plugins.Docket;
import springfox.documentation.swagger2.annotations.EnableSwagger2;

import java.util.ArrayList;

@Configuration
@EnableSwagger2 //开启Swagger2
public class SwggerConfig {

    //peizhi Swagger 的 Docket的bean实例
    @Bean
    public Docket docket(){
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo());
    }

    //配置Swagger信息=apiInfo
    private ApiInfo apiInfo(){
        //作者信息
        Contact contact = new Contact("chen", "https:chenyaoze666.github.io", "xxx@qq.com");
        return new ApiInfo("Chen的Swagger2",
                        "一步又一步",
                        "v1.0",
                        "https:chenyaoze666.github.io",
                        contact,
                        "Apache 2.0",
                        "http://www.apache.org/licenses/LICENSE-2.0",
                        new ArrayList<>()
                            );
    }
}

```

* 配置扫描接口和开关

* Docket.select()
* Docket.enable()

```java
package com.cyz.swagger.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.bind.annotation.GetMapping;
import springfox.documentation.builders.PathSelectors;
import springfox.documentation.builders.RequestHandlerSelectors;
import springfox.documentation.service.ApiInfo;
import springfox.documentation.service.Contact;
import springfox.documentation.spi.DocumentationType;
import springfox.documentation.spring.web.plugins.Docket;
import springfox.documentation.swagger2.annotations.EnableSwagger2;

import java.util.ArrayList;

@Configuration
@EnableSwagger2 //开启Swagger2
public class SwggerConfig {

    //peizhi Swagger 的 Docket的bean实例
    @Bean
    public Docket docket(){
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
//                false 关闭Swagger
//                .enable(false)
                .select()
//                RequestHandlerSelectors 配置要扫描接口的方式
                //basePackage 指定扫描的包
                //any   全部
                //none 都不扫描
                //withClassAnnotation 扫描类上的注解 反射
                //withMethodAnnotation 扫描方法上的注解 反射
//                .apis(RequestHandlerSelectors.withMethodAnnotation(GetMapping.class))
                .apis(RequestHandlerSelectors.basePackage("com.cyz.swagger.controller"))
//                过滤
                //ant指定路径
//                .paths(PathSelectors.ant("/cyz/**"))
                .build();
    }

    //配置Swagger信息=apiInfo
    private ApiInfo apiInfo(){
        //作者信息
        Contact contact = new Contact("chen", "https:chenyaoze666.github.io", "xxx@qq.com");
        return new ApiInfo("Chen的Swagger2",
                        "一步又一步",
                        "v1.0",
                        "https:chenyaoze666.github.io",
                        contact,
                        "Apache 2.0",
                        "http://www.apache.org/licenses/LICENSE-2.0",
                        new ArrayList<>()
                            );
    }
}

```

* 生产环境和发布环境
* 三个配置文件

```properties
server.port=808
#spring.profiles.active=dev
#spring.profiles.active=pro
```

```properties
server.port=8081
```

```properties
server.port=8082
```

* 配置

```java
    //peizhi Swagger 的 Docket的bean实例
    @Bean
    public Docket docket(Environment environment){

        //设置要显示的Swagger环境
        Profiles profiles = Profiles.of("dev","test");

        //获取项目的环境
        boolean currentEnv = environment.acceptsProfiles(profiles);

        return new Docket(DocumentationType.SWAGGER_2s)
                .apiInfo(apiInfo())
//                false 关闭Swagger
                .enable(currentEnv)
                .select()
//                RequestHandlerSelectors 配置要扫描接口的方式
                //basePackage 指定扫描的包
                //any   全部
                //none 都不扫描
                //withClassAnnotation 扫描类上的注解 反射
                //withMethodAnnotation 扫描方法上的注解 反射
//                .apis(RequestHandlerSelectors.withMethodAnnotation(GetMapping.class))
                .apis(RequestHandlerSelectors.basePackage("com.cyz.swagger.controller"))
//                过滤
                //ant指定路径
//                .paths(PathSelectors.ant("/cyz/**"))
                .build();
    }


```

## API分组和接口注释

```java
  return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                .groupName("Chen")
```

* 多个分组

```java
    @Bean
    public Docket docket1(){
        return new Docket(DocumentationType.SWAGGER_2).groupName("A");
    }
    @Bean
    public Docket docket2(){
        return new Docket(DocumentationType.SWAGGER_2).groupName("B");
    }
    @Bean
    public Docket docket3(){
        return new Docket(DocumentationType.SWAGGER_2).groupName("C");
    }
    @Bean
    public Docket docket4(){
        return new Docket(DocumentationType.SWAGGER_2).groupName("D");
    }
```

* 实体类 添加注解

```java
package com.cyz.swagger.pojo;

import io.swagger.annotations.ApiModel;
import io.swagger.annotations.ApiModelProperty;

//@Api注释等价
@ApiModel("用户实体类")
public class User {

    @ApiModelProperty("用户名")
    private String username;
    @ApiModelProperty("密码")
    private String password;



    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }
}

```

* controller

```javA
//只要我们的接口中,返回的实体类, 他就会被扫描道Swagger中
    @PostMapping("/user")
    public User user(){
        return new User();
    }
    
      //接口注释
    @ApiOperation("Hello控制类")
    @GetMapping("/hello2")
    public String hello2(@ApiParam("用户名") String username){
        return "hello"+username;
    }
    
 //接口注释
    @ApiOperation("Post测试类")
    @PostMapping("/hellop")
    public String hellop(@ApiParam("用户名") @RequestBody String username){
        System.out.println(username);
        return username;
    }
```

# 异步任务

* 业务层@Async//开启异步
* 启动类@EnableAsync//开启异步支持

```java
package com.cyz.service;

import org.springframework.scheduling.annotation.Async;
import org.springframework.stereotype.Service;

@Service
public class AsyncService {

    @Async//开启异步
    public void hello(){

        try {
            Thread.sleep(3000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println("数据正在请求中");

    }
}



```

```java
package com.cyz.controller;

import com.cyz.service.AsyncService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;


@RestController
public class AsyncController {

    @Autowired
    AsyncService asyncService;

    @RequestMapping("/hello")
    public String hello(){
        asyncService.hello();
        return "ok";
    }

}

```

```java
package com.cyz;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.scheduling.annotation.EnableAsync;

@EnableAsync//开启异步支持
@SpringBootApplication
public class Springboot09TestApplication {

    public static void main(String[] args) {
        SpringApplication.run(Springboot09TestApplication.class, args);
    }

}

```

# 邮件发送

* 依赖

```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-mail</artifactId>
        </dependency>
```

* 给自己都邮箱开启服务得到授权码
* 配置

```xml
server.port=8085
spring.mail.username=1604275453@qq.com
spring.mail.password=gkrcjhdtnmygiaec
spring.mail.host=smtp.qq.com
# 开启加密认证
spring.mail.properties.mail.smtp.ssl.enable=true
```

```java
package com.cyz;

import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.mail.SimpleMailMessage;
import org.springframework.mail.javamail.JavaMailSenderImpl;

@SpringBootTest
class Springboot09TestApplicationTests {

    @Autowired
    JavaMailSenderImpl mailSender;
    
    @Test
    void contextLoads() {
        //一个简单的邮件
        SimpleMailMessage message = new SimpleMailMessage();

        message.setSubject("cyz您好啊");
        message.setText("谢谢你的教程");

        message.setTo("1604275453@qq.com");
        message.setFrom("1604275453@qq.com");

        mailSender.send(message);
    }
    
    @Test
    void contextLoads2() throws MessagingException {
        //一个复杂的邮件
        MimeMessage mimeMessage = mailSender.createMimeMessage();

        //组装
        MimeMessageHelper helper = new MimeMessageHelper(mimeMessage,true);

        helper.setSubject("chen你好呀plus");
        helper.setText("<p style='color:red'>chen hello!</p>",true);

        //附件
        helper.addAttachment("1.jpg",new File("C:\\Users\\Administrator\\Desktop\\1.jpg"));
        helper.addAttachment("2.jpg",new File("C:\\Users\\Administrator\\Desktop\\1.jpg"));

        helper.setTo("1604275453@qq.com");
        helper.setFrom("1604275453@qq.com");

        mailSender.send(mimeMessage);
    }

}

```

# 定时任务

* 开启

```java
 package com.cyz;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.scheduling.annotation.EnableAsync;
import org.springframework.scheduling.annotation.EnableScheduling;

@EnableAsync//开启异步支持
@EnableScheduling//开启定时功能
@SpringBootApplication
public class Springboot09TestApplication {

    public static void main(String[] args) {
        SpringApplication.run(Springboot09TestApplication.class, args);
    }

}

```

* 什么时候执行@Scheduled

```java
package com.cyz.service;

import org.springframework.scheduling.annotation.Scheduled;
import org.springframework.scheduling.annotation.Schedules;
import org.springframework.stereotype.Service;

@Service
public class ScheduledService {

    //在特定的事件执行这个方法
    //使用cron表达式
    //秒 分 时 日 月 周几
    /*
        cron = "50 50 8 * * ?"  每天8点50分50秒

        30 0/5 10,18 * * ?      每天10点和18点 每个5分钟执行

        0 15 10 ? * 1-6         每个月周一到周六 10点15分 执行一次
     */
    @Scheduled(cron = "50 50 8 * * ?")
    public void hello(){
        System.out.println("hello,执行了");
    }
}

```

# SpringBoot整合Redis

* 依赖
* jedis: 采用的直连, 多线程操作不安全,避免不安全需要使用jedis pool连接池
* lettuce:采用netty, 实例可以在多个线程中进行共享,不存在线程不安全, 可以减少线程数据

```xml
<dependencies>
<!--        redis-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-configuration-processor</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
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

* 配置

```properties
spring.redis.host=127.0.0.1
spring.redis.port=6379
```

* 测试

```java
package com.cyz;

import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.data.redis.connection.RedisConnection;
import org.springframework.data.redis.core.RedisTemplate;

@SpringBootTest
class SpringbootRedisApplicationTests {

    @Autowired
    private RedisTemplate redisTemplate;

    @Test
    void contextLoads() {

        //redisTemplate
        //opsForValue 操作字符串
        //opsForList  操作 List
        //opsForSet
        //opsForHash
        //opsForZSet
        //opsForGeo
        //opsForHyperLogLog

        //除了这些
        //还可以操作基本的 事务和CRUD

        //获取连接对象
//        RedisConnection connection = redisTemplate.getConnectionFactory().getConnection();
//        connection.flushDb();
//        connection.flushAll();

        redisTemplate.opsForValue().set("mykey","chen");
        System.out.println(redisTemplate.opsForValue().get("mykey"));
    }

}

```

* 配置自己的配置类

```java
package com.cyz.config;

import com.fasterxml.jackson.annotation.JsonAutoDetect;
import com.fasterxml.jackson.annotation.PropertyAccessor;
import com.fasterxml.jackson.databind.ObjectMapper;
import org.springframework.boot.configurationprocessor.json.JSONObject;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.redis.connection.RedisConnectionFactory;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.serializer.Jackson2JsonRedisSerializer;
import org.springframework.data.redis.serializer.StringRedisSerializer;

import java.net.UnknownHostException;

@Configuration
public class RedisConfig {
    //编写自己的refisTemplate
    @Bean
    @SuppressWarnings("all")
    public RedisTemplate<String,Object> redisTemplate(RedisConnectionFactory redisConnectionFactory)
        throws UnknownHostException {
        //<String,Object> 为了开发方便
        RedisTemplate<String,Object> template = new RedisTemplate<>();
        template.setConnectionFactory(redisConnectionFactory);

        //json序列化配置
        Jackson2JsonRedisSerializer<Object> objectJackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer<>(Object.class);
        ObjectMapper objectMapper = new ObjectMapper();
        objectMapper.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        objectMapper.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);
        objectJackson2JsonRedisSerializer.setObjectMapper(objectMapper);
        //String 的序列化
        StringRedisSerializer stringRedisSerializer = new StringRedisSerializer();

        //配置采用String的序列化方式
        template.setKeySerializer(stringRedisSerializer);
        //hash的key也采用string的序列化方式
        template.setHashKeySerializer(stringRedisSerializer);
        //value的序列化方式采用jackson
        template.setValueSerializer(objectJackson2JsonRedisSerializer);
        //hash的value序列化方式采用jackson
        template.setHashValueSerializer(objectJackson2JsonRedisSerializer);
        template.afterPropertiesSet();

        return template;
    }
}
```

* pojo实现序列化

```java
package com.cyz.pojo;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;
import org.springframework.stereotype.Component;

import java.io.Serializable;

@Component
@NoArgsConstructor
@AllArgsConstructor
@Data
public class User implements Serializable {
    private String name;
    private int age;
}

```

* 测试

```java
package com.cyz;

import com.cyz.pojo.User;
import com.fasterxml.jackson.core.JsonProcessingException;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.data.redis.core.RedisTemplate;

@SpringBootTest
class SpringbootRedisApplicationTests {

    @Autowired
    @Qualifier("redisTemplate")
    private RedisTemplate redisTemplate;

    @Test
    public void test() throws JsonProcessingException {
        User user = new User("jack", 18);
        //转字符串
//        String jsonUser = new ObjectMapper().writeValueAsString(user);
        //直接传值对象会报错  对象需要序列化 implements Serializable
        redisTemplate.opsForValue().set("user",user);
        System.out.println(redisTemplate.opsForValue().get("user"));
    }
}

```

* 工具类

```java
package com.cyz.utils;

import ch.qos.logback.core.util.TimeUtil;
import com.fasterxml.jackson.databind.ObjectMapper;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.stereotype.Component;
import org.springframework.util.CollectionUtils;

import java.util.List;
import java.util.Map;
import java.util.Set;
import java.util.concurrent.TimeUnit;

@Component
public final class RedisUtil {

    @Autowired
    private RedisTemplate<String,Object> redisTemplate;

    /**
     * 指定缓存失效时间
     * @param key
     * @param time
     * @return
     */
    public boolean expire(String key, long time){
        try {
            if(time > 0){
                redisTemplate.expire(key,time, TimeUnit.SECONDS);
            }
            return true;
        }catch (Exception e){
            e.printStackTrace();
            return false;
        }
    }

    /**
     * 根据key 获取过期时间
     * @param key 键 不能为null
     * @return 时间(秒) 返回0 代表永久有效
     */
    public long getExpire(String key){
        return redisTemplate.getExpire(key,TimeUnit.SECONDS);
    }

    /**
     * 判断key是否存在
     * @param key 键
     * @return true 存在 false 不存在
     */
    public boolean hasKey(String key) {
        try {
            return redisTemplate.hasKey(key);
        }catch (Exception e){
            e.printStackTrace();
            return false;
        }
    }

    /**
     * 删除缓存
     * @param key 可以传一个值 或多个
     */
    public void del(String... key){
        if(key != null && key.length > 0){
            if(key.length == 1){
                redisTemplate.delete(key[0]);
            }else{
                redisTemplate.delete(CollectionUtils.arrayToList(key));
            }
        }
    }

    /**
     * 普通缓存获取
     * @param key 键
     * @return 值
     */
    public Object get (String key) {
        return key == null ? null : redisTemplate.opsForValue().get(key);
    }

    /**
     * 普通缓存放入
     * @param key 键
     * @param value 值
     * @return true 成功 false 失败
     */
    public boolean set(String key, Object value){
        try {
            redisTemplate.opsForValue().set(key,value);
            return true;
        }catch (Exception e){
            e.printStackTrace();
            return false;
        }
    }

    /**
     *  普通缓存放入并设置时间
     * @param key
     * @param value
     * @param time
     * @return
     */
    public boolean set(String key,Object value, long time){
        try {
            if (time > 0){
                redisTemplate.opsForValue().set(key,value,time,TimeUnit.SECONDS);
            }else{
                set(key,value);
            }
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }

    /**
     * 递增
     * @param key 键
     * @param delta 要增加几(大于0)
     * @return
     */
    public long incr(String key, long delta){
        if(delta < 0){
            throw new RuntimeException("递增因子必须大于0");
        }
        return redisTemplate.opsForValue().increment(key,delta);
    }

    /**
     * 递减
     * @param key 键
     * @param delta 要增加几(小于0)
     * @return
     */
    public long decr(String key, long delta){
        if(delta < 0){
            throw new RuntimeException("递增因子必须大于0");
        }
        return redisTemplate.opsForValue().increment(key,-delta);
    }

    /**
     * HashGet
     * @param key 键 不能为null
     * @param item 项 不能为null
     * @return
     */
    public Object hget(String key, String item){
        return redisTemplate.opsForHash().get(key,item);
    }

    /**
     * 获取hashkey对应的所有键值
     * @param key 键
     * @return 对应的多个键值
     */
    public Map<Object,Object> hmget(String key){
        return redisTemplate.opsForHash().entries(key);
    }

    /**
     * HashSet
     * @param key 键
     * @param map 对应对各键值
     * @return
     */
    public boolean hmset(String key, Map<String,Object> map){
        try {
            redisTemplate.opsForHash().putAll(key,map);
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }

    /**
     * HashSet 并设置时间
     * @param key 键
     * @param map 对应多个键值
     * @param time 时间(秒)
     * @return true 成功 false 失败
     */
    public boolean hmset(String key, Map<String,Object> map, long time){
        try {
            redisTemplate.opsForHash().putAll(key,map);
            if(time > 0){
                expire(key, time);
            }
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }


    /**
     * 向一张hash表中放入数据, 如果不存在将创建
     * @param key 键
     * @param item 项
     * @param value 值
     * @return true 成功 false 失败
     */
    public boolean hset(String key, String item, Object value){
        try {
            redisTemplate.opsForHash().put(key, item, value);
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }

    /**
     * 向一张hash表中放入数据, 如果不存在将创建
     * @param key 键
     * @param item 项
     * @param value 值
     * @param time 时间(秒)  注意如果已近存在hash表有时间, 会替换原来的
     * @return true 成功 false 失败
     */
    public boolean hset(String key, String item, Object value, long time){
        try {
            redisTemplate.opsForHash().put(key, item, value);
            if(time > 0){
                expire(key, time);
            }
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }

    /**
     * 删除hash表中的值
     * @param key 键 不能为null
     * @param item 项 可以使多个 不能为null
     */
    public void hdel(String key, Object... item){
        redisTemplate.opsForHash().delete(key,item);
    }

    /**
     * 判断hash表中是否有该项的值
     * @param key 键 不能为null
     * @param itme 项 不能为null
     * @return true 存在 false 不存在
     */
    public boolean hHaskey(String key, String itme){
        return redisTemplate.opsForHash().hasKey(key,itme);
    }

    /**
     *  hash递增 如果不存在, 就会创建一个 并把新增后的值返回
     * @param key 键
     * @param item 项
     * @param by 要增加几(大于0)
     * @return
     */
    public double hincr(String key, String item, double by){
        return redisTemplate.opsForHash().increment(key,item,by);
    }

    /**
     *  hash递减
     * @param key 键
     * @param item 项
     * @param by 要增加几(大于0)
     * @return
     */
    public double hdecr(String key, String item, double by){
        return redisTemplate.opsForHash().increment(key,item,-by);
    }

    /**
     * 根据key获取Set中的所有制
     * @param key
     * @return
     */
    public Set<Object> sGet(String key){
        try {
            return redisTemplate.opsForSet().members(key);
        } catch (Exception e) {
            e.printStackTrace();
            return null;
        }
    }


    /**
     * 根据value从一个set中查询 是否存在
     * @param key
     * @param value
     * @return
     */
    public boolean sHasKey(String key, Object value){
        try {
            return redisTemplate.opsForSet().isMember(key, value);
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }

    /**
     * 将数据放入set缓存
     * @param key 键
     * @param values 值 可以是多个
     * @return 成功个数
     */
    public long sSet(String key, Object... values){
        try {
            return redisTemplate.opsForSet().add(key, values);
        } catch (Exception e) {
            e.printStackTrace();
            return 0;
        }
    }

    /**
     * 将set数据放入缓存
     * @param key
     * @param time
     * @param values
     * @return
     */
    public long sSetAndTime(String key, long time, Object... values){
        try {
            Long count = redisTemplate.opsForSet().add(key, values);
            if (time > 0){
                expire(key,time);
            }
            return count;
        } catch (Exception e) {
            e.printStackTrace();
            return 0;
        }
    }


    /**
     * 获取set缓存的长度
     * @param key
     * @return
     */
    public long sGetSetSize(String key){
        try {
            return redisTemplate.opsForSet().size(key);
        } catch (Exception e) {
            e.printStackTrace();
            return 0;
        }
    }


    /**
     * 移除值为value的
     * @param key
     * @param values
     * @return
     */
    public long setRemove(String key, Object... values){
        try {
            Long count = redisTemplate.opsForSet().remove(key, values);
            return count;
        } catch (Exception e) {
            e.printStackTrace();
            return 0;
        }
    }

    /**
     * 获取list缓存的内容
     * @param key
     * @param start
     * @param end
     * @return
     */
    public List<Object> lGet(String key, long start, long end){
        try {
            return redisTemplate.opsForList().range(key,start,end);
        } catch (Exception e) {
            e.printStackTrace();
            return null;
        }
    }

    /**
     * 获取list缓存长度
     * @param key
     * @return
     */
    public long lGetListSize(String key){
        try {
            return redisTemplate.opsForList().size(key);
        } catch (Exception e) {
            e.printStackTrace();
            return 0;
        }
    }

    /**
     * 通过索引 获取list中的值
     * @param key
     * @param index
     * @return
     */
    public Object lGetListSize(String key, long index){
        try {
            return redisTemplate.opsForList().index(key, index);
        } catch (Exception e) {
            e.printStackTrace();
            return null;
        }
    }


    /**
     * 将list放入缓存
     * @param key
     * @param value
     * @return
     */
    public boolean lSet(String key, Object value){
        try {
            redisTemplate.opsForList().rightPush(key, value);
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }

    /**
     * 将list放入缓存 并设置时间
     * @param key
     * @param value
     * @param time
     * @return
     */
    public boolean lSet(String key, Object value, long time){
        try {
            redisTemplate.opsForList().rightPush(key, value);
            if(time > 0){
                expire(key,time);
            }
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }

    /**
     * 将list放入缓存
     * @param key
     * @param value
     * @return
     */
    public boolean lSet(String key, List<Object> value){
        try {
            redisTemplate.opsForList().rightPushAll(key, value);
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }

    /**
     * 将list放入缓存 并设置时间
     * @param key
     * @param value
     * @param time
     * @return
     */
    public boolean lSet(String key, List<Object> value, long time){
        try {
            redisTemplate.opsForList().rightPushAll(key, value);
            if(time > 0){
                expire(key,time);
            }
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }


    /**
     *  根据索引修改
     * @param key
     * @param index
     * @param value
     * @return
     */
    public boolean lUpdateIndex(String key, long index, Object value){
        try {
            redisTemplate.opsForList().set(key, index, value);
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }


    /**
     * 移除N个值为value
     * @param key 键
     * @param count 移除多少个
     * @param value 值
     * @return 移除的个数
     */
    public long lRemove(String key, long count, Object value) {
        try {
            Long remove = redisTemplate.opsForList().remove(key, count, value);
            return remove;
        } catch (Exception e) {
            e.printStackTrace();
            return 0;
        }
    }
}

```

* 测试

```java
 @Autowired
    private RedisUtil redisUtil;

    @Test
    public void Test1(){
        redisUtil.set("name","jack");
        System.out.println(redisUtil.get("name"));
    }
```

# 分布式Dubbo + Zookeeper

* 若干的独立计算机的集合 完成一个相同的任务

# RPC

* http协议  通讯
* RPC协议  远程过程调用
  * 通讯
  * 序列化: 数据传输需要转换
  * 引入dubbo

# Dubbo

* 高性能, 轻量级的开源Java RPC框架

## 三大核心

* 面向接口的远程方法调用
* 智能容错和负载均衡
* 服务自动注册和发现

http://dubbo.apache.org/img/architecture.png

# Zookeeper

* https://www.apache.org/dyn/closer.lua/zookeeper/zookeeper-3.6.1/apache-zookeeper-3.6.1-bin.tar.gz
* 配置文件添加zkServer.cmd

```
pause 
```

* 在conf中复制zoo_sample.cfg为zoo.cfg
* 启动服务端和客户端

```
 ls /  查看

 create -w /chen 123  存值
 
 get /chen //查值
```

# Dubbo-admin

* https://github.com/apache/dubbo-admin/tree/master
* 打包

```
mvn clean package -Dmaven.test.skip=true

或
mvn clean package install '-Dmaven.test.skip=true'

```

* 打包生成的target下的jar文件

* 执行

```
java -jar dubbo-admin-0.0.1-SNAPSHOT.jar
```

* 打开zookeeper服务
* 启动返回http://localhost:7001/

```
zookeeper  注册中心

dubbo-admin: 是一个监控管理后台 

Dubbo  jar包
```

## 环境测试

* 创建项目
* provider-server
* consumer-server
* 依赖

```xml
<dependencies>
        <!--        导入依赖  Dubbo + zookeeper-->
        <!--Dubbo-->
        <dependency>
            <groupId>org.apache.dubbo</groupId>
            <artifactId>dubbo-spring-boot-starter</artifactId>
            <version>2.7.6</version>
        </dependency>

        <!--    zookeeper    zkclient-->
        <!-- https://mvnrepository.com/artifact/com.github.sgroschupf/zkclient -->
        <dependency>
            <groupId>com.github.sgroschupf</groupId>
            <artifactId>zkclient</artifactId>
            <version>0.1</version>
        </dependency>
<!--        解决日志冲突-->
        <!-- https://mvnrepository.com/artifact/org.apache.curator/curator-framework -->
        <dependency>
            <groupId>org.apache.curator</groupId>
            <artifactId>curator-framework</artifactId>
            <version>4.3.0</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.apache.curator/curator-recipes -->
        <dependency>
            <groupId>org.apache.curator</groupId>
            <artifactId>curator-recipes</artifactId>
            <version>4.3.0</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.apache.zookeeper/zookeeper -->
        <dependency>
            <groupId>org.apache.zookeeper</groupId>
            <artifactId>zookeeper</artifactId>
            <version>3.6.1</version>
            <!--        排除这个-->
            <exclusions>
            <exclusion>
                <groupId>org.slf4j</groupId>
                <artifactId>slf4j-log4j12</artifactId>
            </exclusion>
            </exclusions>
        </dependency>



        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

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

* 测试
* provider-service

```java
package com.cyz.service;
public interface TicketService {
    public String getTicket();
}
```

```java
package com.cyz.service;

import org.apache.dubbo.config.annotation.Service;
import org.springframework.stereotype.Component;

//项目启动自动注册到注册中心
@Component//
@Service//注意包不要导入错  必须要
public class TicketServiceImpl implements TicketService {
    @Override
    public String getTicket() {
        return "《chen之教学》";
    }
}

```

```properties
server.port=8001

# 服务应用名字
dubbo.application.name=provider-server
# 注册中心地址
dubbo.registry.address=zookeeper://127.0.0.1:2181
# 那些服务要被注册
dubbo.scan.base-packages=com.cyz.service
```

* consumer-server

```properties
server.port=8002

# 消费者去哪里拿 需要名字
dubbo.application.name=consumer-server
# 注册中心
dubbo.registry.address=zookeeper://127.0.0.1:2181
```

```java
package com.cyz.service;

import org.apache.dubbo.config.annotation.Reference;
import org.springframework.stereotype.Service;

@Service //这里是spring的
public class UserService {
    //想拿到provider的票   去注册中心拿
    @Reference//引用  POM左边   或 定义路径相同的接口名  注意包是dubbo的
    TicketService ticketService;

    public void buyTicket(){
        String ticket = ticketService.getTicket();
        System.out.println("在注册中心拿到ticket"+ticket);
    }
}

```

```java
package com.cyz.service;

public interface TicketService {
    public String getTicket();
}

```

* 测试

```java
package com.cyz;

import com.cyz.service.UserService;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

@SpringBootTest
class ConsumerServerApplicationTests {

    @Autowired
    UserService userService;

    @Test
    void contextLoads() {
        userService.buyTicket();
    }

}

```





