---
   layout: post
   title: "MyBatis-Plus"                                                        
   date: 2020-06-15 09:00:00 +0530
   categories: JAVA MyBatis-Plus
---
  JAVA MyBatis-Plus


# MyBatis Plus

## 特点

```bash
# 无侵入：只做增强不做改变
# 强大的 CRUD 操作
# 内置代码生成器
# 内置分页插件
# 内置性能分析插件
```

## 使用

* 依赖

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter</artifactId>
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

    <!--mybatis-plus-->
    <dependency>
        <groupId>com.baomidou</groupId>
        <artifactId>mybatis-plus-boot-starter</artifactId>
        <version>3.0.5</version>
    </dependency>

    <!--mysql-->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
    </dependency>

    <!--lombok用来简化实体类-->
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
    </dependency>
</dependencies>
```

* 配置

```properties
#mysql数据库连接
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/mybatis_plus
spring.datasource.username=root
spring.datasource.password=123456

# mysql8+
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/mybatis_plus?serverTimezone=GMT%2B8
spring.datasource.username=root
spring.datasource.password=123456
```

* 在 Spring Boot 启动类中添加 `@MapperScan` 注解，扫描 Mapper 文件夹

```java
@SpringBootApplication
@MapperScan("com.atguigu.mybatisplus.mapper")
public class MybatisPlusApplication {
	......
}
```

* 实体类

```java
@Data
public class User {
    private Long id;
    private String name;
    private Integer age;
    private String email;
}
```

* Mapper 继承 BaseMapper<User>

```java
@Repository//可以解决 注入报错的问题 不加也没事
public interface UserMapper extends BaseMapper<User> {
    
}
```

## 测试简单使用

```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class MybatisPlusApplicationTests {

    @Autowired
    private UserMapper userMapper;//@Repository解决报错的问题 

    @Test
    public void testSelectList() {
        System.out.println(("----- selectAll method test ------"));
        //UserMapper 中的 selectList() 方法的参数为 MP 内置的条件封装器 Wrapper
        //所以不填写就是无任何条件
        List<User> users = userMapper.selectList(null);
        
        //配置逻辑删除后 查询语句
        //SELECT id,name,age,email,create_time,update_time,deleted FROM user WHERE deleted=0

        users.forEach(System.out::println);
    }
    
    @Test
    public void testInsert(){

        User user = new User();//id策略
        user.setName("Helen");
        user.setAge(18);
        user.setEmail("55317332@qq.com");

        int result = userMapper.insert(user);
        System.out.println(result); //影响的行数
        System.out.println(user); //id自动回填
    }
    
    
   @Test
    public void testUpdateById(){

        //根据id UPDATE user SET age=? WHERE id=? 
        User user = new User();
        user.setId(1L);
        user.setAge(28);

        int result = userMapper.updateById(user);
        System.out.println(result);

    }
    
    @Test
    public void testSelectById(){

        User user = userMapper.selectById(1L);
        System.out.println(user);
    }	
    
    @Test
    public void testSelectBatchIds(){

        List<User> users = userMapper.selectBatchIds(Arrays.asList(1, 2, 3));
        users.forEach(System.out::println);
    }	
    
    @Test
    public void testSelectByMap(){

        HashMap<String, Object> map = new HashMap<>();
        map.put("name", "Helen");
        map.put("age", 18);
        List<User> users = userMapper.selectByMap(map);

        users.forEach(System.out::println);
    }	
    
    
    @Test
    public void testDeleteById(){

        int result = userMapper.deleteById(8L);
        System.out.println(result);
    }
    
    @Test
    public void testDeleteBatchIds() {

        int result = userMapper.deleteBatchIds(Arrays.asList(8, 9, 10));
        System.out.println(result);
    }
    
    @Test
    public void testDeleteByMap() {

        HashMap<String, Object> map = new HashMap<>();
        map.put("name", "Helen");
        map.put("age", 18);

        int result = userMapper.deleteByMap(map);
        System.out.println(result);
    }

}
```

## 开启日志

```properties
#mybatis日志
mybatis-plus.configuration.log-impl=org.apache.ibatis.logging.stdout.StdOutImpl
```

## id策略

```java
为@TableId(type = IdType.AUTO) //主键自增  需要数据库设置自增
private Long id;

