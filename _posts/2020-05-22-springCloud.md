---
   layout: post
   title: "SpringCloud"                                                        
   date: 2020-05-22 09:00:00 +0530
   categories: JAVA SpringCloud Eureka Ribbon Feign Hystrix Zuul Config
---
  JAVA SpringCloud Eureka Ribbon Feign Hystrix Zuul Config


# SpringCloud

## 基础

1. JavaSE
2. 数据库
3. 前端
4. Servlet
5. Http
6. Mybatis
7. Spring
8. SpringMVC
9. SpringBoot
10. Dubbo, Zookeeper, 分布式基础
11. Maven, Git
12. Ajax, Json

## 微服务架构四个核心问题

1. 客户端如何访问
2. 服务之间如何通信
3. 如何治理服务
4. 服务挂了, 如何处理

## 解决问题

1. Spring Cloud NeyFlix                 一站式
   1. api网关, zuul组件
   2. Feign------Httpclinet-----Http通信方式  同步, 阻塞
   3. 服务注册发现:Eureka
   4. 熔断机制: Hystrix
   5. ....
2. Apache Dubbo Zookeeper        半自动, 整合其他的
   1. API: 没有, 找第三方,或者自己实现
   2. Dubbo
   3. Zookeeper
   4. 没有:借助Hystrix
   5. Dubbo这个方案不完善
3. Spring Cloud Alibaba                 一站式 更简单
   1. 拥有所有的

## 服务网格-----Server Mesh

## 总的

1. API
2. HTTP,RPC
3. 注册和发现
4. 熔断机制

# 什么是微服务

1. 架构思想/模式/风格
2. 提供单一额应用程序分层成一组小的服务   模块化
3. 服务运行独立, 服务之间相互协调,互相配置
4. 服务能够独立的部署运行, 使用不同语言,不同存储构建
5. 服务之间采用轻量级通信机制
6. 每个服务围绕具体业务构建, 为用户提供最终价值
7. 解耦, 单个服务独立进程

## 微服务和微服务架构

* 微服务:   注重点
* 微服务架构: 模式/风格

## 微服务优缺点

### 优点

1. 单一职责原则
2. 足够小, 开发效率高
3. 单独开发
4. 不同语言构建
5. 易于和第三方集成, 自动部署
6. 易于维护
7. 只是逻辑的代码, 不会和HTML, CSS或其他界面混合
8. 每个微服务都有自己的存储能力, 可以有自己的数据库, 也可以统一数据库

### 缺点

1. 通信成本高
2. 分布式系统的复杂性
3. 运维压力大
4. 系统部署依赖
5. 数据不一致性
6. 系统集成测试
7. 性能监控

## 微服务技术栈

| 微服务条目                             | 落地技术                                                  |
| -------------------------------------- | --------------------------------------------------------- |
| 服务开发                               | SpringBoot, Spring, SpringMvc                             |
| 服务配置与管理                         | Netflix公司的Archaius,阿里的Diamond                       |
| 服务注册和发现                         | Eureka, Consul, Zookeeper                                 |
| 服务调用                               | Rest, RPC, gRPC                                           |
| 服务熔断器                             | Hystrix, Envoy                                            |
| 负载均衡                               | RIbbon,Nginx                                              |
| 服务接口调用(客户端调用服务的简化工具) | Feign                                                     |
| 消息队列                               | Kafka,RabitMQ,ActiveMQ                                    |
| 服务配置中心管理                       | SpringClundConfig,Chef                                    |
| 服务路由(API网关)                      | Zuul等                                                    |
| 服务监控                               | Zabbix,Nagios,Metrics,Specatator                          |
| 全链路追踪                             | Zipkin,Brave,Dapper                                       |
| 服务部署                               | Docker,OpenStack,Kubernetes                               |
| 数据流操作开发包                       | SpringCloud Stream(封装Redis, Rabbit,Kafka)等发送接收消息 |
| 事件消息总线                           | SpringCloud Bus                                           |

## 为什么选择SpringCloud作为微服务架构

1. 整体解决方案和框架成熟度
2. 社区热度
3. 可维护性
4. 学习曲线

# SpringCloud

* 基于SpringBoot的微服务解决方案
* 包括: 注册与发现,配置中心,全链路监控,服务网关, 负载均可, 熔断器等组件
* 总之实现零配置的形式

## SpringClund和SpringBoot的关系

1. SpringBoot专注于快速方便的开单个个体微服务
2. SpringCloud整合治理单个个体微服务, 集成服务
3. SpringBoot可以离开SprigCloud独立使用,  当是springCloud离不开SpringBoot
4. SpringBoot专注于快速, 方便的开发单个个体微服务, SpringCloud关注全局的服务治理框架

## Dubbo和SpringCloud技术选型

1. 分布式+服务治理Dubbo

   1. 成熟的互联网架构: 应用服务化拆分 + 消息中间件
   2. 网站架构图

   ![网站架构图](G:\Soft\Git\chenyaoze666.github.io\assets\image\网站架构图.png)

2. Dubbo和SpringCloud对比

|              | Dubbo         | Spring                       |
| ------------ | ------------- | ---------------------------- |
| 服务注册中心 | Zookeeper     | Spring Cloud Netfilx Eureka  |
| 服务调用方式 | RPC           | REST API                     |
| 服务监控     | Dubbo-monitor | Spring Boot Admin            |
| 断路器       | 不完善        | Spring Cloud Netflix Hystrix |
| 服务网关     | 无            | Spring Cloud Netflix Zuul    |
| 分布式配置   | 无            | Spring Cloud Config          |
| 服务跟踪     | 无            | Spring Cloud Sleuth          |
| 消息总线     | 无            | Spring Cloud Bus             |
| 数据流       | 无            | Spring Cloud Stream          |
| 批量任务     | 无            | Spring Cloud Task            |

* 最大区别在于服务调用方式

## SpringCloud能做什么

1. 分布式/版本控制配置
2. 服务注册与发现
3. 路由
4. 服务到服务的调用
5. 负载均衡配置
6. 断路器
7. 分布式消息管理

# 项目搭建

* 数据库