@TableId(type = IdType.NONE)   //空, 需用户输入
@TableId(type = IdType.INPUT) // 用户输入
@TableId(type = IdType.ID_WORKER) //默认策略 19位 Long
@TableId(type = IdType.ID_WORKER_STR)  //19位  String

@Getter
public enum IdType {
    /**
     * 数据库ID自增
     */
    AUTO(0),
    /**
     * 该类型为未设置主键类型
     */
    NONE(1),
    /**
     * 用户输入ID
     * 该类型可以通过自己注册自动填充插件进行填充
     */
    INPUT(2),

    /* 以下3种类型、只有当插入对象ID 为空，才自动填充。 */
    /**
     * 全局唯一ID (idWorker)
     */
    ID_WORKER(3),
    /**
     * 全局唯一ID (UUID)
     */
    UUID(4),
    /**
     * 字符串全局唯一ID (idWorker 的字符串表示)
     */
    ID_WORKER_STR(5);

    private int key;

    IdType(int key) {
        this.key = key;
    }
}



```

* 全局配置主键策略

```properties
#全局设置主键生成策略
mybatis-plus.global-config.db-config.id-type=auto
```

## 自动填充

* 注意点

```bash
# 支持的数据类型只有 int,Integer,long,Long,Date,Timestamp,LocalDateTime
# 整数类型下 newVersion = oldVersion + 1
# newVersion 会回写到 entity 中
# 仅支持 updateById(id) 与 update(entity, wrapper) 方法
# 在 update(entity, wrapper) 方法下, wrapper 不能复用!!!
```

```java
@Data
public class User {
    ......
        
    @TableField(fill = FieldFill.INSERT)
    private Date createTime;

    //@TableField(fill = FieldFill.UPDATE)
    @TableField(fill = FieldFill.INSERT_UPDATE)
    private Date updateTime;
}
```

* 需要在元对象处理器中配置

## 元对象处理器

```java
package com.cyz.mybatisplus.handler;
import com.baomidou.mybatisplus.core.handlers.MetaObjectHandler;
import org.apache.ibatis.reflection.MetaObject;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.stereotype.Component;
import java.util.Date;

@Component//交由spring
public class MyMetaObjectHandler implements MetaObjectHandler {

private static final Logger LOGGER = LoggerFactory.getLogger(MyMetaObjectHandler.class);

    @Override
    public void insertFill(MetaObject metaObject) {
        LOGGER.info("start insert fill ....");
        this.setFieldValByName("createTime", new Date(), metaObject);
        this.setFieldValByName("updateTime", new Date(), metaObject);
        
        //用于乐观锁
        this.setFieldValByName("version", 1, metaObject);
    }

    @Override
    public void updateFill(MetaObject metaObject) {
        LOGGER.info("start update fill ....");
        this.setFieldValByName("updateTime", new Date(), metaObject);
    }
}
```

## 乐观锁

* 实现方式(判断数据库中的version字段)

```bash
# 取出记录时，获取当前version
# 更新时，带上这个version
# 执行更新时， set version = newVersion where version = oldVersion
# 如果version不对，就更新失败
```

```java
@Version
@TableField(fill = FieldFill.INSERT)//在自动填充处理器添加 
private Integer version;
```

* 需要配置插件

## 统一配置插件

```java
package com.cyz.mybatisplus.config;

import com.baomidou.mybatisplus.extension.plugins.PaginationInterceptor;
import org.mybatis.spring.annotation.MapperScan;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.transaction.annotation.EnableTransactionManagement;

@EnableTransactionManagement
@Configuration
@MapperScan("com.cyz.mybatis_plus.mapper")
public class MybatisPlusConfig {

    /**
     * 乐观锁插件
     */
    @Bean
    public OptimisticLockerInterceptor optimisticLockerInterceptor() {
        return new OptimisticLockerInterceptor();
    }
}
```

## 分页

* 配置插件

```java
/**
 * 分页插件
 */
@Bean
public PaginationInterceptor paginationInterceptor() {
    return new PaginationInterceptor();
}
```

* 测试

```java
@Test
public void testSelectPage() {
    
    //SELECT id,name,age,email,create_time,update_time FROM user LIMIT 0,5 

    Page<User> page = new Page<>(1,5);
    userMapper.selectPage(page, null);

    page.getRecords().forEach(System.out::println);//结果集合
    System.out.println(page.getCurrent());//当前页
    System.out.println(page.getPages());//总页数
    System.out.println(page.getSize());//每页条数
    System.out.println(page.getTotal());//总条数
    System.out.println(page.hasNext());//是否有下一页
    System.out.println(page.hasPrevious());//是否有上一页
}
```

## 逻辑删除

* 数据库中增加 deleted 字段
* 实体类

```java
@TableLogic
@TableField(fill = FieldFill.INSERT)
private Integer deleted;
```

* 元对象处理器中配置

```java
@Override
public void insertFill(MetaObject metaObject) {
    ......
    this.setFieldValByName("deleted", 0, metaObject);
}
```

* 配置

```properties
# 默认配置 相同可不配置
mybatis-plus.global-config.db-config.logic-delete-value=1
mybatis-plus.global-config.db-config.logic-not-delete-value=0
```

* 插件

```java
@Bean
public ISqlInjector sqlInjector() {
    return new LogicSqlInjector();
}
```

* 测试

```java
/**
 * 测试 逻辑删除   实际上是修改操作
 */
@Test
public void testLogicDelete() {

    int result = userMapper.deleteById(1L);
    System.out.println(result);
}
```

## 性能分析

* 配置插件

```java
/**
 * SQL 执行性能分析插件
 * 开发环境使用，线上不推荐。 maxTime 指的是 sql 最大执行时长
 * format: SQL是否格式化
 */
@Bean
@Profile({"dev","test"})// 设置 dev test 环境开启
public PerformanceInterceptor performanceInterceptor() {
    PerformanceInterceptor performanceInterceptor = new PerformanceInterceptor();
    performanceInterceptor.setMaxTime(100);//ms，超过此处设置的ms则sql不执行
    performanceInterceptor.setFormat(true);
    return performanceInterceptor;
}
```

* 配置

```properties
#环境设置：dev、test、prod
spring.profiles.active=dev
```

* 测试

```java
/**
 * 测试 性能分析插件
 */
@Test
public void testPerformance() {
    User user = new User();
    user.setName("我是Helen");
    user.setEmail("helen@sina.com");
    user.setAge(18);
    userMapper.insert(user);
}
```

```bash
# 符合性能 输出时间 + sql 语句

# 不符合性能 输出 error (time is too large)
```

## Wrapper

* 使用子类QueryWrapper构造条件

* 测试

```java
//mp实现复杂查询操作
    @Test
    void testSelectQuery() {
        //创建QueryWrapper对象
        QueryWrapper<User> wrapper = new QueryWrapper<>();

        //通过QueryWrapper设置条件
        //ge、gt、le、lt  >=, >, <=, <
        //查询age>=30记录
        //第一个参数字段名称，第二个参数设置值
//        wrapper.ge("age",30);

        //eq、ne  =,!=或<>
//        wrapper.eq("name","cyz");
//        wrapper.ne("name","cyz");

        //between
        //查询年龄 20-30
//         wrapper.between("age",20,30);

        //like
//        wrapper.like("name","c");

        //orderByDesc
//         wrapper.orderByDesc("id");

        //last  向拼接sql
//        wrapper.last("limit 1");

        //指定要查询的列
        wrapper.select("id","name");

        List<User> users = userMapper.selectList(wrapper);
        System.out.println(users);

    }
```