```sql
CREATE DATABASE db01;

DROP TABLE IF EXISTS `dept`;
CREATE TABLE `dept`  (
  `deptno` bigint(20) NOT NULL AUTO_INCREMENT,
  `dname` varchar(60) NULL DEFAULT NULL,
  `db_source` varchar(60) NULL DEFAULT NULL,
  PRIMARY KEY (`deptno`) USING BTREE
) ENGINE = InnoDB AUTO_INCREMENT = 6 CHARACTER SET = utf8

INSERT INTO `dept` VALUES (1, '开发部', DATABASE());
INSERT INTO `dept` VALUES (2, '人事部', DATABASE());
INSERT INTO `dept` VALUES (3, '财务部', DATABASE());
INSERT INTO `dept` VALUES (4, '市场部', DATABASE());
INSERT INTO `dept` VALUES (5, '运维部', DATABASE());
```

## 父项目springcloud

* 依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>springcloud</artifactId>
        <groupId>com.cyz</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>springcloud-provider-dept-8001</artifactId>

    <dependencies>
        <!--        需要实体类  配置apimodule-->
        <dependency>
            <groupId>com.cyz</groupId>
            <artifactId>springcloud-api</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
        <!--        junit-->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
        </dependency>
        <dependency>
            <groupId>ch.qos.logback</groupId>
            <artifactId>logback-core</artifactId>
        </dependency>
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
        </dependency>

        <!--        test-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-test</artifactId>
            <version>2.3.0.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <version>2.3.0.RELEASE</version>
        </dependency>
        <!--jetty-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jetty</artifactId>
            <version>2.3.0.RELEASE</version>
        </dependency>
        <!--        热部署-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <version>2.3.0.RELEASE</version>
        </dependency>
    </dependencies>
</project>
```

## 子项目springcloud-api

* 依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>springcloud</artifactId>
        <groupId>com.cyz</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>springcloud-api</artifactId>

<!--    当前的module自己需要的依赖 如果父级依赖有版本就不用了写版本了-->
    <dependencies>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>
    </dependencies>
</project>
```

* 实现pojo

```java
package com.cyz.springcloud.pojo;

import lombok.Data;
import lombok.NoArgsConstructor;
import lombok.experimental.Accessors;

import java.io.Serializable;

@Data
@NoArgsConstructor
@Accessors(chain = true)//链式写法
public class Dept implements Serializable {
    private Long deptno;
    private String dname;
    //一个服务对应一个数据库
    private String db_source;

    public Dept(String dname) {
        this.dname = dname;
    }
    /*
    链式写法
    Dept dept = new Dept();

    dept.setDeptNo(11).setDname("ss").setDb_Source("001")
     */
}

```

## 子项目springcloud-provider-dept-8001服务提供者

* 依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>springcloud</artifactId>
        <groupId>com.cyz</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>springcloud-provider-dept-8001</artifactId>

    <dependencies>
        <!--        需要实体类  配置apimodule-->
        <dependency>
            <groupId>com.cyz</groupId>
            <artifactId>springcloud-api</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
        <!--        junit-->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
        </dependency>
        <dependency>
            <groupId>ch.qos.logback</groupId>
            <artifactId>logback-core</artifactId>
        </dependency>
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
        </dependency>

        <!--        test-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-test</artifactId>
            <version>2.3.0.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <version>2.3.0.RELEASE</version>
        </dependency>
        <!--jetty-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jetty</artifactId>
            <version>2.3.0.RELEASE</version>
        </dependency>
        <!--        热部署-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <version>2.3.0.RELEASE</version>
        </dependency>
    </dependencies>
</project>
```

* 配置application.yml

```yaml
server:
  port: 8001
# mybatis配置
mybatis:
  type-aliases-package: com.cyz.springcloud.pojo
  config-location: classpath:mybatis/mybatis-config.xml
  mapper-locations: classpath:mybatis/mapper/*.xml

# spring的配置
spring:
  application:
    name: springcloud-provider-dept
  datasource:
    type: com.alibaba.druid.pool.DruidDataSource
    driver-class-name: org.gjt.mm.mysql.Driver
    url: jdbc:mysql://localhost:3306/db01?serverTimezone=Asia/Shanghai&useSSL=false&useUncode=true&characterEncoding=utf-8
    username: root
    password: 123456
```

* 配置mybatis-config.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<!--核心配置文件-->
<configuration>
    <settings>
<!--        开启二级缓存-->
        <setting name="cacheEnabled" value="true"/>
    </settings>
</configuration>
```

* Mapper

```java
package com.cyz.springcloud.mapper;

import com.cyz.springcloud.pojo.Dept;
import org.apache.ibatis.annotations.Mapper;
import org.springframework.stereotype.Repository;

import java.util.List;

@Mapper
@Repository
public interface DeptMapper {
    public boolean addDept(Dept dept);

    public Dept queryById(Long id);

    public List<Dept> queryAll();
}

```

* mapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--核心配置文件-->
<mapper namespace="com.cyz.springcloud.mapper.DeptMapper">
    <insert id="addDept" parameterType="Dept">
        insert into db01.dept (dname, db_source) values (#{dname},DATABASE());
    </insert>
    <select id="queryById" resultType="Dept" parameterType="Long">
        select * from db01.dept where deptno = #{deptno}
    </select>
    <select id="queryAll" resultType="Dept">
        select * from db01.dept;
    </select>
</mapper>
```

* service

```java
package com.cyz.springcloud.service;

import com.cyz.springcloud.pojo.Dept;

import java.util.List;
public interface DeptService {
    public boolean addDept(Dept dept);

    public Dept queryById(Long id);

    public List<Dept> queryAll();
}

```

* serviceImpl

```java
package com.cyz.springcloud.service;

import com.cyz.springcloud.mapper.DeptMapper;
import com.cyz.springcloud.pojo.Dept;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class DeptServiceImol implements DeptService {

    @Autowired
    private DeptMapper deptMapper;

    @Override
    public boolean addDept(Dept dept) {
        return deptMapper.addDept(dept);
    }

    @Override
    public Dept queryById(Long id) {
        return deptMapper.queryById(id);
    }

    @Override
    public List<Dept> queryAll() {
        return deptMapper.queryAll();
    }
}

```

* controller

```java
package com.cyz.springcloud.controller;

import com.cyz.springcloud.pojo.Dept;
import com.cyz.springcloud.service.DeptService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.List;

@RestController
public class DeptController {
    @Autowired
    private DeptService deptService;

    @PostMapping("/dept/add")
    public boolean addDept(Dept dept){
        return deptService.addDept(dept);
    }


    @GetMapping("/dept/get/{id}")
    public Dept get(@PathVariable("id") Long id){
        return deptService.queryById(id);
    }

    @GetMapping("/dept/list")
    public List<Dept> queryAll(){
        return deptService.queryAll();
    }
}

```

* 启动类

```java
package com.cyz.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

//启动类
@SpringBootApplication
public class DeptProvide_8001 {
    public static void main(String[] args) {
        SpringApplication.run(DeptProvide_8001.class,args);
    }
}

```

* 运行测试

```http
http://localhost:8001/dept/get/1
http://localhost:8001/dept/list
http://localhost:8001/dept/add?dname="游戏部"//post请求不支持
```

## 子项目springcloud-consumer-dept-80消费者

* 依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>springcloud</artifactId>
        <groupId>com.cyz</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>springcloud-consumer-dept-80</artifactId>

    <!--    实体类+web-->
    <dependencies>
        <dependency>
            <groupId>com.cyz</groupId>
            <artifactId>springcloud-api</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <version>2.3.0.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <version>2.3.0.RELEASE</version>
        </dependency>
    </dependencies>

</project>
```

* 配置

```yaml
server:
  port: 80
```

* RestTemplate配置

```java
package com.cyz.springcloud.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.client.RestTemplate;

@Configuration
public class ConfigBean {

    @Bean
    public RestTemplate getRestTemplate(){
        return new RestTemplate();
    }
}

```

* controller

```java
package com.cyz.springcloud.controller;

import com.cyz.springcloud.pojo.Dept;
import com.sun.org.apache.xpath.internal.operations.Bool;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.client.RestTemplate;

import java.util.List;

@RestController
public class DeptConsumerController {

    //消费者 不应该有service层
    //RestTemplate 提供我们直接调用   需要手动配置爱注册
    //(url,实体:Map,Class<T> responseType)
    @Autowired
    private RestTemplate restTemplate;//提供多种便捷访问远程HTTP服务的方法  简单的RestFul服务模板

    private static final String REST_URL_PREFIX = "http://localhost:8001";

    @RequestMapping("/consumer/dept/add")
    public boolean add(Dept dept){
        return restTemplate.postForObject(REST_URL_PREFIX+"/dept/add",dept, Boolean.class);
    }

    @RequestMapping("/consumer/dept/get/{id}")
    public Dept get(@PathVariable("id") Long id){
        return restTemplate.getForObject(REST_URL_PREFIX+"/dept/get/"+id,Dept.class);
    }


    @RequestMapping("/consumer/dept/list")
    public List<Dept> list(){
        return restTemplate.getForObject(REST_URL_PREFIX+"/dept/list",List.class);
    }
}

```

* 启动类

```java
package com.cyz.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class DeptConsumer_80 {
    public static void main(String[] args) {
        SpringApplication.run(DeptConsumer_80.class);
    }
}

```

* 测试访问

```http
http://localhost/consumer/dept/get/1
http://localhost/consumer/dept/list
http://localhost/consumer/dept/add?dname="游戏部"
```

# Eureka

* 服务注册与发现
* 采用AP原则
* 类似于Zookeeper
* 通过EurekaServer来监控系统(心跳连接90秒) 监控微服务是否正常运行

## 三大角色

* Eureka Server: 提供服务的注册于发现, zookeeper
* service Provider: 将自身服务注册到Eureka中, 从而使消费方能够掌握
* Service Consumer: 服务消费方从Eureka中获取注册服务列表, 从而找到消费服务

## 搭建springcloud-eureka-7001

* 父依赖导入

```xml
		<dependency>
            <groupId>com.google.code.gson</groupId>
            <artifactId>gson</artifactId>
            <version>2.8.6</version>
        </dependency>

        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-core</artifactId>
            <version>2.11.0</version>
        </dependency>
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>2.11.0</version>
        </dependency>
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-annotations</artifactId>
            <version>2.11.0</version>
        </dependency>
```

* 依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>springcloud</artifactId>
        <groupId>com.cyz</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>springcloud-eureka-7001</artifactId>

<!--    导入包-->
    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
            <version>2.2.3.RELEASE</version>
        </dependency>
        
         <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <version>2.3.0.RELEASE</version>
        </dependency>
    </dependencies>

</project>
```

* 配置

```yaml
server:
  port: 7001

eureka:
  instance:
    hostname: localhost # Eureka服务端的实例名称
  client:
    register-with-eureka: false #表示是否向eureka注册中心注册自己
    fetch-registry: false # false 表示自己是注册中心
    service-url: # 表示监控页面
      defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/
```

* 启动类

```java
package com.cyz.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.server.EnableEurekaServer;

@SpringBootApplication
@EnableEurekaServer //服务端的启动类, 可以接受别人注册进来
public class EurekaServer_7001 {
    public static void main(String[] args) {
        SpringApplication.run(EurekaServer_7001.class,args);
    }
}

```

* 启动测试访问localhost:7001

* 服务注册
* 在8001导入依赖

```xml
  <!--eureka-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-eureka</artifactId>
            <version>1.4.7.RELEASE</version>
        </dependency>
```

* 配置

```yaml

# Eureka的配置 服务注册到哪里
eureka:
  client:
    service-url:
      defaultZone: http://localhost:7001/eureka/
  instance:
    instance-id: springcloud-provider-dept8001 # 描述信息
```

* 启动类

```java
package com.cyz.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.EnableEurekaClient;

//启动类
@SpringBootApplication
@EnableEurekaClient//自动注册到服务中心
public class DeptProvide_8001 {
    public static void main(String[] args) {
        SpringApplication.run(DeptProvide_8001.class, args);
    }
}

```

* 访问http://localhost:7001/
* 导入依赖

```xml
  <!--        完善监控信息-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
            <version>2.3.0.RELEASE</version>
        </dependency>
```

* 配置

```yaml
# info配置
info:
  app.name: cyz-springcloud
  company.name: chenyaoze666.github.io
```

* 访问http://localhost:8001/actuator/info

## 自动保护机制

```java
EMERGENCY! EUREKA MAY BE INCORRECTLY CLAIMING INSTANCES ARE UP WHEN THEY'RE NOT. RENEWALS ARE LESSER THAN THRESHOLD AND HENCE THE INSTANCES ARE NOT BEING EXPIRED JUST TO BE SAFE.
```

## 注册服务信息

* controller

```java
package com.cyz.springcloud.controller;

import com.cyz.springcloud.pojo.Dept;
import com.cyz.springcloud.service.DeptService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.cloud.client.ServiceInstance;
import org.springframework.cloud.client.discovery.DiscoveryClient;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.List;

@RestController
public class DeptController {
    //获取一些配置的信息, 得到具体的微服务
    @Autowired
    private DiscoveryClient client;

    ...
        
    //注册进来的微服务 , 获取一些消息
    @GetMapping("/dept/discovery")
    public Object discovery() {
        //获取微服务列表的清单
        List<String> services = client.getServices();
        System.out.println("discovery=>services" + services);
        //得到一个具体的微服务ID applicationName
        List<ServiceInstance> instances = client.getInstances("SPRINGCLOUD-PROVIDER-DEPT");
        for (ServiceInstance instance : instances) {
            System.out.println(
                    instance.getHost() +"\t"+
                            instance.getPort() +"\t"+
                            instance.getUri() +"\t"+
                            instance.getServiceId()
            );
        }

        return this.client;
    }
}

```

* 启动类

```java
package com.cyz.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;
import org.springframework.cloud.netflix.eureka.EnableEurekaClient;

//启动类
@SpringBootApplication
@EnableEurekaClient
@EnableDiscoveryClient//服务发现
public class DeptProvide_8001 {
    public static void main(String[] args) {
        SpringApplication.run(DeptProvide_8001.class, args);
    }
}

```

* 访问http://localhost:8001/dept/discovery

* 集群配置
* 注意这里不能是localhost
* 分别创建三个服务注册
* 分别配置

```yaml
  eureka:
  instance:
    hostname: cyz7001 # Eureka服务端的实例名称
  defaultZone: http://cyz7002:7002/eureka/,http://cyz7001:7001/eureka/
```

* 8001配置

```yaml
defaultZone: http://cyz7001:7001/eureka/,http://cyz7002:7002/eureka/,http://cyz7003:7003/eureka/
```

## ACID(RDBMS)

* 原子性
* 一致性
* 隔离性
* 持久性
* RDBMS: Mysql, Oracle, sqlServer

## CAP原则(NoSQL)

* C(Consistency)  强一致性
* A(Availability) 可用性
* P(Partition tolerance) 分区容错性
* NoSQL: redis, mongdb

## CAP的三进二(最多实现三个原则)

* CA: 单点集群, 可扩展性较差
* AP: 通常性能不是特别高
* CP: 可能对一致性要求低一些

## CAP理论的核心

* 一个分布式系统不可能同时很好的满足一致性, 可用性和容错性三个需求

## Eureka比Zookeepre好在哪里

* P是必须的

* Zookeeper: 保证的是CP

  * 保证数据的一致性, 数据会出现是几分钟以前的
  * 当主节点死亡时, 重新选取主节点, 选取过程时间长, 产生服务不可用

* Eureka: 保证的是AP

  * 先保证可用性, 但可能查到的信息不是最新的

  * 心跳机制, 15分内钟超过85%的节点没有正常的心跳

    1. 不会移除没有收到心跳而应该过去的服务

    2. 仍然能够接受新服务的注册和查询请求, 但是不会被同步到其他节点上(保证当前节点可用)

    3. 当网络稳定时, 当前实例新的注册信息会被同步到其他节点中

##  Eureka可用很好的应对网络故障导致部分节点失去联系的情况, 而zookeeper那样整个注册服务瘫痪

# Ribbon

* 基于Netflix Ribbon实现的一套"客户端负载均衡的工具"
* 提供客户端软件负载均衡算法,将Netflix的中间层服务连接在一起,.
* 提供的配置: 连接超时, 重试等
* 简单的说就是在配置文件中列出LoadBalancer(负载均衡)后面所有的机器(服务器),Ribbon会自动的帮助我们一某种规则(如简单轮询, 随机连接等)去连接这些机器. 我们很容易使用Ribbon实现自定义的负载均衡算法.

## Ribbon能干嘛

1. 负载均衡(LB): 用户请求平摊在多台服务器上 
2. 负载均衡软件: Nginx, Lvs等等
3. dubbo和springcloud都提供了负载均衡, springcloud的负载均衡算法可以自定义

## 负载均衡简单分类

1. 集中式LB
   1. 即在服务的消费方和提供方之间使用独立的LB设施,如: Nginx反向代理服务器, 由该设施负责把访问请求通过某种策略转发至服务的提供方
2. 进程式LB
   1. 将LB逻辑集成到消费方, 消费方从服务注册中心获知有哪些地址可用, 然后自己在从这些地址中选一个合适的服务器
   2. Ribbon就属于进程内LB, 它只是一个类库, 集成于消费方进程, 消费方通过它来获取到服务提供方的地址

## 环境搭建

* 在客户端80导入依赖

```xml
<!--        集成Ribbon-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-ribbon</artifactId>
            <version>2.2.3.RELEASE</version>
        </dependency>
<!--        eureka-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-eureka</artifactId>
            <version>1.4.7.RELEASE</version>
        </dependency>

  		<dependency>
            <groupId>org.apache.httpcomponents</groupId>
            <artifactId>httpclient</artifactId>
            <version>4.5.12</version>
        </dependency>
```

* 配置

```yaml
server:
  port: 80

# Eureka配置
eureka:
  client:
    register-with-eureka: false # 不向Eureka注册自己
    service-url:
      defaultZone: http://cyz7001:7001/eureka/,http://cyz7002:7002/eureka/,http://cyz7003:7003/eureka/
```

* 启动类

```java
package com.cyz.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.EnableEurekaClient;

//Eureka 和 Ribbon 整合后, 不需要关心访问地址和端口号
@SpringBootApplication
@EnableEurekaClient
public class DeptConsumer_80 {
    public static void main(String[] args) {
        SpringApplication.run(DeptConsumer_80.class);
    }
}

```

* 配置类

```java
package com.cyz.springcloud.config;

import org.springframework.cloud.client.loadbalancer.LoadBalanced;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.client.RestTemplate;

@Configuration
public class ConfigBean {

    //配置负载均衡实现RestTemplate
    @Bean
    @LoadBalanced//Ribbon
    public RestTemplate getRestTemplate(){
        return new RestTemplate();
    }

}

```

* controller

```java
package com.cyz.springcloud.controller;

import com.cyz.springcloud.pojo.Dept;
import com.sun.org.apache.xpath.internal.operations.Bool;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.client.RestTemplate;

import java.util.List;

@RestController
public class DeptConsumerController {

    //消费者 不应该有service层
    //RestTemplate 提供我们直接调用   需要手动配置爱注册
    //(url,实体:Map,Class<T> responseType)
    @Autowired
    private RestTemplate restTemplate;//提供多种便捷访问远程HTTP服务的方法  简单的RestFul服务模板

    //Ribbon实现的地址应该是一个变量  及服务的名称: SPRINGCLOUD-PROVIDER-DEPT
//    private static final String REST_URL_PREFIX = "http://localhost:8001";
    private static final String REST_URL_PREFIX = "http://SPRINGCLOUD-PROVIDER-DEPT";

    @RequestMapping("/consumer/dept/add")
    public boolean add(Dept dept){
        return restTemplate.postForObject(REST_URL_PREFIX+"/dept/add",dept, Boolean.class);
    }

    @RequestMapping("/consumer/dept/get/{id}")
    public Dept get(@PathVariable("id") Long id){
        return restTemplate.getForObject(REST_URL_PREFIX+"/dept/get/"+id,Dept.class);
    }


    @RequestMapping("/consumer/dept/list")
    public List<Dept> list(){
        return restTemplate.getForObject(REST_URL_PREFIX+"/dept/list",List.class);
    }

}

```

## 实现负载均衡

* 创建相同的数据库db02,db03
* 创建相同的三个8001,8002,8003连接三个数据库
* 默认轮询算法

### 改成随机

```java
package com.cyz.springcloud.config;

import com.netflix.loadbalancer.IRule;
import com.netflix.loadbalancer.RandomRule;
import org.springframework.cloud.client.loadbalancer.LoadBalanced;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.client.RestTemplate;

@Configuration
public class ConfigBean {

    //配置负载均衡实现RestTemplate
    //IRule接口
    //AvailabilityFilteringRule 会先过滤跳闸的服务  对剩下的进行轮询
    //RoundRobinRule  轮询
    //RandomRule 随机
    //RetryRule   会先按照轮询获取服务, 如果服务获取失败, 则会在指定时间内进行 重试
    @Bean
    @LoadBalanced//Ribbon
    public RestTemplate getRestTemplate(){
        return new RestTemplate();
    }


    @Bean//改成随机
    public IRule myRule(){
        return new RandomRule();
    }
}

```

## 自定义Ribbon

* 在启动类上级目录进行配置   避免被@ComponentScan扫描
* 启动配配置

```java
package com.cyz.springcloud;

import com.cyz.myrule.ChenRule;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.EnableEurekaClient;
import org.springframework.cloud.netflix.ribbon.RibbonClient;

//Eureka 和 Ribbon 整合后, 不需要关心访问地址和端口号
@SpringBootApplication
@EnableEurekaClient
//在微服务启动的时候就能去加载我们自定义Ribbon类
@RibbonClient(name = "SPRINGCLOUD-PROVIDER-DEPT",configuration = ChenRule.class)
public class DeptConsumer_80 {
    public static void main(String[] args) {
        SpringApplication.run(DeptConsumer_80.class);
    }
}
```

* 自定义ChenRandomRule  改造原有的RandomRule算法

```java
package com.cyz.myrule;

import com.netflix.client.config.IClientConfig;
import com.netflix.loadbalancer.AbstractLoadBalancerRule;
import com.netflix.loadbalancer.ILoadBalancer;
import com.netflix.loadbalancer.Server;

import java.util.List;
import java.util.concurrent.ThreadLocalRandom;


public class ChenRandomRule extends AbstractLoadBalancerRule {

    //每个机器, 访问5次,换下一个服务(3个)

    //total = 0, 默认 0 如果=5, 我们指向下一个服务节点
    //index = 0 默认 0 如果total = 5, index + 1

    private int total = 0; //被调用的次数
    private int currentIndex = 0; //当前是谁在提供服务

    //@edu.umd.cs.findbugs.annotations.SuppressWarnings(value = "RCN_REDUNDANT_NULLCHECK_OF_NULL_VALUE")
    public Server choose(ILoadBalancer lb, Object key) {
        if (lb == null) {
            return null;
        }
        Server server = null;

        while (server == null) {
            if (Thread.interrupted()) {
                return null;
            }
            List<Server> upList = lb.getReachableServers();//获得活着的
            List<Server> allList = lb.getAllServers();//获得全部的

            int serverCount = allList.size();
            if (serverCount == 0) {
                return null;
            }

//            int index = chooseRandomInt(serverCount);//生成区间随机数
//            server = upList.get(index);//从活着的服务中随机获取

            //自定义算法
            //============================
            if(total < 5){
                server = upList.get(currentIndex);
                total++;
            }else {
                total = 0;
                currentIndex++;
                if (currentIndex > upList.size() -1){
                    currentIndex = 0;
                }
                server = upList.get(currentIndex);//从活着额服务中, 获取指定的服务进行操作
            }
            //=======================
            if (server == null) {
                Thread.yield();
                continue;
            }
            if (server.isAlive()) {
                return (server);
            }

            server = null;
            Thread.yield();
        }

        return server;

    }

    protected int chooseRandomInt(int serverCount) {
        return ThreadLocalRandom.current().nextInt(serverCount);
    }

    @Override
    public Server choose(Object key) {
        return choose(getLoadBalancer(), key);
    }

    @Override
    public void initWithNiwsConfig(IClientConfig clientConfig) {
        // TODO Auto-generated method stub

    }
}

```

* 配置自定义 注意是在启动类上一级

```java
package com.cyz.myrule;

import com.netflix.loadbalancer.IRule;
import com.netflix.loadbalancer.RandomRule;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

public class ChenRule {
    @Bean
    public IRule myRule(){
        return new ChenRandomRule();
    }
}

```

# Feign负载均衡

* 声明式的web service客户端
* 只需要穿件一个借口, 然后添加注解即可
* Feign集成了Ribbon

## 调用微服务

1. 微服务名字(ribbon)
2. 接口和注解

## 环境搭建

* 依赖

```xml
 <!--        feign-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-openfeign</artifactId>
            <version>2.2.3.RELEASE</version>
        </dependency>
```

* 在api中增加service层 提供接口

```java
package com.cyz.springcloud.service;

import com.cyz.springcloud.pojo.Dept;
import org.springframework.cloud.openfeign.FeignClient;
import org.springframework.stereotype.Component;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;

import java.util.List;

@Component
@FeignClient(value = "SPRINGCLOUD-PROVIDER-DEPT")
public interface DeptClientService {

    @GetMapping("/dept/get/{id}")
    public Dept queryById(@PathVariable("id") Long id);

    @GetMapping("/dept/list")
    public List<Dept> queryAll();

    @GetMapping("/dept/add")
    public boolean addDept(Dept dept);
}

```

* 消费者

```java
package com.cyz.springcloud.controller;

import com.cyz.springcloud.pojo.Dept;
import com.cyz.springcloud.service.DeptClientService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.client.RestTemplate;

import java.util.List;

@RestController
public class DeptConsumerController {

//原来通过服务名调用 现在通过接口
    @Autowired
    private DeptClientService deptClientService;

    @RequestMapping("/consumer/dept/add")
    public boolean add(Dept dept){
        return this.deptClientService.addDept(dept);
    }

    @RequestMapping("/consumer/dept/get/{id}")
    public Dept get(@PathVariable("id") Long id){
        return this.deptClientService.queryById(id);
    }


    @RequestMapping("/consumer/dept/list")
    public List<Dept> list(){
        return this.deptClientService.queryAll();
    }
}

```

# 熔断Hystrix

* 服务雪崩
* 一个服务失败, 卡住了 不能进入下一个服务, 这期间会要大量访问这个失败的服务, 造成服务占用大量资源, 出现奔溃
* 我们要做的就是在失败的是后, 做一个备份返回给客户端服务奔溃的消息 让其他流程能够继续走
* Hystrix就是为了避免服务失败, 返回备选响应

## 能做什么

1. 服务降级
2. 服务熔断
3. 服务限流
4. 接近实时的监控
5. ...

## 服务熔断

* 微服务链路的一种保护机制
* 熔断节点微服务的调用, 快速返回 错误的响应信息

### 环境搭建

* 依赖

```xml
 <!--hystrix-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
            <version>2.2.3.RELEASE</version>
        </dependency>
```

* 复制8001修改

* controller

```java
package com.cyz.springcloud.controller;

import com.cyz.springcloud.pojo.Dept;
import com.cyz.springcloud.service.DeptService;
import com.netflix.hystrix.contrib.javanica.annotation.HystrixCommand;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.cloud.client.ServiceInstance;
import org.springframework.cloud.client.discovery.DiscoveryClient;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.List;

@RestController
public class DeptController {
    @Autowired
    private DeptService deptService;

    //获取一些配置的信息, 得到具体的微服务
    @Autowired
    private DiscoveryClient client;

    @PostMapping("/dept/add")
    public boolean addDept(Dept dept) {
        return deptService.addDept(dept);
    }

    @GetMapping("/dept/get/{id}")
    @HystrixCommand(fallbackMethod = "hystrixGet")//开启注解 失败调用方法
    public Dept get(@PathVariable("id") Long id) {
        Dept dept = deptService.queryById(id);

        if (dept == null){
            throw new RuntimeException("id=>"+id+",不存在该用户, 或者信息无法找到");
        }

        return dept;
    }

    //备选方法
    public Dept hystrixGet(@PathVariable("id") Long id) {
        return new Dept()
                .setDeptno(id)
                .setDname("id=>"+id+",没有对应的i信息, null--@Hystrix")
                .setDb_source("no this database in MySQL");
    }
}

```

* 启动类

```java
package com.cyz.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.circuitbreaker.EnableCircuitBreaker;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;
import org.springframework.cloud.netflix.eureka.EnableEurekaClient;

//启动类
@SpringBootApplication
@EnableEurekaClient
@EnableDiscoveryClient//服务发现
@EnableCircuitBreaker//添加熔断的支持 断路器
public class DeptProviderHystrix_8001 {
    public static void main(String[] args) {
        SpringApplication.run(DeptProviderHystrix_8001.class, args);
    }
}

```

* 访问时, 不存在返回错误信息

* 显示服务的IP地址

```yaml
 instance:
      instance-id: springcloud-provider-dept-hystrix-8001 # 描述信息
      prefer-ip-address: true
```

## 服务降级

* 不同的服务之间   访问A的很多 而B和C的服务很闲
* 整体资源不够用, 就忍痛割爱
* 告诉客户服务B和C当前不可用

### 环境搭建

* api中

```java
package com.cyz.springcloud.service;

import com.cyz.springcloud.pojo.Dept;
import feign.hystrix.FallbackFactory;
import org.springframework.stereotype.Component;

import java.util.List;

//降级 ~
@Component
public class DeptClientServiceFallFactory implements FallbackFactory {
    @Override
    public DeptClientService create(Throwable throwable) {
        return new DeptClientService() {
            @Override
            public Dept queryById(Long id) {
                return new Dept()
                        .setDeptno(id)
                        .setDname("id=>"+id+"没有响应的信息, 这个服务开启了降级, 暂时关闭")
                        .setDb_source("没有数据");
            }

            @Override
            public List<Dept> queryAll() {
                return null;
            }

            @Override
            public boolean addDept(Dept dept) {
                return false;
            }
        };
    }
}

```

* service接口

```java
package com.cyz.springcloud.service;

import com.cyz.springcloud.pojo.Dept;
import org.springframework.cloud.openfeign.FeignClient;
import org.springframework.stereotype.Component;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;

import java.util.List;

@Component
@FeignClient(value = "SPRINGCLOUD-PROVIDER-DEPT",fallbackFactory = DeptClientServiceFallFactory.class)
public interface DeptClientService {

    @GetMapping("/dept/get/{id}")
    public Dept queryById(@PathVariable("id") Long id);

    @GetMapping("/dept/list")
    public List<Dept> queryAll();

    @GetMapping("/dept/add")
    public boolean addDept(Dept dept);
}

```

* 配置fegin消费者

```yaml
server:
  port: 80

# 开启降级fegin.hystrix
feign:
  hystrix:
    enabled: true

# Eureka配置
eureka:
  client:
    register-with-eureka: false # 不向Eureka注册自己
    service-url:
      defaultZone: http://cyz7001:7001/eureka/,http://cyz7002:7002/eureka/,http://cyz7003:7003/eureka/

```

* 服务熔断
  * 某个服务超时或者异常,引起熔断 保险丝
* 服务降级
  * 客户端 从整体网络请求负载考虑 当某个服务熔断或者关闭之后, 服务将不断调用
    * 此时在客户端, 我们可以准备一个FallbackFactory, 返回一个默认的值(缺省值)

## 流监控dashboard

## 环境搭建

* 新建9001消费者

* 依赖

```xml
 <dependencies>
<!--hystrix-dashboard-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-hystrix-dashboard</artifactId>
            <version>2.2.3.RELEASE</version>
        </dependency>
        <!--hystrix-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
            <version>2.2.3.RELEASE</version>
        </dependency>
    </dependencies>
```

* 配置端口

```yaml
server:
  port: 9001
```

* 启动类

```java
package com.cyz.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.hystrix.dashboard.EnableHystrixDashboard;

@SpringBootApplication
@EnableHystrixDashboard//开启
public class DeptConsumerDashboard {
    public static void main(String[] args) {
        SpringApplication.run(DeptConsumerDashboard.class,args);
    }
}
```

* 访问http://localhost:9001/hystrix

* 在8001的启动类增加servlet

```java
package com.cyz.springcloud;

        import com.netflix.hystrix.contrib.metrics.eventstream.HystrixMetricsStreamServlet;
        import org.springframework.boot.SpringApplication;
        import org.springframework.boot.autoconfigure.SpringBootApplication;
        import org.springframework.boot.web.servlet.ServletRegistrationBean;
        import org.springframework.cloud.client.circuitbreaker.EnableCircuitBreaker;
        import org.springframework.cloud.client.discovery.EnableDiscoveryClient;
        import org.springframework.cloud.netflix.eureka.EnableEurekaClient;
        import org.springframework.context.annotation.Bean;


//启动类
@SpringBootApplication
@EnableEurekaClient
@EnableDiscoveryClient//服务发现
@EnableCircuitBreaker//添加熔断的支持 断路器
public class DeptProviderHystrix_8001 {
    public static void main(String[] args) {
        SpringApplication.run(DeptProviderHystrix_8001.class, args);
    }
    //增加一个Servlet
    @Bean
    public ServletRegistrationBean hystrixMetricsStreamServlet(){
        ServletRegistrationBean servletRegistrationBean = new ServletRegistrationBean(new HystrixMetricsStreamServlet());
        servletRegistrationBean.addUrlMappings("/actuator/hystrix.stream");
        return servletRegistrationBean;
    }
}

```

* 注意生产者需要

```xml
  <!--        完善监控信息-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
            <version>2.3.0.RELEASE</version>
        </dependency>
```

# Zuul路由网关

* 对请求的路由和过滤的两个最主要的功能
* 外部请求统一入口,过滤器过程干预,请求校验
* Zuul整合Eureka
* Zuul最终还是把服务注册到Eureka
* 代理+路由+过滤 三大功能

## 环境搭建

* 依赖

```xml
  <!--zuul-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-zuul</artifactId>
            <version>2.2.3.RELEASE</version>
        </dependency>

```

* 配置

```yaml
server:
  port: 9527

spring:
  application:
    name: springcloud-zuul

eureka:
  client:
    service-url:
      defaultZone: http://cyz7001:7001/eureka/,http://cyz7002:7002/eureka/,http://cyz7003:7003/eureka/
  instance:
    instance-id: zuul9527.com
    prefer-ip-address: true

info:
  app.name: chen-springcloud
  company.name: chenyaoze666.github.io

zuul:
  routes:
    mydept.serviceId: springcloud-provider-dept
    mydept.path: /mydept/**
  ignored-services: springcloud-provider-dept # 忽略这个
   # ignored-services: "*"   # 隐藏全部
  prefix: /chen # 配置公共的前缀

```

* 启动类

```java
package com.cyz.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.zuul.EnableZuulServer;

@SpringBootApplication
@EnableZuulServer
public class ZuulAplication_9527 {
    public static void main(String[] args) {
        SpringApplication.run(ZuulAplication_9527.class,args);
    }
}

```

# Config

* 配置文件统一管理(配置管理中心)
* 分布式配置中心

## git仓库

* 配置文件application.yml

```yaml
spring:
  profiles:
    active: dev

---
spring:
  profiles: dev
  application:
    name: springcloud-config-dev
---
spring:
  profiles: test
  application:
    name: springcloud-config-test
```

## 环境搭建

* 依赖

```xml
  <dependencies>
<!--        config-->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-config-server</artifactId>
    <version>2.2.3.RELEASE</version>
</dependency>


        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <version>2.3.0.RELEASE</version>
        </dependency>
    </dependencies>
```

* 配置

```yaml
server:
  port: 3344

spring:
  application:
    name: springcloud-config-server
#  连接远程仓库
  cloud:
    config:
      git:
        uri: https://gitee.com/cyz666666/springcloud-config.git

```

* 启动类

```java
package com.cyz.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.config.server.EnableConfigServer;

@SpringBootApplication
@EnableConfigServer
public class Config_Server_3344 {
    public static void main(String[] args) {
        SpringApplication.run(Config_Server_3344.class,args);
    }
}

```

* 访问

```http
http://localhost:3344/application-dev.yml
http://localhost:3344/application-test.yml

http://localhost:3344/application-dev.yml/master
http://localhost:3344/master/application-dev.yml
```

## 客户端环境3355

* 仓库config-client.yml

```yaml
spring:
  profiles:
    active: dev

---
server:
  port: 8201

# spring的配置
spring:
  profiles: dev
  application:
    name: springcloud-provider-dept

# Eureka的配置 服务注册到哪里
eureka:
  client:
    service-url:
      defaultZone: http://cyz7001:7001/eureka/,http://cyz7002:7002/eureka/,http://cyz7003:7003/eureka/

---
server:
  port: 8202
# spring的配置
spring:
  profiles: test
  application:
    name: springcloud-provider-dept

# Eureka的配置 服务注册到哪里
eureka:
  client:
    service-url:
      defaultZone: http://cyz7001:7001/eureka/,http://cyz7002:7002/eureka/,http://cyz7003:7003/eureka/

```

* 依赖

```xml
 <dependencies>
<!--        config-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-config</artifactId>
            <version>2.2.3.RELEASE</version>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <version>2.3.0.RELEASE</version>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
            <version>2.3.0.RELEASE</version>
        </dependency>
    </dependencies>
```

* bootstrap.yml

```yaml
# 系统级别的配置
spring:
  cloud:
    config:
      uri: http://localhost:3344
      name: config-client # 需要从git中获取的配置名称
      profile: dev
      label: master
```

* application.yml

```yaml
# 用户级别的配置
spring:
  application:
    name: springcloud-config-client-3355
```

* controller

```java
package com.cyz.springcloud.controller;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class ConfigClientController {

    @Value("${spring.application.name}")
    private String applicationName;

    @Value("${eureka.client.service-url.defaultZone}")
    private String eurekaServer;

    @Value("${server.port}")
    private String port;

    @RequestMapping("/config")
    public String getConfig(){
        return "applicationName:"+applicationName +
                "eurekaServer:" +
                "port:" + port;
    }
}
```

* 启动类

```java
package com.cyz.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class ConfigClient_3355 {
    public static void main(String[] args) {
        SpringApplication.run(ConfigClient_3355.class,args);
    }
}

```

* 访问http://localhost:8201/config

## 实战配置解耦

* 仓库配置

* config-eureka.yml

```yaml
spring:
  profiles:
    active: dev

---

server:
  port: 7001

spring:
  profiles: dev
  application:
    name: springcloud-config-eureka

eureka:
  instance:
    hostname: cyz7001 # Eureka服务端的实例名称
  client:
    register-with-eureka: false #表示是否向eureka注册中心注册自己
    fetch-registry: false # false 表示自己是注册中心
    service-url: # 表示监控页面
      defaultZone: http://cyz7002:7002/eureka/,http://cyz7003:7003/eureka/ # 关联
#      defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/   # 单击

---
server:
  port: 7001

spring:
  profiles: test
  application:
    name: springcloud-config-eureka

eureka:
  instance:
    hostname: cyz7001 # Eureka服务端的实例名称
  client:
    register-with-eureka: false #表示是否向eureka注册中心注册自己
    fetch-registry: false # false 表示自己是注册中心
    service-url: # 表示监控页面
      defaultZone: http://cyz7002:7002/eureka/,http://cyz7003:7003/eureka/ # 关联
#      defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/   # 单击

```

* config-dept.yml

```yaml
spring:
  profiles:
    active: dev
---

server:
  port: 8001
# mybatis配置
mybatis:
  type-aliases-package: com.cyz.springcloud.pojo
  config-location: classpath:mybatis/mybatis-config.xml
  mapper-locations: classpath:mybatis/mapper/*.xml

# spring的配置
spring:
  profiles: dev
  application:
    name: springcloud-config-dept
  datasource:
    type: com.alibaba.druid.pool.DruidDataSource
    driver-class-name: org.gjt.mm.mysql.Driver
    url: jdbc:mysql://localhost:3306/db02?serverTimezone=Asia/Shanghai&useSSL=false&useUncode=true&characterEncoding=utf-8
    username: root
    password: 123456

# Eureka的配置 服务注册到哪里
eureka:
  client:
    service-url:
      defaultZone: http://cyz7001:7001/eureka/,http://cyz7002:7002/eureka/,http://cyz7003:7003/eureka/
  instance:
    instance-id: springcloud-config-dept8001 # 描述信息

# info配置
info:
  app.name: com.cyz-springcloud
  company.name: chenyaoze666.github.io

---
server:
  port: 8001
# mybatis配置
mybatis:
  type-aliases-package: com.cyz.springcloud.pojo
  config-location: classpath:mybatis/mybatis-config.xml
  mapper-locations: classpath:mybatis/mapper/*.xml

# spring的配置
spring:
  profiles: test
  application:
    name: springcloud-config-dept
  datasource:
    type: com.alibaba.druid.pool.DruidDataSource
    driver-class-name: org.gjt.mm.mysql.Driver
    url: jdbc:mysql://localhost:3306/db02?serverTimezone=Asia/Shanghai&useSSL=false&useUncode=true&characterEncoding=utf-8
    username: root
    password: 123456

# Eureka的配置 服务注册到哪里
eureka:
  client:
    service-url:
      defaultZone: http://cyz7001:7001/eureka/,http://cyz7002:7002/eureka/,http://cyz7003:7003/eureka/
  instance:
    instance-id: springcloud-config-dept8001 # 描述信息

# info配置
info:
  app.name: com.cyz-springcloud
  company.name: chenyaoze666.github.io
```

* springcloud-config-eureka-7001
* 依赖

```xml
<dependencies>
        <!--        config-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-config</artifactId>
            <version>2.2.3.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
        </dependency>


        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <version>2.3.0.RELEASE</version>
        </dependency>
   </dependencies>
```

* bootstrap.yml

```yaml
spring:
  cloud:
    config:
      name: config-eureka
      label: master
      profile: dev
      uri: http://localhost:3344
```

* application.yml

```yaml
spring:
  application:
    name: springcloud-config-eureka-7001
```

* 启动类

```java
package com.cyz.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.server.EnableEurekaServer;

@SpringBootApplication
@EnableEurekaServer //服务端的启动类, 可以接受别人注册进来
public class EurekaServer_7001 {
    public static void main(String[] args) {
        SpringApplication.run(EurekaServer_7001.class,args);
    }
}

```

* 访问

```http
http://localhost:3344/config-eureka-dev.yml
http://localhost:7001/
```

* springcloud-config-dept-8001

* 依赖

```xml
 <dependencies>
        <!--        config-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-config</artifactId>
            <version>2.2.3.RELEASE</version>
        </dependency>
        <!--hystrix-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
            <version>2.2.3.RELEASE</version>
        </dependency>

        <!--eureka-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
            <version>2.2.3.RELEASE</version>
        </dependency>

        <!--        完善监控信息-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
            <version>2.3.0.RELEASE</version>
        </dependency>



        <!--        需要实体类  配置apimodule-->
        <dependency>
            <groupId>com.com.cyz</groupId>
            <artifactId>springcloud-api</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
        <!--        junit-->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
        </dependency>
        <dependency>
            <groupId>ch.qos.logback</groupId>
            <artifactId>logback-core</artifactId>
        </dependency>
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
        </dependency>

        <!--        test-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-test</artifactId>
            <version>2.3.0.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <version>2.3.0.RELEASE</version>
        </dependency>
        <!--jetty-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jetty</artifactId>
            <version>2.3.0.RELEASE</version>
        </dependency>
        <!--        热部署-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <version>2.3.0.RELEASE</version>
        </dependency>
    </dependencies>
```

* bootstrap.yml

```yaml
spring:
  cloud:
    config:
      name: config-dept
      label: master
      profile: dev
      uri: http://localhost:3344
```

* application.yml

```yaml
spring:
  application:
    name: springcloud-config-dept-8001
```

* 其他和springcloud-provider-dept-8001一样
* 注意Mapper.xml的数据库
* 访问

```http
http://localhost:3344/config-dept-dev.yml

http://localhost:8001/dept/get/1
```

* 只用修改远程仓库配置即可 配置和代码解耦



