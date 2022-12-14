---
title: 瑞吉外卖
date: 2022-10-11
tags: 项目
categories: 项目
description: 外卖项目
---
# 	软件开发

**软件开发流程**

- 需求分析：产品原型，需求规格说明书
- 设计：产品文档，UI界面设计，概要设计，详细设计，数据库设计
- 编码：项目代码，单元测试
- 测试：测试用例，测试报告
- 上线运维：软件环境安装，配置

**角色分工**

- 项目经理：对整个项目负责，任务分配，进度把控
- 产品经理：进行需求调研，输出需求调研文档，产品原型
- UI设计师：根据产品原型输出界面效果图
- 架构师：项目整体架构设计，技术选型
- 开发工程师：代码实现
- 测试工程师：编写测试用例，输出测试报告
- 运维工程师：软件环境搭建，项目上线

**软件环境**

- 开发环境：开发人员在开发阶段使用的环境，一般外部用户无法访问
- 测试环境：专门给测试人员使用的环境，用于测试项目，一般外部用户无法访问
- 生产环境：线上环境，正式提供对外服务的环境

# **项目介绍**

项目包括系统管理后台和移动端应用两部分，管理后台主要提供给餐饮企业内部员工使用，可以对餐厅菜品，套餐订单等进行管理维护，移动端应用主要提供给消费者使用，可以在线浏览菜品，添加购物车，下单。

**技术选型**

- 用户层
  - H5
  - VUE.js
  - Element UI
  - 微信小程序
- 网关层
  - Nginx
- 应用层
  - Spring Boot
  - Spring MVC
  - Spring Session
  - Spring
  - Swagger
  - lombok
- 数据层
  - Mysql
  - Mybatis
  - Mybatis Plus
  - Redis
- 工具
  - git
  - maven
  - junit

**功能构架**

- 移动端前台
  - 手机号登录
  - 微信登录
  - 地址管理
  - 历史订单
  - 菜品规格
  - 购物车
  - 下单
  - 菜品浏览
- 系统管理后台
  - 分类管理
  - 菜品管理
  - 套餐管理
  - 菜品口味管理
  - 员工登录
  - 员工退出
  - 员工管理
  - 订单管理

**角色**

- 后台系统管理员：拥有后台系统中所有操作权限
- 后台系统普通员工：对菜品，套餐，订单等进行管理
- C端用户：登录移动端，可以浏览菜品等

# 通过git管理代码

- 添加分支v1.0
- 提交到仓库
- 在v1.0分支进行开发，确认无误后和master进行合并
- 合并：切换为master选择要合并的分支进行marge

# 开发环境搭建

## **数据库环境搭建**

1. 员工表
2. 菜品和套餐分类表
3. 菜品表
4. 套餐表
5. 套餐菜品关系表
6. 菜品口味关系表
7. 用户表
8. 地址表
9. 购物车表
10. 订单表
11. 订单明细表

## **Maven依赖**

- spring-boot-starter-web(创建spring boot时勾选)

- spring-boot-starter-test

- mybatis-plus-boot-starter

- lombok

- fastjson

- commons-lang

- druid-spring-boot-starter

- mysql-connector-java

- aliyun-java-sdk-core

- aliyun-java-sdk-dysmsapi

- spring-boot-starter-data-redis

- spring-boot-starter-cache

- sharding-jdbc-spring-boot-starter

- knife4j-spring-boot-starter

```xml
 <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>

        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>3.4.2</version>
        </dependency>

        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.24</version>
        </dependency>

        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>fastjson</artifactId>
            <version>1.2.62</version>
        </dependency>

        <dependency>
            <groupId>commons-lang</groupId>
            <artifactId>commons-lang</artifactId>
            <version>2.6</version>
        </dependency>

        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid-spring-boot-starter</artifactId>
            <version>1.2.13</version>
        </dependency>

        <dependency>
            <groupId>com.mysql</groupId>
            <artifactId>mysql-connector-j</artifactId>
            <scope>runtime</scope>
        </dependency>

        <!--阿里云短信服务-->
        <dependency>
            <groupId>com.aliyun</groupId>
            <artifactId>aliyun-java-sdk-core</artifactId>
            <version>4.5.16</version>
        </dependency>

        <dependency>
            <groupId>com.aliyun</groupId>
            <artifactId>aliyun-java-sdk-dysmsapi</artifactId>
            <version>2.1.0</version>
        </dependency>
     
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-cache</artifactId>
        </dependency>
        <dependency>
            <groupId>org.apache.shardingsphere</groupId>
            <artifactId>sharding-jdbc-spring-boot-starter</artifactId>
            <version>4.0.0-RC1</version>
        </dependency>
        <dependency>
            <groupId>com.github.xiaoymin</groupId>
            <artifactId>knife4j-spring-boot-starter</artifactId>
            <version>3.0.2</version>
        </dependency>
    </dependencies>
```

## **编辑配置文件**

```yaml
spring:
  profiles:
    active: pro
```

### 开发环境

```yaml
#开发环境
server:
  port: 8080

#应用名称
spring:
  application:
    name: ttwm
  datasource:
    druid:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/demo?serverTimezone=Asia/Shanghai&useUnicode=true&characterEncoding=UTF-8 #???????
    username: root
    password: 1234
    
  #Redis配置
  redis:
    host: 104.243.21.3
    port: 6379
    password: 123456
    database: 0 #Redis默认提供16个数据库，默认使用0号数据库
    jedis:
      #Redis连接池配置
      pool:
        max-active: 8 #最大连接数设置
        max-wait: 1ms #连接池最大阻塞等待时间
        max-idle: 8 #连接池中的最大空闲连接，最好和max-active设置的值相同
        min-idle: 0 #连接池中的最小空闲连接
  cache:
    redis:
      time-to-time: 1800000 #设置缓存过期时间，单位毫秒

  shardingsphere:
    datasource:
      names:
        master,slave #数据源，名字无所谓
      #主数据源
      master: #和上边名称对应
        type: com.alibaba.druid.pool.DuidDataSource
        driver-class-name: com.mysql.cj.jdbc.Driver
        url: jdbc:mysql://ip:3306/数据库名?cahracterEncoding=utf-8
        username: root
        password: xxxx
      #从数据源
      slave:
        type: com.alibaba.druid.pool.DuidDataSource
        driver-class-name: com.mysql.cj.jdbc.Driver
        url: jdbc:mysql://ip:3306/数据库名?cahracterEncoding=utf-8
        username: root
        password: xxxx
    masterslave:
      #读写分离配置
      #从库负载均衡策略，从库可能有多个，此项配置决定从库处理数据库查询的顺序，round_robin：按顺序进行
      load-balance-algorithm-type: round_robin
      #最终的数据源名称
      name: dataSource
      #主库数据源名称
      master-data-source: master
      #从库数据源名称列表，逗号分隔
      slave-data-source-names: slave
    props:
      sql:
        show: true #开启SQL显示，默认为false
    #在配置文件中允许bean定义覆盖配置顶
    main:
      allow-bean-definition-overriding: true #

mybatis-plus:
  configuration:
    map-underscore-to-camel-case: true #开启驼峰命名模式, 例如:address_book—> addressBook
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl #开启日志

  #所有id生成策略为雪花算法
  global-config:
    db-config:
      id-type: ASSIGN-ID

#自定义配置，指定文件的转存路径
filePath:
  path: D:\img\
```

### 生产环境

```yaml
#生产环境
server:
  port: 1022

#应用名称
spring:
  application:
    name: ttwm
    
  datasource:
    druid:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/demo?serverTimezone=Asia/Shanghai&useUnicode=true&characterEncoding=UTF-8 #???????
    username: root
    password: 1234

  #Redis配置
  redis:
    host: 104.243.21.3
    port: 6379
    password: 123456
    database: 0 #Redis默认提供16个数据库，默认使用0号数据库
    jedis:
      #Redis连接池配置
      pool:
        max-active: 8 #最大连接数设置
        max-wait: 1ms #连接池最大阻塞等待时间
        max-idle: 8 #连接池中的最大空闲连接，最好和max-active设置的值相同
        min-idle: 0 #连接池中的最小空闲连接
  cache:
    redis:
      time-to-time: 1800000 #设置缓存过期时间，单位毫秒

  shardingsphere:
    datasource:
      names:
        master,slave #数据源，名字无所谓
      #主数据源
      master: #和上边名称对应
        type: com.alibaba.druid.pool.DuidDataSource
        driver-class-name: com.mysql.cj.jdbc.Driver
        url: jdbc:mysql://ip:3306/数据库名?cahracterEncoding=utf-8
        username: root
        password: xxxx
      #从数据源
      slave:
        type: com.alibaba.druid.pool.DuidDataSource
        driver-class-name: com.mysql.cj.jdbc.Driver
        url: jdbc:mysql://ip:3306/数据库名?cahracterEncoding=utf-8
        username: root
        password: xxxx
    masterslave:
      #读写分离配置
      #从库负载均衡策略，从库可能有多个，此项配置决定从库处理数据库查询的顺序，round_robin：按顺序进行
      load-balance-algorithm-type: round_robin
      #最终的数据源名称
      name: dataSource
      #主库数据源名称
      master-data-source: master
      #从库数据源名称列表，逗号分隔
      slave-data-source-names: slave
    props:
      sql:
        show: true #开启SQL显示，默认为false
    #在配置文件中允许bean定义覆盖配置顶
    main:
      allow-bean-definition-overriding: true #

mybatis-plus:
  configuration:
    map-underscore-to-camel-case: true #开启驼峰命名模式, 例如:address_book—> addressBook
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl #开启日志

  #所有id生成策略为雪花算法
  global-config:
    db-config:
      id-type: ASSIGN-ID

#自定义配置，指定文件的转存路径
filePath:
  path: D:\img\
```

# 代码

## 启动类

```java
@Slf4j
@SpringBootApplication
/**
 * SpringBootApplication上使用@ServletComponentScan注解后
 * Servlet可以直接通过@WebServlet注解自动注册
 * Filter可以直接通过@WebFilter注解自动注册
 * Listener可以直接通过@WebListener 注解自动注册
 */
@ServletComponentScan
@EnableTransactionManagement//开启事务支持
@EnableCaching//开启缓存
public class TtApplication {

    public static void main(String[] args) {
        SpringApplication.run(TtApplication.class, args);
        log.info("项目启动成功");
    }
}
```



## 定义WebMvc配置类

### WebMvcConfig

通过重写addResourceHandlers方法来映射静态资源目录

1. 编写WebMvcConfig类继承WebMvcConfigurerAdapter类
2. 重写该类的addResourceHandlers方法
3. addResourceHandler指向映射路径，就是通过浏览器访问时输入的地址，如/backend/index.html
4. addResourceLocations指向资源文件路径，也就是在磁盘中的真实地址，资源文件路径地址必须以/结尾

```java
@Slf4j
@Configuration
public class WebMvcConfig extends WebMvcConfigurationSupport {

    /**
     * 设置静态资源映射
     * @param registry
     */
    //
    @Override
    protected void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("/backend/**").addResourceLocations("classpath:/backend/");
        registry.addResourceHandler("/front/**").addResourceLocations("classpath:/front/");
        //访问接口文档时需要请求doc.html，doc.html是框架已经写好的
        registry.addResourceHandler("doc.html").addResourceLocations("classpath:/META-    INF/resources/");
        registry.addResourceHandler("/webjars/**").addResourceLocations("classpath:/META-INF/resources/webjars/");
    }
}
```

Js对long型数据处理时会丢失精度，导致发送的id和数据库中不一致，根据id修改信息会报错，解决步骤：

1. 提供对象转换器JacksonObjectMapper，基于Jackson进行Java对象到json数据的转换
2. 在WebMvcConfig配置类中创建新的消息转换器，在消息转换器中设置对象转换器，将消息转换器加入转换器容器

**扩展WebMvc配置类**

```java
/**
 * 通过重写addResourceHandlers方法来映射静态资源目录
 * 1. 编写WebMvcConfig类继承WebMvcConfigurerAdapter类
 * 2. 重写该类的addResourceHandlers方法
 * 3. addResourceHandler指向映射路径，就是通过浏览器访问时输入的地址，如/backend/index.html
 * 4. addResourceLocations指向资源文件路径，也就是在磁盘中的真实地址，资源文件路径地址必须以/结尾
 */
@Slf4j
@Configuration
public class WebMvcConfig extends WebMvcConfigurationSupport {

    /**
     * 设置静态资源映射
     * @param registry
     */
    //重写addResourceHandlers方法
    @Override
    protected void addResourceHandlers(ResourceHandlerRegistry registry) {
        //addResourceHandler指向映射路径，就是通过浏览器访问时输入的地址，如/backend/index.html
        registry.addResourceHandler("/backend/**").addResourceLocations("classpath:/backend/");
        //addResourceLocations指向资源文件路径，也就是在磁盘中的真实地址，资源文件路径地址必须以/结尾
        registry.addResourceHandler("/front/**").addResourceLocations("classpath:/front/");
        //访问接口文档时需要请求doc.html，doc.html是框架已经写好的
        registry.addResourceHandler("doc.html").addResourceLocations("classpath:/META-    INF/resources/");
        registry.addResourceHandler("/webjars/**").addResourceLocations("classpath:/META-INF/resources/webjars/");
    }

    /**
     * 扩展MVC框架的消息转换器
     * @param converters
     */
    //重写extendMessageConverters方法
    @Override
    protected void extendMessageConverters(List<HttpMessageConverter<?>> converters) {
        //创建消息转换器对象
        MappingJackson2HttpMessageConverter messageConverter = new MappingJackson2HttpMessageConverter();

        //设置对象转换器，底层使用Jackson将Java转为json
        messageConverter.setObjectMapper(new JacksonObjectMapper());

        //将上面的消息转换器对象追加到mvc框架的转换器容器，0代表顺序，会优先使用该转换器
        converters.add(0,messageConverter);
    }
   
    /**
     * 创建Docket对象
     * @return
     */
    @Bean
    public Docket createRestApi(){
        //文档类型
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                .select()
                //controller包所在位置
                .apis(RequestHandlerSelectors.basePackage("com.zxp.controller"))
                .paths(PathSelectors.any())
                .build();
    }

    /**
     * 接口文档的描述
     * @return
     */
    private ApiInfo apiInfo(){
        return new ApiInfoBuilder()
                .title("tt外卖")
                .version("1.0")
                .description("外卖接口文档")
                .build();
    }
```



## 定义Redis配置类

### RedisConfig

```java
/**
 * "\xac\xed\x00\x05t\x00\x03age"默认序列化器
 * 设置新的序列化器后显示为age
 * value不需要新的序列化器，因为反序列化后会恢复
 * 即使自己不创建RedisTemplate，框架也会创建
 * 自己创建是为了修改key序列化器，创建后框架不再创建
 */
@Configuration
public class RedisConfig extends CachingConfigurerSupport {

    //自动装配连接工厂
    @Bean
    public RedisTemplate<Object, Object> redisTemplate(RedisConnectionFactory connectionFactory) {

        RedisTemplate<Object, Object> redisTemplate = new RedisTemplate<>();

        //默认的Key序列化器为：JdkSerializationRedisSerializer
        redisTemplate.setKeySerializer(new StringRedisSerializer());//设置新的序列化器
        redisTemplate.setHashKeySerializer(new StringRedisSerializer());//设置hash的key的序列化器

        redisTemplate.setConnectionFactory(connectionFactory);//设置连接工厂
        return redisTemplate;
    }
}
```



## 定义对象转换器

### JacksonObjectMapper

```java
/**
 * 对象映射器:基于jackson将Java对象转为json，或者将json转为Java对象
 * 将JSON解析为Java对象的过程称为 [从JSON反序列化Java对象]
 * 从Java对象生成JSON的过程称为 [序列化Java对象到JSON]
 */
public class JacksonObjectMapper extends ObjectMapper {

    public static final String DEFAULT_DATE_FORMAT = "yyyy-MM-dd";
    public static final String DEFAULT_DATE_TIME_FORMAT = "yyyy-MM-dd HH:mm:ss";
    public static final String DEFAULT_TIME_FORMAT = "HH:mm:ss";

    public JacksonObjectMapper() {
        super();
        //收到未知属性时不报异常
        this.configure(FAIL_ON_UNKNOWN_PROPERTIES, false);

        //反序列化时，属性不存在的兼容处理
        this.getDeserializationConfig().withoutFeatures
            (DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES);

        SimpleModule simpleModule = new SimpleModule()
                .addDeserializer(LocalDateTime.class, new LocalDateTimeDeserializer(DateTimeFormatter.ofPattern(DEFAULT_DATE_TIME_FORMAT)))
                .addDeserializer(LocalDate.class, new LocalDateDeserializer(DateTimeFormatter.ofPattern(DEFAULT_DATE_FORMAT)))
                .addDeserializer(LocalTime.class, new LocalTimeDeserializer(DateTimeFormatter.ofPattern(DEFAULT_TIME_FORMAT)))

                .addSerializer(BigInteger.class, ToStringSerializer.instance)
                //将long型数据转为字符串
                .addSerializer(Long.class, ToStringSerializer.instance)
                //将时间类型转为字符串
                .addSerializer(LocalDateTime.class, new LocalDateTimeSerializer(DateTimeFormatter.ofPattern(DEFAULT_DATE_TIME_FORMAT)))
                .addSerializer(LocalDate.class, new LocalDateSerializer(DateTimeFormatter.ofPattern(DEFAULT_DATE_FORMAT)))
                .addSerializer(LocalTime.class, new LocalTimeSerializer(DateTimeFormatter.ofPattern(DEFAULT_TIME_FORMAT)));

        //注册功能模块 例如，可以添加自定义序列化器和反序列化器
        this.registerModule(simpleModule);
    }
}
```



## 定义实体类（pojo）

### 员工类

```java
/**
 * 创建员工类
 */
@Data //包含get，set，toString,equals和hashcode方法
public class Employee {
    
    private static final long serialVersionUID = 1L;
    
    //使用引用数据类型
    private Long id;
    private String name;
    private String username;
    //@TableField(select = false)
    private String password;
    private String phone;
    private String sex;
    private String idNumber;
    private Integer status;
    //在实体类中的属性上加入@TableField注解，指定自动填充的策略
    @TableField(fill = FieldFill.INSERT)
    private LocalDateTime createTime;
    @TableField(fill = FieldFill.INSERT_UPDATE)
    private LocalDateTime updateTime;
    @TableField(fill = FieldFill.INSERT)
    private Long createUser;
    @TableField(fill = FieldFill.INSERT_UPDATE)
    private Long updateUser;
}
```



### 分类类

```java
/**
 * 菜品和套餐分类
 */
@Data
public class Category{
    
    private static final long serialVersionUID = 1L;
    
    private Long id;
    private int type; //1菜品分类，2套餐分类
    private String name; //分类名称
    private Integer sort; //排序属性
    //在实体类中的属性上加入@TableField注解，指定自动填充的策略
    @TableField(fill = FieldFill.INSERT)
    private LocalDateTime createTime;
    @TableField(fill = FieldFill.INSERT_UPDATE)
    private LocalDateTime updateTime;
    @TableField(fill = FieldFill.INSERT)
    private Long createUser;
    @TableField(fill = FieldFill.INSERT_UPDATE)
    private Long updateUser;
}
```



### 菜品类

```java
/**
 * 菜品类
 */
@Data
//要缓存的 Java 对象必须实现 Serializable 接口，因为 Spring 会将对象先序列化再存入 Redis
public class Dish implements Serializable {
    private static final long serialVersionUID = 1L;

    private Long id;
    //菜品名称
    private String name;
    //菜品分类id
    private Long categoryId;
    //菜品价格，BigDecimal：解决浮点型运算精度失真的问题，单位分
    private BigDecimal price;
    //商品码
    private String code;
    //图片
    private String img;
    //描述信息
    private String description;
    //0 停售 1 起售
    private Integer status;
    //顺序
    private Integer sort;

    @TableField(fill = FieldFill.INSERT)
    private LocalDateTime createTime;

    @TableField(fill = FieldFill.INSERT_UPDATE)
    private LocalDateTime updateTime;

    @TableField(fill = FieldFill.INSERT)
    private Long createUser;

    @TableField(fill = FieldFill.INSERT_UPDATE)
    private Long updateUser;

    //是否删除
    private Integer isDeleted;
}
```



### 套餐类

```java
/**
 * 套餐
 */
@Data
//要缓存的 Java 对象必须实现 Serializable 接口，因为 Spring 会将对象先序列化再存入 Redis
public class Setmeal implements Serializable {

    private static final long serialVersionUID = 1L;

    private Long id;

    //分类id
    private Long categoryId;
    //套餐名称
    private String name;
    //套餐价格,BigDecimal：解决浮点型运算精度失真的问题
    private BigDecimal price;
    //状态 0:停用 1:启用
    private Integer status;
    //编码
    private String code;
    //描述信息
    private String description;
    //图片
    private String image;

    @TableField(fill = FieldFill.INSERT)
    private LocalDateTime createTime;

    @TableField(fill = FieldFill.INSERT_UPDATE)
    private LocalDateTime updateTime;

    @TableField(fill = FieldFill.INSERT)
    private Long createUser;

    @TableField(fill = FieldFill.INSERT_UPDATE)
    private Long updateUser;

    //是否删除
    private Integer isDeleted;
}
```



### 菜品口味类

```java
/**
 菜品口味
 */
@Data
public class DishFlavor implements Serializable {

    private static final long serialVersionUID = 1L;

    private Long id;


    //菜品id
    private Long dishId;


    //口味名称
    private String name;


    //口味数据list
    private String value;


    @TableField(fill = FieldFill.INSERT)
    private LocalDateTime createTime;


    @TableField(fill = FieldFill.INSERT_UPDATE)
    private LocalDateTime updateTime;


    @TableField(fill = FieldFill.INSERT)
    private Long createUser;


    @TableField(fill = FieldFill.INSERT_UPDATE)
    private Long updateUser;


    //是否删除
    private Integer isDeleted;

}
```



### 套餐菜品关系类

```java
/**
 * 套餐菜品关系表
 */
@Data
public class SetmealDish {
    
    private static final long serialVersionUID = 1L;
    
    private Long id;

    //套餐id
    private Long setmealId;

    //菜品id
    private Long dishId;

    //菜品名称
    private String name;

    //价格
    private BigDecimal price;

    //份数
    private Integer copies;

    //顺序
    private Integer sort;

    @TableField(fill = FieldFill.INSERT)
    private LocalDateTime createTime;

    @TableField(fill = FieldFill.INSERT_UPDATE)
    private LocalDateTime updateTime;

    @TableField(fill = FieldFill.INSERT)
    private Long createUser;

    @TableField(fill = FieldFill.INSERT_UPDATE)
    private Long updateUser;

    //是否删除
    private Integer isDeleted;

}
```



### 用户类

```java
/**
 * 用户信息
 */
@Data
public class User implements Serializable {

    private static final long serialVersionUID = 1L;

    private Long id;

    //姓名
    private String name;

    //手机号
    private String phone;

    //性别 0 女 1 男
    private String sex;

    //身份证号
    private String idNumber;

    //头像
    private String avatar;

    //状态 0:禁用，1:正常
    private Integer status;
}
```



### 地址簿

```java
/**
 * 地址簿
 */
@Data
public class AddressBook implements Serializable {

    private static final long serialVersionUID = 1L;

    private Long id;

    //用户id
    private Long userId;

    //收货人
    private String consignee;

    //手机号
    private String phone;

    //性别 0 女 1 男
    private String sex;

    //省级区划编号
    private String provinceCode;

    //省级名称
    private String provinceName;

    //市级区划编号
    private String cityCode;

    //市级名称
    private String cityName;

    //区级区划编号
    private String districtCode;

    //区级名称
    private String districtName;

    //详细地址
    private String detail;

    //标签
    private String label;

    //是否默认 0 否 1是
    private Integer isDefault;

    //创建时间
    @TableField(fill = FieldFill.INSERT)
    private LocalDateTime createTime;

    //更新时间
    @TableField(fill = FieldFill.INSERT_UPDATE)
    private LocalDateTime updateTime;

    //创建人
    @TableField(fill = FieldFill.INSERT)
    private Long createUser;

    //修改人
    @TableField(fill = FieldFill.INSERT_UPDATE)
    private Long updateUser;

    //是否删除
    private Integer isDeleted;
}
```



### 购物车

```java
/**
 * 购物车
 */
@Data
public class ShoppingCart implements Serializable {

    private static final long serialVersionUID = 1L;

    private Long id;

    //名称
    private String name;

    //用户id
    private Long userId;

    //菜品id
    private Long dishId;

    //套餐id
    private Long setmealId;

    //口味
    private String dishFlavor;

    //数量
    private Integer number;

    //金额
    private BigDecimal amount;

    //图片
    private String image;

    private LocalDateTime createTime;
}
```



订单

```java
/**
 * 订单
 */
@Data
public class Orders implements Serializable {

    private static final long serialVersionUID = 1L;

    private Long id;

    //订单号
    private String number;

    //订单状态 1待付款，2待派送，3已派送，4已完成，5已取消
    private Integer status;

    //下单用户id
    private Long userId;

    //地址id
    private Long addressBookId;

    //下单时间
    private LocalDateTime orderTime;

    //结账时间
    private LocalDateTime checkoutTime;

    //支付方式 1微信，2支付宝
    private Integer payMethod;

    //实收金额
    private BigDecimal amount;

    //备注
    private String remark;

    //用户名
    private String userName;

    //手机号
    private String phone;

    //地址
    private String address;

    //收货人
    private String consignee;
}
```



### 订单明细

```java
/**
 * 订单明细
 */
@Data
public class OrderDetail implements Serializable {

    private static final long serialVersionUID = 1L;

    private Long id;

    //名称
    private String name;

    //订单id
    private Long orderId;

    //菜品id
    private Long dishId;

    //套餐id
    private Long setmealId;

    //口味
    private String dishFlavor;
    
    //数量
    private Integer number;

    //金额
    private BigDecimal amount;

    //图片
    private String image;
}
```



## 定义服务端返回结果类

### R

主要是为了方便前后端之间的信息交互

```java
@Data
//要缓存的 Java 对象必须实现 Serializable 接口，因为 Spring 会将对象先序列化再存入 Redis
public class R<T> implements Serializable{
    private Integer code;//编码：1成功，0和其他数字为失败
    private String msg;//错误信息
    private T data;//数据
    private Map map = new HashMap();//动态数据

    public static <T> R<T> success(T object){
        R<T> r = new R<T>();
        r.data = object;
        r.code = 1;
        return r;
    }

    public static <T> R<T> error(String msg) {
        R r = new R();
        r.msg = msg;
        r.code = 0;
        return r;
    }

    public R<T> add(String key, Object value) {
        this.map.put(key, value);
        return this;
    }

}
```



## 定义mapper类

### EmployeeMapper

```java
//@Mapper是mybatis自身带的注解。在spring程序中，mybatis需要找到对应的mapper，在编译时生成动态代理类，与数据库进行交互，这时需要用到@Mapper注解
@Mapper
//使用mybatis-plus要先继承BaseMapper接口
public interface EmployeeMapper extends BaseMapper<Employee>{
}
```



### CategoryMapper

```java
@Mapper
public interface CategoryMapper extends BaseMapper<Category> {
}
```



### DishMapper

```java
@Mapper
public interface DishMapper extends BaseMapper<Dish> {
}
```



### SetmealMapper

```java
@Mapper
public interface SetmealMapper extends BaseMapper<Setmeal> {
}
```



### DishFlavorMapper

```java
@Mapper
public interface DishFlavorMapper extends BaseMapper<DishFlavor> {
}
```



### SetmealDishMapper

```java
@Mapper
public interface SetmealDishMapper extends BaseMapper<SetmealDish> {
}
```



### UserMapper

```java
@Mapper
public interface UserMapper extends BaseMapper<User> {
}
```



### AddressBookMapper

```java
@Mapper
public interface AddressBookMapper extends BaseMapper<AddressBook> {
}
```



### ShoppingCartMapper

```java
@Mapper
public interface ShoppingCartMapper extends BaseMapper<ShoppingCart> {
}
```



### OrderMapper

```java
@Mapper
public interface OrderMapper extends BaseMapper<Orders> {
}
```



### OrderDetailMapper

```java
@Mapper
public interface OrderDetailMapper extends BaseMapper<OrderDetail> {
}
```



## 定义service类

### EmployeeService

```java
//@Service注解放在接口上没有意义
public interface EmployeeService extends IService<Employee> {
}
```

```java
@Service
public class EmployeeServiceImpl extends ServiceImpl<EmployeeMapper, Employee> implements EmployeeService{
}
```



### CategoryService

```java
public interface CategoryService extends IService<Category>{
    //实现类重写该方法
    public void remove(Long id);
}
```

```java
@Service
public class CategoryServiceImpl extends ServiceImpl<CategoryMapper, Category> implements CategoryService {

    @Autowired
    private DishService dishService;
    @Autowired
    private SetmealService setmealService;

    /**
     * 根据id删除分类，删除之前需要进行判断
     * @param id
     */
    @Override
    public void remove(Long id) {
        //查询当前分类是否关联菜品，如果已经关联，抛出一个异常
        LambdaQueryWrapper<Dish> dishLQW = new LambdaQueryWrapper<>();
        //添加查询条件，根据分类id进行查询
        dishLQW.eq(Dish::getCategoryId,id);
        int count1 = dishService.count(dishLQW);//根据Wrapper条件，查询总记录数,int count(Wrapper<T> queryWrapper);
        if (count1 > 0){
            //已经关联菜品，抛出一个业务异常
            throw new CustomException("当前分类下关联了菜品,不能删除");
        }

        //查询当前分类是否关联套餐，如果已经关联，抛出一个异常
        LambdaQueryWrapper<Setmeal> setmealLQW = new LambdaQueryWrapper<>();
        setmealLQW.eq(Setmeal::getCategoryId,id);
        int count2 = setmealService.count(setmealLQW);
        if (count2 > 0){
            //已经关联套餐，抛出一个业务异常
            throw new CustomException("当前分类下关联了套餐，不能删除");
        }

        //如果都没关联，调用父类IService的removeById方法正常删除
        super.remove(id);
    }
}
```



### DishService

```java
public interface DishService extends IService<Dish> {
    //新增菜品，同时插入菜品对应的口味数据，需要操作两张表：dish和dish_flavor
    void saveWithFlavor(DishDto dishDto);

    //查询菜品和对应的口味信息，需要查询两张表dish和dish_flavor
    DishDto getByIdWithFlavor(Long id);

    //修改菜品和对应的口味信息
    void updateByIdWithFlavor(DishDto dishDto);

    //删除菜品和对应的口味信息
    void deleteWithFlavor(List<Long> ids);
}
```

```java
@Service
public class DishServiceImpl extends ServiceImpl<DishMapper, Dish> implements DishService {

    @Autowired
    private DishFlavorService dishFlavorService;

    /**
     * 新增菜品，同时保存菜品对应的口味数据
     *
     * @param dishDto
     */
    @Transactional//涉及到多张表操作，因此要添加事务，需要在启动类开启事务支持
    public void saveWithFlavor(DishDto dishDto) {

        //保存菜品的基本信息到菜品表dish，DishDto继承自Dish，因此可以直接添加
        this.save(dishDto);

        //保存完菜品信息后，会自动生成DishId
        Long dishId = dishDto.getId();

        //获取菜品口味集合,并添加DishId
        List<DishFlavor> flavors = dishDto.getFlavors();
        for (DishFlavor flavor : flavors) {
            flavor.setDishId(dishId);
        }

        //保存菜品口味信息到dish_flavor
        //saveBatch:批量新增数据
        dishFlavorService.saveBatch(flavors);
    }

    /**
     * 查询菜品和对应的口味信息
     *
     * @param id
     */
    public DishDto getByIdWithFlavor(Long id) {
        //查询菜品基本信息，从dish表查询
        Dish dish = this.getById(id);

        //拷贝dish属性到dishDto
        DishDto dishDto = new DishDto();
        BeanUtils.copyProperties(dish, dishDto);

        //查询当前菜品对应的口味信息
        LambdaQueryWrapper<DishFlavor> queryWrapper = new LambdaQueryWrapper<>();
        queryWrapper.eq(DishFlavor::getDishId, id);
        List<DishFlavor> flavors = dishFlavorService.list(queryWrapper);

        //设置口味信息
        dishDto.setFlavors(flavors);
        return dishDto;
    }

    /**
     * 修改菜品和对应的口味信息
     *
     * @param dishDto
     */
    @Transactional
    public void updateByIdWithFlavor(DishDto dishDto) {
        //更新菜品信息
        this.updateById(dishDto);

        //清理当前菜品对应的口味数据
        LambdaQueryWrapper<DishFlavor> queryWrapper = new LambdaQueryWrapper<>();
        queryWrapper.eq(DishFlavor::getDishId, dishDto.getId());
        dishFlavorService.remove(queryWrapper);

        //设置口味数据对应的菜品id，并把口味信息添加到表中
        List<DishFlavor> flavors = dishDto.getFlavors();
        for (DishFlavor dishFlavor : flavors) {
            dishFlavor.setDishId(dishDto.getId());
        }
        dishFlavorService.saveBatch(flavors);
    }
    
     /**
     * 删除菜品和对应口味信息
     * @param ids
     */
    @Transactional
    public void deleteWithFlavor(List<Long> ids) {

        //判断菜品是否处于在售状态，如果在售则不能删除
        LambdaQueryWrapper<Dish> queryWrapper = new LambdaQueryWrapper<>();
        queryWrapper.in(Dish::getId,ids);
        queryWrapper.eq(Dish::getStatus,1);

        int count = this.count(queryWrapper);

        if (count > 0){
            //有菜品处于在售状态，不能删除
            throw new CustomException("菜品在售，不能删除");
        }

        //没有在售，删除菜品
        this.removeByIds(ids);

        //删除关联的菜品口味
        LambdaQueryWrapper<DishFlavor> lambdaQueryWrapper = new LambdaQueryWrapper<>();
        lambdaQueryWrapper.in(DishFlavor::getDishId,ids);
        dishFlavorService.remove(lambdaQueryWrapper);
    }
}
```



### SetmealService

```java
public interface SetmealService extends IService<Setmeal> {
    void saveWithDishes(SetmealDto setmealDto);
    void deleteWithDishes(List<Long> ids);
    SetmealDto getWithDishes(Long id);
    void updatewithDishes(SetmealDto setmealDto);
}
```

```java
@Service
public class SetmealServiceImpl extends ServiceImpl<SetmealMapper, Setmeal> implements SetmealService {

    @Autowired
    private SetmealDishService setmealDishService;

    /**
     * 新增套餐，同时保存套餐对应的菜品信息
     * @param setmealDto
     */
    @Transactional
    public void saveWithDishes(SetmealDto setmealDto) {
        //把套餐信息添加到数据库中，setmealDto继承自setmeal，可以直接添加
        this.save(setmealDto);

        //保存完套餐信息后，会自动生成DishId
        Long setmealId = setmealDto.getId();

        //获取套餐包含的菜品集合
        List<SetmealDish> list = setmealDto.getSetmealDishes();
        for (SetmealDish setmealDish : list) {
            setmealDish.setSetmealId(setmealId);
        }

        setmealDishService.saveBatch(list);
    }

    /**
     * 删除套餐和套餐关联菜品
     * @param ids
     */
    //操作两张表为了保障一致性，需要添加事务
    @Transactional
    public void deleteWithDishes(List<Long> ids) {

        //查询套餐是否在售，确定是否可以删除
        LambdaQueryWrapper<Setmeal> lambdaQueryWrapper = new LambdaQueryWrapper<>();
        //select count(*) from setmeal where id in (ids:1,2,3) and status = 1
        lambdaQueryWrapper.in(Setmeal::getId,ids);
        lambdaQueryWrapper.eq(Setmeal::getStatus,1);

        //查询在售套餐数量
        int count = this.count(lambdaQueryWrapper);

        if (count > 0){
            //有在售的套餐不能删除
            //抛出一个自定义业务异常
            throw  new CustomException("套餐在售，不能删除");
        }

        //没有在售的套餐，可以删除
        this.removeByIds(ids);

        //删除关联的菜品数据
        LambdaQueryWrapper<SetmealDish> queryWrapper = new LambdaQueryWrapper<>();
        queryWrapper.in(SetmealDish::getSetmealId,ids);
        setmealDishService.remove(queryWrapper);
    }
    
    /**
     * 查询套餐和对应菜品信息
     * @param id
     */
    public SetmealDto getWithDishes(Long id) {
        Setmeal setmeal = this.getById(id);
        SetmealDto setmealDto = new SetmealDto();
        BeanUtils.copyProperties(setmeal,setmealDto);

        LambdaQueryWrapper<SetmealDish> queryWrapper = new LambdaQueryWrapper<>();
        queryWrapper.eq(SetmealDish::getSetmealId,id);
        List<SetmealDish> list = setmealDishService.list(queryWrapper);

        setmealDto.setSetmealDishes(list);
        return setmealDto;
    }
    
    /**
     * 修改套餐和对应菜品信息
     * @param setmealDto
     */
    @Transactional
    public void updatewithDishes(SetmealDto setmealDto) {
        this.updateById(setmealDto);

        //清理当前菜品对应的口味数据
        LambdaQueryWrapper<SetmealDish> queryWrapper = new LambdaQueryWrapper<>();
        queryWrapper.eq(SetmealDish::getSetmealId,setmealDto.getId());
        setmealDishService.remove(queryWrapper);

        List<SetmealDish> setmealDishes = setmealDto.getSetmealDishes();
        for (SetmealDish setmealDish : setmealDishes) {
            setmealDish.setSetmealId(setmealDto.getId());
        }

        setmealDishService.saveBatch(setmealDishes);
    }
}
```



### DishFlavorService

```java
public interface DishFlavorService extends IService<DishFlavor> {
}
```

```java
@Service
public class DishFlavorServiceImpl extends ServiceImpl<DishFlavorMapper, DishFlavor> implements DishFlavorService {
}
```



### SetmealDishService

```java
public interface SetmealDishService extends IService<SetmealDish> {
}
```

```java
@Service
public class SetmealDishServiceImpl extends ServiceImpl<SetmealDishMapper, SetmealDish> implements SetmealDishService {
}
```



### UserService

```JAVA
public interface UserService extends IService<User> {
}
```

```java
@Service
public class UserServiceImpl extends ServiceImpl<UserMapper, User> implements UserService {
}
```



### AddressBookService

```java
public interface AddressBookService extends IService<AddressBook> {
}
```

```java
@Service
public class AddressBookServiceImpl extends ServiceImpl<AddressBookMapper, AddressBook> implements AddressBookService {
}
```



### ShoppingCartService

```java
public interface ShoppingCartService extends IService<ShoppingCart> {
}
```

```java
@Service
public class ShoppingCartServiceImpl extends ServiceImpl<ShoppingCartMapper, ShoppingCart> implements ShoppingCartService {
}
```



### OrderService

```java
public interface OrderService extends IService<Orders> {
        void submit(Orders orders);
}
```

```java
@Service
public class OrderServiceImpl extends ServiceImpl<OrderMapper, Orders> implements OrderService {

    @Autowired
    private ShoppingCartService shoppingCartService;

    @Autowired
    private UserService userService;

    @Autowired
    private AddressBookService addressBookService;

    @Autowired
    private OrderDetailService orderDetailService;

    @Transactional
    public void submit(Orders orders) {

        //获取当前用户id
        Long currentId = BaseContext.getCurrentId();
        orders.setUserId(currentId);

        //查询用户当前的购物车数据
        LambdaQueryWrapper<ShoppingCart> queryWrapper = new LambdaQueryWrapper<>();
        queryWrapper.eq(ShoppingCart::getUserId,currentId);
        List<ShoppingCart> shoppingCarts = shoppingCartService.list(queryWrapper);
        if (shoppingCarts == null){
            throw new CustomException("购物车为空");
        }

        //查询用户信息
        User user = userService.getById(currentId);

        //查询地址数据
        Long addressBookId = orders.getAddressBookId();
        AddressBook addressBook = addressBookService.getById(addressBookId);
        if (addressBook == null){
            throw new CustomException("收货地址为空");
        }

        long orderId = IdWorker.getId();//订单号

        AtomicInteger amount = new AtomicInteger(0);
        List<OrderDetail> orderDetailList = new ArrayList<>();

        for (ShoppingCart shoppingCart : shoppingCarts) {
            OrderDetail orderDetail = new OrderDetail();
            orderDetail.setOrderId(orderId);
            orderDetail.setNumber(shoppingCart.getNumber());
            orderDetail.setDishFlavor(shoppingCart.getDishFlavor());
            orderDetail.setDishId(shoppingCart.getDishId());
            orderDetail.setSetmealId(shoppingCart.getSetmealId());
            orderDetail.setName(shoppingCart.getName());
            orderDetail.setImage(shoppingCart.getImage());
            orderDetail.setAmount(shoppingCart.getAmount());
            amount.addAndGet(shoppingCart.getAmount().multiply(new BigDecimal(shoppingCart.getNumber())).intValue());
            orderDetailList.add(orderDetail);
        }
        orderDetailService.saveBatch(orderDetailList);

        orders.setId(orderId);
        orders.setOrderTime(LocalDateTime.now());
        orders.setCheckoutTime(LocalDateTime.now());
        orders.setStatus(2);
        orders.setAmount(new BigDecimal(amount.get()));//总金额
        orders.setUserId(currentId);
        orders.setNumber(String.valueOf(orderId));
        orders.setUserName(user.getName());
        orders.setConsignee(addressBook.getConsignee());
        orders.setPhone(addressBook.getPhone());
        orders.setAddress((addressBook.getProvinceName() == null ? "" : addressBook.getProvinceName())
                + (addressBook.getCityName() == null ? "" : addressBook.getCityName())
                + (addressBook.getDistrictName() == null ? "" : addressBook.getDistrictName())
                + (addressBook.getDetail() == null ? "" : addressBook.getDetail()));

        this.save(orders);

        //清空购物车数据
        shoppingCartService.remove(queryWrapper);
    }
}
```



### OrderDetailService

```java
public interface OrderDetailService extends IService<OrderDetail> {
}
```

```java
@Service
public class OrderDetailService extends ServiceImpl<OrderDetailMapper, OrderDetail> implements com.zxp.service.OrderDetailService {
}
```



## 定义controller类

### EmployeeController

#### 员工登录

```java
 /**
     * 员工登录
     * @param request
     * @param employee
     * @return
     */
    @PostMapping("/login")//网页发送的请求为post方法：/employee/login
    //HttpServletRequest对象用于获取Session对象
    //@RequestBody主要用来接收前端传递给后端的json字符串中的数据
    public R<Employee> login(HttpServletRequest request, @RequestBody Employee employee){

        //1.将页面提交的密码进行md5加密
        String password = employee.getPassword();
        password = DigestUtils.md5DigestAsHex(password.getBytes());

        //2.根据页面提交的用户名查询数据库
        LambdaQueryWrapper<Employee> lqw = new LambdaQueryWrapper<>();
        //由于直接写字段名容易写错，因此采用直接获取实体类字段名的方式，User::getId相当于“id”
        ////第一个参数代表要查询的字段，第二个参数表示查询数据对应字段与该参数相等
        lqw.eq(Employee::getUsername,employee.getUsername());
        Employee emp = employeeService.getOne(lqw);//根据Wrapper，查询一条记录

        //3.如果查询结果为空返回登陆失败结果
        if(emp == null){
            return R.error("该用户不存在！");
        }

        //4.进行密码比对，如果不一致返回登陆失败结果
        if(!password.equals(emp.getPassword())){
            return R.error("密码错误！");
        }

        //5.查看员工状态，如果禁用返回登陆失败结果
        if (emp.getStatus() == 0){
            return R.error("该用户已经停用！");
        }

        //6.登陆成功，将员工id存入Session并返回登录成功结果
        //获取Session对象，并存储用户id到Session域中
        request.getSession().setAttribute("employee",emp.getId());
        return R.success(emp);
    }
```

#### 用户退出

```java
/**
     * 用户退出
     * @param request
     * @return
     */
    @PostMapping("/logout")//网页发送的请求为post方法：/employee/logout
    public R<String> logout(HttpServletRequest request){
        //把员工id从session中去除
        request.getSession().removeAttribute("employee");
        return R.success("退出成功");
    }
```

#### 添加员工

```java
 /**
     * 添加员工
     * @param employee
     * @return
     */
    @PostMapping//网页发送的请求为post方法,没有额外路径
    public R<String> addEmployee(@RequestBody Employee employee){

        log.info("新增员工，员工信息：{}", employee.toString());

        //设置初始密码为123456，并进行md5加密
        employee.setPassword(DigestUtils.md5DigestAsHex("123456".getBytes()));

        //设置创建和修改时间，使用自动填充功能后，不需要手动设置
//        employee.setCreateTime(LocalDateTime.now());
//        employee.setUpdateTime(LocalDateTime.now());

        //获取当前登录用户id
//        Long empId = (long) request.getSession().getAttribute("employee");

        //设置创建和修改者id
//        employee.setCreateUser(empId);
//        employee.setUpdateUser(empId);

        //添加员工
        employeeService.save(employee);
        return R.success("新增员工成功");
    }
```

#### 员工信息分页查询

```java
/**
     * 员工信息分页查询
     * @param page
     * @param pageSize
     * @param name
     * @return
     */
    //http://localhost:8080/employee/page?page=1&pageSize=10&name=?
    @GetMapping("/page")//网页发送的请求为get方法,/employee/page
    public R<IPage> page(int page,int pageSize,String name){//根据名称自动解析
        log.info("page = {},pageSize = {},name = {}",page,pageSize,name);

        //创建分页构造器
        IPage pageInfo<Employee> = new Page<>(page,pageSize);
        //多态，第一个参数为当前页码，第二个参数为每页显示的条数

        //创建条件构造器
        LambdaQueryWrapper<Employee> lqw = new LambdaQueryWrapper<>();
        //添加构造条件，name不为空时，查询Name包含name的对象
        lqw.like(StringUtils.isNotEmpty(name), Employee::getName,name);
        //添加排序条件，根据修改时间排序，降序排列，ASC升序排序
        lqw.orderByDesc(Employee::getUpdateTime);

        //执行查询
        employeeService.page(pageInfo,lqw);//条件分页查询，看官网
        return R.success(pageInfo);
    }
```

#### 根据id修改员工信息

这个方法既可以修改员工的状态，也适用于编辑员工信息

```java
/**
     * 根据id修改员工信息
     * @param request
     * @param employee
     * @return
     */
    @PutMapping//网页发送的请求为put方法,没有额外路径,restful中修改用户信息用put方法
    //里边包含需要修改的的信息
    public R<String> update(HttpServletRequest request,@RequestBody Employee employee){

        //employee.setUpdateTime(LocalDateTime.now());//LocalDateTime.now():获取当前时间
        //从session中获取当前登录用户id
        //employee.setUpdateUser((Long) request.getSession().getAttribute("employee"));
        
        //根据id对员工信息进行修改
        boolean check = employeeService.updateById(employee);
        if (check){
            //js对long型数据处理时会丢失精度，导致发送的id和数据库中不一致
            return R.success("员工信息修改成功");
        }else {
            return R.error("修改失败");
        }
    }
```

#### 根据id查询员工信息

在编辑员工信息时，我们需要在表单时填充员工的初始信息，因此我们需要根据id查询员工信息，并展示

```java
 /**
     * 根据id查询员工信息
     * 在编辑员工信息时，我们需要在表单时填充员工的初始信息，因此我们需要根据id查询员工信息，并展示
     * @param id
     * @return
     */
    @GetMapping("/{id}")//网页发送的请求为get方法，/employee/id
    public R<Employee> getById(@PathVariable Long id){//@PathVariable：把请求路径占位符中的id解析出来
        log.info("根据id查询员工信息");
        Employee employee = employeeService.getById(id);
        if (employee != null){
            return R.success(employee);
        }else {
            return R.error("没有查询到对应员工信息");
        }
    }
```

#### 汇总

```java
@Slf4j
@RestController //@RestController = @Controller + @ResponseBody
//@ResponseBody把方法返回值以json形式返回
@RequestMapping("/employee") //访问路径
public class EmployeeController {

    @Autowired
    private EmployeeService employeeService;

    /**
     * 员工登录
     * @param request
     * @param employee
     * @return
     */
    @PostMapping("/login")//网页发送的请求为post方法：/employee/login
    //HttpServletRequest对象用于获取Session对象
    //@RequestBody主要用来接收前端传递给后端的json字符串中的数据
    public R<Employee> login(HttpServletRequest request, @RequestBody Employee employee){

        //1.将页面提交的密码进行md5加密
        String password = employee.getPassword();
        password = DigestUtils.md5DigestAsHex(password.getBytes());

        //2.根据页面提交的用户名查询数据库
        LambdaQueryWrapper<Employee> lqw = new LambdaQueryWrapper<>();
        //由于直接写字段名容易写错，因此采用直接获取实体类字段名的方式，User::getId相当于“id”
        ////第一个参数代表要查询的字段，第二个参数表示查询数据对应字段与该参数相等
        lqw.eq(Employee::getUsername,employee.getUsername());
        Employee emp = employeeService.getOne(lqw);//根据Wrapper，查询一条记录

        //3.如果查询结果为空返回登陆失败结果
        if(emp == null){
            return R.error("该用户不存在！");
        }

        //4.进行密码比对，如果不一致返回登陆失败结果
        if(!password.equals(emp.getPassword())){
            return R.error("密码错误！");
        }

        //5.查看员工状态，如果禁用返回登陆失败结果
        if (emp.getStatus() == 0){
            return R.error("该用户已经停用！");
        }

        //6.登陆成功，将员工id存入Session并返回登录成功结果
        //获取Session对象，并存储用户id到Session域中
        request.getSession().setAttribute("employee",emp.getId());
        return R.success(emp);
    }

    /**
     * 用户退出
     * @param request
     * @return
     */
    @PostMapping("/logout")//网页发送的请求为post方法：/employee/logout
    public R<String> logout(HttpServletRequest request){
        //把员工id从session中去除
        request.getSession().removeAttribute("employee");
        return R.success("退出成功");
    }
    
     /**
     * 添加员工
     * @param request
     * @param employee
     * @return
     */
    @PostMapping//网页发送的请求为post方法,没有额外路径
    public R<String> addEmployee(HttpServletRequest request, @RequestBody Employee employee){

        log.info("新增员工，员工信息：{}", employee.toString());

        //设置初始密码为123456，并进行md5加密
        employee.setPassword(DigestUtils.md5DigestAsHex("123456".getBytes()));

        //设置创建和修改时间
        //employee.setCreateTime(LocalDateTime.now());
        //employee.setUpdateTime(LocalDateTime.now());

        //获取当前登录用户id
        //Long empId = (long) request.getSession().getAttribute("employee");

        //设置创建和修改者id
        //employee.setCreateUser(empId);
        //employee.setUpdateUser(empId);

        //添加员工
        employeeService.save(employee);
        return R.success("新增员工成功");
    }
    
     /**
     * 员工信息分页查询
     * @param page
     * @param pageSize
     * @param name
     * @return
     */
    //http://localhost:8080/employee/page?page=1&pageSize=10&name=?
    @GetMapping("/page")//网页发送的请求为get方法,/employee/page
    public R<IPage> page(int page,int pageSize,String name){
        log.info("page = {},pageSize = {},name = {}",page,pageSize,name);

        //创建分页构造器
        IPage pageInfo = new Page(page,pageSize);//多态，第一个参数为当前页码，第二个参数为每页显示的条数

        //创建条件构造器
        LambdaQueryWrapper<Employee> lqw = new LambdaQueryWrapper<>();
        //添加构造条件，name不为空时，查询Name包含name的对象
        lqw.like(StringUtils.isNotEmpty(name), Employee::getName,name);
        //添加排序条件，根据修改时间排序，降序排列，ASC升序排序
        lqw.orderByDesc(Employee::getUpdateTime);

        //执行查询
        employeeService.page(pageInfo,lqw);//条件分页查询，看官网
        return R.success(pageInfo);
    }
    
    /**
     * 根据id修改员工信息
     * 这个方法既可以修改员工的状态，也适用于编辑员工信息	
     * @param request
     * @param employee
     * @return
     */
    @PutMapping//网页发送的请求为put方法,没有额外路径,restful中修改用户信息用put方法
    //里边包含需要修改的的信息
    public R<String> update(HttpServletRequest request,@RequestBody Employee employee){

        //employee.setUpdateTime(LocalDateTime.now());//LocalDateTime.now():获取当前时间
        //从session中获取当前登录用户id
        //employee.setUpdateUser((Long) request.getSession().getAttribute("employee"));
        
        //根据id对status进行修改，并说明修改者和修改时间
        boolean check = employeeService.updateById(employee);
        if (check){
            //js对long型数据处理时会丢失精度，导致发送的id和数据库中不一致
            return R.success("员工信息修改成功");
        }else {
            return R.error("修改失败");
        }
    }
    
     /**
     * 根据id查询员工信息
     * 在编辑员工信息时，我们需要在表单时填充员工的初始信息，因此我们需要根据id查询员工信息，并展示
     * @param id
     * @return
     */
    @GetMapping("/{id}")//网页发送的请求为get方法，/employee/id
    public R<Employee> getById(@PathVariable Long id){//@PathVariable：把请求路径占位符中的id解析出来
        log.info("根据id查询员工信息");
        Employee employee = employeeService.getById(id);
        if (employee != null){
            return R.success(employee);
        }else {
            return R.error("没有查询到对应员工信息");
        }
    }
}
```

### CategoryController

#### 添加分类

```java
/**
     * 添加分类
     * @param category
     * @return
     */
    @PostMapping//网页发送的请求为post方法,没有额外路径
    //@RequestBody主要用来接收前端传递给后端的json字符串中的数据
    public R<String> addCategory(@RequestBody Category category){
        log.info("新增分类信息为：{}", category.toString());

        //添加分类
        boolean check = categoryService.save(category);
        if (check) {
            return R.success("添加分类成功！");
        }else {
            return R.error("添加失败");
        }
    }
```



#### 分类信息分页查询

```java
/**
     * 分类信息分页查询
     * @param page
     * @param pageSize
     * @return
     */
    @GetMapping("/page")//网页发送的请求为get方法,/category/page
    public R<IPage> page(int page,int pageSize){//根据名称自动解析
        log.info("page = {}, pageSize = {}", page, pageSize);

        //创建分页构造器
        IPage<Category> pageInfo = new Page<>(page,pageSize);

        //创建条件构造器
        LambdaQueryWrapper<Category> lqw = new LambdaQueryWrapper<>();
        //添加排序条件，根据修改时间排序，降序排列，ASC升序排序
        lqw.orderByDesc(Category::getUpdateTime);

        //执行查询
        categoryService.page(pageInfo,lqw);//条件分页查询，看官网
        return R.success(pageInfo);
    }
```



#### 修改分类信息

```java
 /**
     * 修改分类信息
     * @param category
     * @return
     */
    @PutMapping
    public R<String> updateCategory(@RequestBody Category category) {
        categoryService.updateById(category);
        return R.success("修改分类信息成功");
    }
```



#### 删除分类信息

```java
/**
 * 根据id删除分类
 *
 * @param ids
 * @return
 */
@DeleteMapping
public R<String> deleteCategory(Long ids) {
    log.info("id:{}", ids);
    //修改后的功能
    categoryService.removeById(ids);
    return R.success("分类信息删除成功");
}
```



#### 查询分类数据

```java
/**
 * 根据条件查询分类数据
 * @param category
 * @return
 */
@GetMapping("/list")//请求方式为Get，路径为/category/list?type=1或2
public R<List<Category>> list(Category category){//把接受到的type参数封装到Category对象中
    //条件构造器
    LambdaQueryWrapper<Category> lqw = new LambdaQueryWrapper<>();
    //接收的type不为空时，查询对应分类信息
    lqw.eq(category.getType() != null,Category::getType,category.getType());
    //根据分类字段和修改时间降序排列
    lqw.orderByDesc(Category::getSort).orderByDesc(Category::getUpdateTime);

    List<Category> categoryList = categoryService.list(lqw);
    return R.success(categoryList);
}
```



#### 汇总

```java
/**
 * 分类管理
 */
@Slf4j
@RestController
@RequestMapping("/category")
public class CategoryController {

    @Autowired
    private CategoryService categoryService;

    /**
     * 添加分类
     * @param category
     * @return
     */
    @PostMapping//网页发送的请求为post方法,没有额外路径
    //@RequestBody主要用来接收前端传递给后端的json字符串中的数据
    public R<String> addCategory(@RequestBody Category category) {
        log.info("新增分类信息为：{}", category.toString());

        //添加分类
        boolean check = categoryService.save(category);
        if (check) {
            return R.success("添加分类成功！");
        } else {
            return R.error("添加失败");
        }
    }

    /**
     * 分类信息分页查询
     * @param page
     * @param pageSize
     * @return
     */
    @GetMapping("/page")//网页发送的请求为get方法,/category/page
    public R<IPage> page(int page, int pageSize) {//根据名称自动解析
        log.info("page = {}, pageSize = {}", page, pageSize);

        //创建分页构造器
        IPage pageInfo = new Page(page, pageSize);

        //创建条件构造器
        LambdaQueryWrapper<Category> lqw = new LambdaQueryWrapper<>();
        //添加排序条件，根据修改时间排序，降序排列，ASC升序排序
        lqw.orderByDesc(Category::getUpdateTime);

        //执行查询
        categoryService.page(pageInfo, lqw);//条件分页查询，看官网
        return R.success(pageInfo);
    }

    /**
     * 修改分类信息
     * @param category
     * @return
     */
    @PutMapping
    public R<String> updateCategory(@RequestBody Category category) {
        categoryService.updateById(category);
        return R.success("修改分类信息成功");
    }

    /**
     * 根据id删除分类
     * @param ids
     * @return
     */
    @DeleteMapping
    public R<String> deleteCategory(Long ids) {
        log.info("id:{}", ids);
        categoryService.remove(ids);
        return R.success("分类信息删除成功");
    }
    
     /**
     * 根据条件查询分类数据
     * @param category
     * @return
     */
    @GetMapping("/list")//请求方式为Get，路径为/category/list?type=1或2
    public R<List<Category>> list(Category category){//把接受到的type参数封装到Category对象中
        //条件构造器
        LambdaQueryWrapper<Category> lqw = new LambdaQueryWrapper<>();
        //接收的type不为空时，查询对应分类信息
        lqw.eq(category.getType() != null,Category::getType,category.getType());
        //根据分类字段和修改时间降序排列
        lqw.orderByDesc(Category::getSort).orderByDesc(Category::getUpdateTime);

        List<Category> categoryList = categoryService.list(lqw);
        return R.success(categoryList);
    }
}
```



### CommonController

#### 文件上传

文件上传，也称为upload，是将本地图片，视频，音频等文件上传到服务器上，可以供其他用户浏览或下载的过程，文件上传在项目中应用广泛，发朋友圈用到了上传文件功能

文件上传时，对页面的form表单有如下要求

- method = "post"，采用post方式提交数据
- enctype = "multipart/form-data"，采用multiparty格式上传文件
- type = "file"，使用input的file控件上传

例如

```html
<form method = "post" action = "/common/upload enctype = "multipart/form-data">
     <input name = "myfile" type = "file"/>                                                     
     <input type = "submit" value = "提交"/>                                    
</form>
```

服务器要接受客户端页面上传的文件，通常使用

- commons-fileupload
- commons-io

Spring框架在spring-web包中对文件上传进行了封装，大大简化了服务器代码，只需要在Controller的方法中声明一个MultipartFile类型的参数即可接收上传的文件

```java
@PostMapping("/upload")
    //Spring框架在spring-web包中对文件上传进行了封装，只需要在Controller的方法中声明一个MultipartFile类型的参数即可接收上传的文件
    //参数名称必须与form-data中的name一致
    //file是一个临时文件，需要转存到指定位置，否则本次请求完成后临时文件会删除
    public R<String> upload(MultipartFile file){

        //获得原始文件名
        String originalFilename = file.getOriginalFilename();
        //截取文件名后缀
        String suffix = originalFilename.substring(originalFilename.lastIndexOf("."));//截取了.和后缀名，包前不包后

        //使用UUID重新生成文件名，防止文件名重复造成文件覆盖
        String fileName = UUID.randomUUID().toString() + suffix;

        //创建一个File对象
        File dir = new File(filePath);
        //判断当前目录是否存在
        if (!dir.exists()){
            //目录不存在，需要创建
            dir.mkdirs();//创建由此绝对路径名命名的目录，包括任何需要但不存在的父目录。
        }

        //将临时文件转存到另外一个位置
        try {
            file.transferTo(new File(filePath + fileName));
        } catch (IOException e) {
            e.printStackTrace();
        }
        return R.success(fileName);//返回文件名
    }
```



#### 文件下载

文件下载，也称为download，是指将文件从服务器传输到本地计算机的过程

通过浏览器进行文件下载，通常有两种表现形式：

- 以附件形式下载，弹出保存对话框，将文件保存到指定磁盘目录
- 直接在浏览器中打开

通过浏览器进行文件下载，本质就是服务器将文件以流的形式写回浏览器的过程

```java
@GetMapping("download")
    public void download(String name, HttpServletResponse response){//根据名字自动匹配参数

        try {
            //输入流，通过输入流读取文件内容
            FileInputStream fileInputStream = new FileInputStream(new File(filePath + name));

            //输出流，通过输出流将文件写回到浏览器，在浏览器展示图
            ServletOutputStream outputStream = response.getOutputStream();

            //设置响应数据类型为图片
            response.setContentType("image/jpg");

            //创捷字节数组用来读取图片
            byte[] bytes = new byte[1024];
            int len = 0;
            //每次读取一个字节数组,返回值为读取的字节数，如果字节已经没有可读的返回-1
            while ((len = fileInputStream.read(bytes)) != -1){
                //写一个字节数组的一部分进去，从0读取到len
                outputStream.write(bytes,0,len);
                //写完数据必须刷新数据
                outputStream.flush();
            }

            //关闭资源
            outputStream.close();
            fileInputStream.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
```



#### 汇总

```java
@RestController
@RequestMapping("/common")
public class CommonController {

    //从配置文件中读取属性的值
    @Value("${filePath.path}")
    private String filePath;

    @PostMapping("/upload")
    //Spring框架在spring-web包中对文件上传进行了封装，只需要在Controller的方法中声明一个MultipartFile类型的参数即可接收上传的文件
    //参数名称必须与form-data中的name一致
    //file是一个临时文件，需要转存到指定位置，否则本次请求完成后临时文件会删除
    public R<String> upload(MultipartFile file){

        //获得原始文件名
        String originalFilename = file.getOriginalFilename();
        //截取文件名后缀
        String suffix = originalFilename.substring(originalFilename.lastIndexOf("."));//截取了.和后缀名，包前不包后

        //使用UUID重新生成文件名，防止文件名重复造成文件覆盖
        String fileName = UUID.randomUUID().toString() + suffix;

        //创建一个File对象
        File dir = new File(filePath);
        //判断当前目录是否存在
        if (!dir.exists()){
            //目录不存在，需要创建
            dir.mkdirs();//创建由此绝对路径名命名的目录，包括任何需要但不存在的父目录。
        }

        //将临时文件转存到另外一个位置
        try {
            file.transferTo(new File(filePath + fileName));
        } catch (IOException e) {
            e.printStackTrace();
        }
        return R.success(fileName);//返回文件名
    }

    @GetMapping("download")
    public void download(String name, HttpServletResponse response){//根据名字自动匹配参数

        try {
            //输入流，通过输入流读取文件内容
            FileInputStream fileInputStream = new FileInputStream(new File(filePath + name));

            //输出流，通过输出流将文件写回到浏览器，在浏览器展示图
            ServletOutputStream outputStream = response.getOutputStream();

            //设置响应数据类型为图片
            response.setContentType("image/jpg");

            //创捷字节数组用来读取图片
            byte[] bytes = new byte[1024];
            int len = 0;
            //每次读取一个字节数组,返回值为读取的字节数，如果字节已经没有可读的返回-1
            while ((len = fileInputStream.read(bytes)) != -1){
                //写一个字节数组的一部分进去，从0读取到len
                outputStream.write(bytes,0,len);
                //写完数据必须刷新数据
                outputStream.flush();
            }

            //关闭资源
            outputStream.close();
            fileInputStream.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```



### DishController

#### 新增菜品

```java
    /**
     * 新增菜品，同时保存菜品对应的口味数据
     * @param dishDto
     * @return
     */
    @PostMapping
    public R<String> save(@RequestBody DishDto dishDto) {
        log.info(dishDto.toString());

        dishService.saveWithFlavor(dishDto);

        String key = "dish_" + dishDto.getCategoryId() + "_" + dishDto.getStatus();
        redisTemplate.delete(key);

        return R.success("新增菜品成功");
    }
```



#### 分页查询菜品信息

```java
/**
 * 分页查询菜品信息
 * @param page
 * @param pageSize
 * @param name
 * @return
 */
@GetMapping("/page")
public R<IPage> page(int page,int pageSize,String name){
    //创建分页构造器
    IPage<Dish> pageInfo = new Page<>(page,pageSize);
    //页面需要接收菜品分类信息，因此需要用DTO加上CategoryName
    IPage<DishDto> dishDtoPage = new Page<>();

    //创建条件构造器
    LambdaQueryWrapper<Dish> queryWrapper = new LambdaQueryWrapper<>();
    //添加过滤条件
    queryWrapper.like(name != null,Dish::getName,name);
    queryWrapper.eq(Dish::getIsDeleted,0);
    //添加排序条件
    queryWrapper.orderByDesc(Dish::getSort).orderByDesc(Dish::getUpdateTime);


    //执行查询
    dishService.page(pageInfo,queryWrapper);

    //对象拷贝，把第一个参数的属性拷贝到第二个参数，第三个参数是不参与拷贝的属性
    BeanUtils.copyProperties(pageInfo,dishDtoPage,"records");

    //创建DishDto集合，方便接收对象
    List<DishDto> list = new ArrayList<>();

    //records记录的是分页查询出来的对象集合
    List<Dish> records = pageInfo.getRecords();
    for (Dish dish : records) {
        DishDto dishDto = new DishDto();
        //把dish的属性拷贝到dishDto
        BeanUtils.copyProperties(dish,dishDto);

        //获取categoryName，并添加到dishDto
        Long categoryId = dish.getCategoryId();
        Category category = categoryService.getById(categoryId);
        if (category != null) {
            String categoryName = category.getName();
            dishDto.setCategoryName(categoryName);
        }
        //把对象添加到集合中
        list.add(dishDto);
    }

    //设置dishDtoPage的records记录
    dishDtoPage.setRecords(list);

    return R.success(dishDtoPage);
}
```



#### 查询菜品和口味信息

```java
/**
 * 根据id查询菜品信息和对应的口味信息
 * @param id
 * @return
 */
@GetMapping("/{id}")//dish/1587357731009540098
public R<DishDto> get(@PathVariable Long id){//@PathVariable：把请求路径占位符中的id解析出来
    //查询菜品和菜品对应的口味信息
    DishDto dishDto = dishService.getByIdWithFlavor(id);
    return R.success(dishDto);
}
```



#### 修改菜品和口味信息

```java
    /**
     * 修改菜品和对应的口味信息
     * @param dishDto
     * @return
     */
    @PutMapping
    public R<String> updateWithFlavor(@RequestBody DishDto dishDto) {
        dishService.updateByIdWithFlavor(dishDto);
        String key = "dish_" + dishDto.getCategoryId() + "_" + dishDto.getStatus();
        redisTemplate.delete(key);
        return R.success("修改成功");
    }
```



#### 修改菜品状态

```java
    /**
     * 修改菜品状态
     * @param status
     * @param ids
     * @return
     */
    @PostMapping("/status/{status}")//“/dish/status/0?ids=1413342036832100354,1397862198033297410”
    public R<String> updateStatus(@PathVariable Integer status, @RequestParam List<Long> ids) {

        Dish dish = new Dish();
        dish.setStatus(status);
        LambdaQueryWrapper<Dish> queryWrapper= new LambdaQueryWrapper<>();
        queryWrapper.in(Dish::getId,ids);
        dishService.update(dish,queryWrapper);
        return R.success("修改成功");
    }
```



#### 删除菜品

```java
 /**
     * 删除菜品
     * @param ids
     * @return
     */
    @DeleteMapping
    public R<String> delete(@RequestParam List<Long> ids){
        dishService.deleteWithFlavor(ids);
        return R.success("删除成功");
    }
```



#### 根据条件查询菜品信息

```java
   /**
     * 根据条件查询对应的菜品信息
     * @param dish
     * @return
     */
    @GetMapping("/list")//dish/list?categoryId=1413384954989060097
    public R<List<DishDto>> list(Dish dish){

        List<DishDto> dtoList = null;

        //动态构造key
        String key = "dish_" + dish.getCategoryId() + "_" + dish.getStatus();

        //从缓存中获取数据
        dtoList = (List<DishDto>) redisTemplate.opsForValue().get(key);

        if (dtoList != null){
            //如果存在直接返回，无需查询数据库
            return R.success(dtoList);
        }

        //构造查询条件
        LambdaQueryWrapper<Dish> queryWrapper = new LambdaQueryWrapper<>();
        queryWrapper.eq(dish.getCategoryId() != null, Dish::getCategoryId, dish.getCategoryId());
        //菜品状态应该是在售状态，status=1
        queryWrapper.eq(Dish::getStatus,1);

        queryWrapper.orderByDesc(Dish::getPrice);

        List<Dish> dishList = dishService.list(queryWrapper);

        /**
         * abcList.stream().map(item -> {
         * 一系列操作
         * return "想要输出到新List中的元素";
         * }).collect(Collectors.toList());
         *
         * item: abcList中每个元素
         * Collectors.toList(): 将返回结果输出到一个新的List中
         * 返回结果类型为List
         */
        dtoList = dishList.stream().map(item -> {
            DishDto dishDto = new DishDto();

            BeanUtils.copyProperties(item,dishDto);

            //获取categoryName，并添加到dishDto
            Long categoryId = item.getCategoryId();
            Category category = categoryService.getById(categoryId);
            if (category != null) {
                String categoryName = category.getName();
                dishDto.setCategoryName(categoryName);
            }

            Long dishId = item.getId();
            LambdaQueryWrapper<DishFlavor> lambdaQueryWrapper = new LambdaQueryWrapper<>();
            lambdaQueryWrapper.eq(DishFlavor::getDishId,dishId);
            List<DishFlavor> flavors = dishFlavorService.list(lambdaQueryWrapper);

            dishDto.setFlavors(flavors);

            return dishDto;
        }).collect(Collectors.toList());

        //如果不存在，需要查询数据库，将查询到的菜品数据缓存到Redis
        redisTemplate.opsForValue().set(key,dtoList,60, TimeUnit.MINUTES);

        return R.success(dtoList);
    }
```



#### 汇总

```java
@RestController
@RequestMapping("/dish")
@Slf4j
public class DishController {

    @Autowired
    private DishService dishService;

    @Autowired
    private DishFlavorService dishFlavorService;

    @Autowired
    private CategoryService categoryService;

    @Autowired
    private RedisTemplate redisTemplate;


    /**
     * 新增菜品，同时保存菜品对应的口味数据
     * @param dishDto
     * @return
     */
    @PostMapping
    public R<String> save(@RequestBody DishDto dishDto) {
        log.info(dishDto.toString());

        dishService.saveWithFlavor(dishDto);

        String key = "dish_" + dishDto.getCategoryId() + "_" + dishDto.getStatus();
        redisTemplate.delete(key);

        return R.success("新增菜品成功");
    }

    /**
     * 分页查询菜品信息
     *
     * @param page
     * @param pageSize
     * @param name
     * @return
     */
    @GetMapping("/page")///dish/page?page=1&pageSize=10&name=米饭
    public R<IPage> page(int page, int pageSize, String name) {
        //创建分页构造器
        IPage<Dish> pageInfo = new Page<>(page, pageSize);
        //页面需要接收菜品分类信息，因此需要用DTO加上CategoryName
        IPage<DishDto> dishDtoPage = new Page<>();

        //创建条件构造器
        LambdaQueryWrapper<Dish> queryWrapper = new LambdaQueryWrapper<>();
        //添加过滤条件
        queryWrapper.like(name != null, Dish::getName, name);
        queryWrapper.eq(Dish::getIsDeleted,0);
        //添加排序条件
        queryWrapper.orderByDesc(Dish::getSort).orderByDesc(Dish::getUpdateTime);

        //执行查询
        dishService.page(pageInfo, queryWrapper);

        //对象拷贝，把第一个参数的属性拷贝到第二个参数，第三个参数是不参与拷贝的属性
        BeanUtils.copyProperties(pageInfo, dishDtoPage, "records");

        //创建DishDto集合，方便接收对象
        List<DishDto> list = new ArrayList<>();

        //records记录的是分页查询出来的对象集合
        List<Dish> records = pageInfo.getRecords();
        for (Dish dish : records) {
            DishDto dishDto = new DishDto();
            //把dish的属性拷贝到dishDto
            BeanUtils.copyProperties(dish, dishDto);

            //获取categoryName，并添加到dishDto
            Long categoryId = dish.getCategoryId();
            Category category = categoryService.getById(categoryId);
            if (category != null) {
                String categoryName = category.getName();
                dishDto.setCategoryName(categoryName);
            }
            //把对象添加到集合中
            list.add(dishDto);
        }

        //设置dishDtoPage的records记录
        dishDtoPage.setRecords(list);

        return R.success(dishDtoPage);
    }

    /**
     * 根据id查询菜品信息和对应的口味信息
     * @param id
     * @return
     */
    @GetMapping("/{id}")//dish/1587357731009540098
    public R<DishDto> get(@PathVariable Long id) {//@PathVariable：把请求路径占位符中的id解析出来
        //查询菜品和菜品对应的口味信息
        DishDto dishDto = dishService.getByIdWithFlavor(id);
        return R.success(dishDto);
    }

    /**
     * 修改菜品和对应的口味信息
     * @param dishDto
     * @return
     */
    @PutMapping
    public R<String> updateWithFlavor(@RequestBody DishDto dishDto) {
        dishService.updateByIdWithFlavor(dishDto);
        String key = "dish_" + dishDto.getCategoryId() + "_" + dishDto.getStatus();
        redisTemplate.delete(key);
        return R.success("修改成功");
    }

    /**
     * 修改菜品状态
     * @param status
     * @param ids
     * @return
     */
    @PostMapping("/status/{status}")//“/dish/status/0?ids=1413342036832100354,1397862198033297410”
    public R<String> updateStatus(@PathVariable Integer status, @RequestParam List<Long> ids) {

        Dish dish = new Dish();
        dish.setStatus(status);
        LambdaQueryWrapper<Dish> queryWrapper= new LambdaQueryWrapper<>();
        queryWrapper.in(Dish::getId,ids);
        dishService.update(dish,queryWrapper);
        return R.success("修改成功");
    }

    /**
     * 删除菜品
     * @param ids
     * @return
     */
    @DeleteMapping
    public R<String> delete(@RequestParam List<Long> ids){
        dishService.deleteWithFlavor(ids);
        return R.success("删除成功");
    }

    /**
     * 根据条件查询对应的菜品信息
     * @param dish
     * @return
     */
    @GetMapping("/list")//dish/list?categoryId=1413384954989060097
    public R<List<DishDto>> list(Dish dish){

        List<DishDto> dtoList = null;

        //动态构造key
        String key = "dish_" + dish.getCategoryId() + "_" + dish.getStatus();

        //从缓存中获取数据
        dtoList = (List<DishDto>) redisTemplate.opsForValue().get(key);

        if (dtoList != null){
            //如果存在直接返回，无需查询数据库
            return R.success(dtoList);
        }

        //构造查询条件
        LambdaQueryWrapper<Dish> queryWrapper = new LambdaQueryWrapper<>();
        queryWrapper.eq(dish.getCategoryId() != null, Dish::getCategoryId, dish.getCategoryId());
        //菜品状态应该是在售状态，status=1
        queryWrapper.eq(Dish::getStatus,1);

        queryWrapper.orderByDesc(Dish::getPrice);

        List<Dish> dishList = dishService.list(queryWrapper);

        /**
         * abcList.stream().map(item -> {
         * 一系列操作
         * return "想要输出到新List中的元素";
         * }).collect(Collectors.toList());
         *
         * item: abcList中每个元素
         * Collectors.toList(): 将返回结果输出到一个新的List中
         * 返回结果类型为List
         */
        dtoList = dishList.stream().map(item -> {
            DishDto dishDto = new DishDto();

            BeanUtils.copyProperties(item,dishDto);

            //获取categoryName，并添加到dishDto
            Long categoryId = item.getCategoryId();
            Category category = categoryService.getById(categoryId);
            if (category != null) {
                String categoryName = category.getName();
                dishDto.setCategoryName(categoryName);
            }

            Long dishId = item.getId();
            LambdaQueryWrapper<DishFlavor> lambdaQueryWrapper = new LambdaQueryWrapper<>();
            lambdaQueryWrapper.eq(DishFlavor::getDishId,dishId);
            List<DishFlavor> flavors = dishFlavorService.list(lambdaQueryWrapper);

            dishDto.setFlavors(flavors);

            return dishDto;
        }).collect(Collectors.toList());

        //如果不存在，需要查询数据库，将查询到的菜品数据缓存到Redis
        redisTemplate.opsForValue().set(key,dtoList,60, TimeUnit.MINUTES);

        return R.success(dtoList);
    }
}
```



### SetmealController

#### 分页查询套餐信息

```java
/**
 * 分页查询套餐信息
 * @param page
 * @param pageSize
 * @param name
 * @return
 */
@GetMapping("/page")
public R<IPage> page(int page, int pageSize, String name){
    //创建分页构造器
    IPage<Setmeal> pageInfo = new Page<>(page,pageSize);
    //页面需要接收套餐分类名称，因此需要用DTO加上CategoryName
    IPage<SetmealDto> pageDto = new Page<>();
    List<SetmealDto> setmealDtoList = new ArrayList<>();

    LambdaQueryWrapper<Setmeal> queryWrapper = new LambdaQueryWrapper<>();
    queryWrapper.like(name != null,Setmeal::getName,name);
    queryWrapper.orderByDesc(Setmeal::getUpdateTime);

    setmealService.page(pageInfo,queryWrapper);
    List<Setmeal> setmeals = pageInfo.getRecords();
    //拷贝pageInfo除records的属性到pageDto
    BeanUtils.copyProperties(pageInfo,pageDto,"records");

    for (Setmeal setmeal : setmeals) {
        SetmealDto setmealDto = new SetmealDto();
        BeanUtils.copyProperties(setmeal,setmealDto);

        Category category = categoryService.getById(setmeal.getCategoryId());
        if (category != null) {
            setmealDto.setCategoryName(category.getName());
        }

        setmealDtoList.add(setmealDto);
    }

    pageDto.setRecords(setmealDtoList);

    return R.success(pageDto);
}
```



#### 新增套餐

```java
    /**
     * 新增套餐，同时保存套餐对应的菜品信息
     * @param setmealDto
     * @return
     */
    @CacheEvict(value = "setmealCache",allEntries = true)
    @PostMapping
    public R<String> save(@RequestBody SetmealDto setmealDto){
        log.info(setmealDto.toString());
        setmealService.saveWithDishes(setmealDto);
        return R.success("添加成功");
    }
```



#### 删除套餐

```java
    /**
     * 删除套餐和套餐关联菜品
     * @param ids
     * @return
     */
    @CacheEvict(value = "setmealCache",allEntries = true)
    @DeleteMapping
    public R<String> delete(@RequestParam List<Long> ids){//@RequestParam从请求参数中获取
        setmealService.deleteWithDishes(ids);
        return R.success("删除成功");
    }
```



#### 修改套餐状态

```java
/**
 * 修套餐状态
 * @param status
 * @param ids
 * @return
 */
@PostMapping("/status/{status}")
public R<String> updateStatus(@PathVariable Integer status, @RequestParam List<Long> ids){
    Setmeal setmeal = new Setmeal();
    setmeal.setStatus(status);
    LambdaQueryWrapper<Setmeal> queryWrapper = new LambdaQueryWrapper<>();
    queryWrapper.in(Setmeal::getId,ids);
    setmealService.update(setmeal,queryWrapper);
    return R.success("修改成功");
}
```



#### 获取套餐信息

```java
/**
 * 获取套餐信息
 * @return
 */
@GetMapping("/{id}")
public R<SetmealDto> get(@PathVariable Long id){
    SetmealDto setmealDto = setmealService.getWithDishes(id);
    return R.success(setmealDto);
}
```



#### 修改套餐和对应菜品信息

```java
/**
 * 修改套餐和对应菜品信息
 * @return
 */
@PutMapping
public R<String> update(@RequestBody SetmealDto setmealDto){
    setmealService.updatewithDishes(setmealDto);
    return R.success("修改成功");
}
```



#### 根据条件查询对应的套餐信息

```java
/**
     * 根据条件查询对应的套餐信息
     * @param setmeal
     * @return
     */
    @Cacheable(value = "setmealCache",key = "#setmeal.categoryId + '_' + #setmeal.status")
    @GetMapping("/list")//setmeal/list?categoryId=1587734123836489730&status=1
    public R<List<Setmeal>> list(Setmeal setmeal){
        LambdaQueryWrapper<Setmeal> queryWrapper = new LambdaQueryWrapper<>();
        queryWrapper.eq(setmeal.getCategoryId() != null,Setmeal::getCategoryId,setmeal.getCategoryId());
        queryWrapper.eq(setmeal.getStatus() != null,Setmeal::getStatus,setmeal.getStatus());

        queryWrapper.orderByDesc(Setmeal::getPrice);

        List<Setmeal> list = setmealService.list(queryWrapper);

        return R.success(list);
    }
```



#### 查看套餐菜品列表

```java
/**
 * 查看套餐菜品列表
 * @param setmealId
 * @return
 */
@GetMapping("/dish/{setmealId}")
public R<List<SetmealDish>> dish(@PathVariable Long setmealId){
    LambdaQueryWrapper<SetmealDish> queryWrapper = new LambdaQueryWrapper<>();
    queryWrapper.eq(SetmealDish::getSetmealId,setmealId);

    List<SetmealDish> list = setmealDishService.list(queryWrapper);
    return R.success(list);
}
```



#### 汇总

```java
@RestController
@RequestMapping("/setmeal")
@Slf4j
public class SetmealController {

    @Autowired
    private SetmealService setmealService;

    @Autowired
    private SetmealDishService setmealDishService;

    @Autowired
    private CategoryService categoryService;

    /**
     * 分页查询套餐信息
     * @param page
     * @param pageSize
     * @param name
     * @return
     */
    @GetMapping("/page")
    public R<IPage> page(int page, int pageSize, String name){
        //创建分页构造器
        IPage<Setmeal> pageInfo = new Page<>(page,pageSize);
        //页面需要接收套餐分类名称，因此需要用DTO加上CategoryName
        IPage<SetmealDto> pageDto = new Page<>();
        List<SetmealDto> setmealDtoList = new ArrayList<>();

        LambdaQueryWrapper<Setmeal> queryWrapper = new LambdaQueryWrapper<>();
        queryWrapper.like(name != null,Setmeal::getName,name);
        queryWrapper.orderByDesc(Setmeal::getUpdateTime);

        setmealService.page(pageInfo,queryWrapper);
        List<Setmeal> setmeals = pageInfo.getRecords();
        //拷贝pageInfo除records的属性到pageDto
        BeanUtils.copyProperties(pageInfo,pageDto,"records");

        for (Setmeal setmeal : setmeals) {
            SetmealDto setmealDto = new SetmealDto();
            BeanUtils.copyProperties(setmeal,setmealDto);

            Category category = categoryService.getById(setmeal.getCategoryId());
            if (category != null) {
                setmealDto.setCategoryName(category.getName());
            }

            setmealDtoList.add(setmealDto);
        }

        pageDto.setRecords(setmealDtoList);
        return R.success(pageDto);
    }

    /**
     * 新增套餐，同时保存套餐对应的菜品信息
     * @param setmealDto
     * @return
     */
    @CacheEvict(value = "setmealCache",allEntries = true)
    @PostMapping
    public R<String> save(@RequestBody SetmealDto setmealDto){
        log.info(setmealDto.toString());
        setmealService.saveWithDishes(setmealDto);
        return R.success("添加成功");
    }

    /**
     * 删除套餐和套餐关联菜品
     * @param ids
     * @return
     */
    @CacheEvict(value = "setmealCache",allEntries = true)
    @DeleteMapping
    public R<String> delete(@RequestParam List<Long> ids){//@RequestParam从请求参数中获取
        setmealService.deleteWithDishes(ids);
        return R.success("删除成功");
    }

    /**
     * 修改套餐状态
     * @param status
     * @param ids
     * @return
     */
    @PostMapping("/status/{status}")
    public R<String> updateStatus(@PathVariable Integer status, @RequestParam List<Long> ids){
        Setmeal setmeal = new Setmeal();
        setmeal.setStatus(status);
        LambdaQueryWrapper<Setmeal> queryWrapper = new LambdaQueryWrapper<>();
        queryWrapper.in(Setmeal::getId,ids);
        setmealService.update(setmeal,queryWrapper);
        return R.success("修改成功");
    }

    /**
     * 获取套餐信息
     * @return
     */
    @GetMapping("/{id}")
    public R<SetmealDto> get(@PathVariable Long id){
        SetmealDto setmealDto = setmealService.getWithDishes(id);
        return R.success(setmealDto);
    }

    /**
     * 修改套餐和对应菜品信息
     * @return
     */
    @PutMapping
    public R<String> update(@RequestBody SetmealDto setmealDto){
        setmealService.updatewithDishes(setmealDto);
        return R.success("修改成功");
    }

    /**
     * 根据条件查询对应的套餐信息
     * @param setmeal
     * @return
     */
    @Cacheable(value = "setmealCache",key = "#setmeal.categoryId + '_' + #setmeal.status")
    @GetMapping("/list")//setmeal/list?categoryId=1587734123836489730&status=1
    public R<List<Setmeal>> list(Setmeal setmeal){
        LambdaQueryWrapper<Setmeal> queryWrapper = new LambdaQueryWrapper<>();
        queryWrapper.eq(setmeal.getCategoryId() != null,Setmeal::getCategoryId,setmeal.getCategoryId());
        queryWrapper.eq(setmeal.getStatus() != null,Setmeal::getStatus,setmeal.getStatus());

        queryWrapper.orderByDesc(Setmeal::getPrice);

        List<Setmeal> list = setmealService.list(queryWrapper);

        return R.success(list);
    }

    /**
     * 查看套餐菜品列表
     * @param setmealId
     * @return
     */
    @GetMapping("/dish/{setmealId}")
    public R<List<SetmealDish>> dish(@PathVariable Long setmealId){
        LambdaQueryWrapper<SetmealDish> queryWrapper = new LambdaQueryWrapper<>();
        queryWrapper.eq(SetmealDish::getSetmealId,setmealId);

        List<SetmealDish> list = setmealDishService.list(queryWrapper);
        return R.success(list);
    }

}
```



### UserController

#### 发送验证码

```java
   /**
     * 发送验证码
     * @param user
     * @param session
     * @return
     */
    @PostMapping("/sendMsg")//user/sendMsg
    //json:{phone: "13333333333"}
    public R<String> sendMessage(@RequestBody User user, HttpSession session){//需要在session中保存验证码
        //获取手机号
        String phone = user.getPhone();

        if (StringUtils.isNotEmpty(phone)){
            //生成随机四位验证码
            String code = ValidateCodeUtils.generateValidateCode(4).toString();
            log.info("code = {}",code);

            //调用发送短信工具类
            //SMSUtils.sendMessage("ABC商城",phone,code);

            //将生成的验证码保存到Session
            //session.setAttribute(phone,code);
            redisTemplate.opsForValue().set(phone,code,1, TimeUnit.MINUTES);

            //设置定时器，一分钟后清除session中的验证码
//            final Timer timer = new Timer();
//            timer.schedule(new TimerTask() {
//                public void run() {
//                    try{
//                        //删除session中的验证码
//                        session.removeAttribute(phone);
//                        timer.cancel();
//                        log.info("删除session中存的验证码");
//                    }catch (Exception e){
//                        log.error("移除错误");
//                    }
//                }
//            },1 * 60 * 1000);//一分钟

            return R.success("验证码发送成功");
        }
        return R.error("短信发送失败");
    }
```

#### 移动端用户登录

```java
   /**
     * 移动端用户登录
     * @param map
     * @param session
     * @return
     */
    @PostMapping("/login")//user/login
    //json:{phone: "13333333333", code: "1233"}
    public R<User> login(@RequestBody Map map,HttpSession session){

        //从map中获取手机号和验证码
        String phone = map.get("phone").toString();
        String code = map.get("code").toString();

        //从session中获取保存的验证码
        //Object codeInSession = session.getAttribute(phone);

        String rightCode = (String)redisTemplate.opsForValue().get(phone);

        if (rightCode == null){
            return R.error("验证码已失效");
        }

        //进行验证码对比
        if (rightCode.equals(code)){
            //登录成功
            //判断当前用户是否为新用户，如果是新用户自动完成注册
            LambdaQueryWrapper<User> queryWrapper = new LambdaQueryWrapper<>();
            queryWrapper.eq(User::getPhone,phone);
            User user = userService.getOne(queryWrapper);

            if (user == null){
                //新用户
                user = new User();
                user.setPhone(phone);
                //保存用户信息到数据库
                userService.save(user);
            }

            //不管是不是新用户都将user的id放入session,并把user返回
            session.setAttribute("user",user.getId());

            redisTemplate.delete(phone);

            return R.success(user);
        }

        return R.error("验证码错误，登录失败");
    }
```

#### 汇总

```java
@RestController
@RequestMapping("/user")
@Slf4j
public class UserController {

    @Autowired
    private RedisTemplate redisTemplate;

    @Autowired
    private UserService userService;

    /**
     * 发送验证码
     * @param user
     * @param session
     * @return
     */
    @PostMapping("/sendMsg")//user/sendMsg
    //json:{phone: "13333333333"}
    public R<String> sendMessage(@RequestBody User user, HttpSession session){//需要在session中保存验证码
        //获取手机号
        String phone = user.getPhone();

        if (StringUtils.isNotEmpty(phone)){
            //生成随机四位验证码
            String code = ValidateCodeUtils.generateValidateCode(4).toString();
            log.info("code = {}",code);

            //调用发送短信工具类
            //SMSUtils.sendMessage("ABC商城",phone,code);

            //将生成的验证码保存到Session
            //session.setAttribute(phone,code);
            redisTemplate.opsForValue().set(phone,code,1, TimeUnit.MINUTES);

            //设置定时器，一分钟后清除session中的验证码
//            final Timer timer = new Timer();
//            timer.schedule(new TimerTask() {
//                public void run() {
//                    try{
//                        //删除session中的验证码
//                        session.removeAttribute(phone);
//                        timer.cancel();
//                        log.info("删除session中存的验证码");
//                    }catch (Exception e){
//                        log.error("移除错误");
//                    }
//                }
//            },1 * 60 * 1000);//一分钟

            return R.success("验证码发送成功");
        }
        return R.error("短信发送失败");
    }

    /**
     * 移动端用户登录
     * @param map
     * @param session
     * @return
     */
    @PostMapping("/login")//user/login
    //json:{phone: "13333333333", code: "1233"}
    public R<User> login(@RequestBody Map map,HttpSession session){

        //从map中获取手机号和验证码
        String phone = map.get("phone").toString();
        String code = map.get("code").toString();

        //从session中获取保存的验证码
        //Object codeInSession = session.getAttribute(phone);

        String rightCode = (String)redisTemplate.opsForValue().get(phone);

        if (rightCode == null){
            return R.error("验证码已失效");
        }

        //进行验证码对比
        if (rightCode.equals(code)){
            //登录成功
            //判断当前用户是否为新用户，如果是新用户自动完成注册
            LambdaQueryWrapper<User> queryWrapper = new LambdaQueryWrapper<>();
            queryWrapper.eq(User::getPhone,phone);
            User user = userService.getOne(queryWrapper);

            if (user == null){
                //新用户
                user = new User();
                user.setPhone(phone);
                //保存用户信息到数据库
                userService.save(user);
            }

            //不管是不是新用户都将user的id放入session,并把user返回
            session.setAttribute("user",user.getId());

            redisTemplate.delete(phone);

            return R.success(user);
        }

        return R.error("验证码错误，登录失败");
    }
}

```

### AdressBookController

#### 新增地址簿

```java
/**
 * 新增
 */
@PostMapping
public R<AddressBook> save(@RequestBody AddressBook addressBook) {
    //从当前线程获取id
    addressBook.setUserId(BaseContext.getCurrentId());
    log.info("addressBook:{}", addressBook);
    addressBookService.save(addressBook);
    return R.success(addressBook);
}
```



#### 设置默认地址簿

```java
/**
 * 设置默认地址
 */
@PutMapping("default")
public R<AddressBook> setDefault(@RequestBody AddressBook addressBook) {
    log.info("addressBook:{}", addressBook);
    LambdaUpdateWrapper<AddressBook> wrapper = new LambdaUpdateWrapper<>();
    //找到所有当前user下的地址簿
    wrapper.eq(AddressBook::getUserId, BaseContext.getCurrentId());
    //把所有地址簿是否默认改为0
    wrapper.set(AddressBook::getIsDefault, 0);
    //SQL:update address_book set is_default = 0 where user_id = ?
    addressBookService.update(wrapper);

    //设置当前地址簿为默认
    addressBook.setIsDefault(1);
    //SQL:update address_book set is_default = 1 where id = ?
    addressBookService.updateById(addressBook);
    return R.success(addressBook);
}
```



#### 根据id查询地址

```java
/**
 * 根据id查询地址
 */
@GetMapping("/{id}")
public R get(@PathVariable Long id) {
    AddressBook addressBook = addressBookService.getById(id);
    if (addressBook != null) {
        return R.success(addressBook);
    } else {
        return R.error("没有找到该对象");
    }
}
```



#### 查询默认地址簿

```java
/**
 * 查询默认地址
 */
@GetMapping("default")
public R<AddressBook> getDefault() {
    LambdaQueryWrapper<AddressBook> queryWrapper = new LambdaQueryWrapper<>();
    //找到当前user所有地址中的默认地址
    queryWrapper.eq(AddressBook::getUserId, BaseContext.getCurrentId());
    queryWrapper.eq(AddressBook::getIsDefault, 1);

    //SQL:select * from address_book where user_id = ? and is_default = 1
    AddressBook addressBook = addressBookService.getOne(queryWrapper);

    if (null == addressBook) {
        return R.error("没有找到该对象");
    } else {
        return R.success(addressBook);
    }
}
```



#### 查询指定用户的全部地址

```java
/**
 * 查询指定用户的全部地址
 */
@GetMapping("/list")
public R<List<AddressBook>> list(AddressBook addressBook) {
    addressBook.setUserId(BaseContext.getCurrentId());
    log.info("addressBook:{}", addressBook);

    //条件构造器
    LambdaQueryWrapper<AddressBook> queryWrapper = new LambdaQueryWrapper<>();
    queryWrapper.eq(null != addressBook.getUserId(), AddressBook::getUserId, addressBook.getUserId());
    queryWrapper.orderByDesc(AddressBook::getUpdateTime);
    List<AddressBook> list = addressBookService.list(queryWrapper);

    //SQL:select * from address_book where user_id = ? order by update_time desc
    return R.success(list);
}
```



#### 汇总

```java
/**
 * 地址簿管理
 */
@Slf4j
@RestController
@RequestMapping("/addressBook")
public class AddressBookController {

    @Autowired
    private AddressBookService addressBookService;

    /**
     * 新增
     */
    @PostMapping
    public R<AddressBook> save(@RequestBody AddressBook addressBook) {
        //从当前线程获取id
        addressBook.setUserId(BaseContext.getCurrentId());
        log.info("addressBook:{}", addressBook);
        addressBookService.save(addressBook);
        return R.success(addressBook);
    }

    /**
     * 设置默认地址
     */
    @PutMapping("default")
    public R<AddressBook> setDefault(@RequestBody AddressBook addressBook) {
        log.info("addressBook:{}", addressBook);
        LambdaUpdateWrapper<AddressBook> wrapper = new LambdaUpdateWrapper<>();
        //找到所有当前user下的地址簿
        wrapper.eq(AddressBook::getUserId, BaseContext.getCurrentId());
        //把所有地址簿是否默认改为0
        wrapper.set(AddressBook::getIsDefault, 0);
        //SQL:update address_book set is_default = 0 where user_id = ?
        addressBookService.update(wrapper);

        //设置当前地址簿为默认
        addressBook.setIsDefault(1);
        //SQL:update address_book set is_default = 1 where id = ?
        addressBookService.updateById(addressBook);
        return R.success(addressBook);
    }

    /**
     * 根据id查询地址
     */
    @GetMapping("/{id}")
    public R get(@PathVariable Long id) {
        AddressBook addressBook = addressBookService.getById(id);
        if (addressBook != null) {
            return R.success(addressBook);
        } else {
            return R.error("没有找到该对象");
        }
    }

    /**
     * 查询默认地址
     */
    @GetMapping("default")
    public R<AddressBook> getDefault() {
        LambdaQueryWrapper<AddressBook> queryWrapper = new LambdaQueryWrapper<>();
        //找到当前user所有地址中的默认地址
        queryWrapper.eq(AddressBook::getUserId, BaseContext.getCurrentId());
        queryWrapper.eq(AddressBook::getIsDefault, 1);

        //SQL:select * from address_book where user_id = ? and is_default = 1
        AddressBook addressBook = addressBookService.getOne(queryWrapper);

        if (null == addressBook) {
            return R.error("没有找到该对象");
        } else {
            return R.success(addressBook);
        }
    }

    /**
     * 查询指定用户的全部地址
     */
    @GetMapping("/list")
    public R<List<AddressBook>> list(AddressBook addressBook) {
        addressBook.setUserId(BaseContext.getCurrentId());
        log.info("addressBook:{}", addressBook);

        //条件构造器
        LambdaQueryWrapper<AddressBook> queryWrapper = new LambdaQueryWrapper<>();
        queryWrapper.eq(null != addressBook.getUserId(), AddressBook::getUserId, addressBook.getUserId());
        queryWrapper.orderByDesc(AddressBook::getUpdateTime);
        List<AddressBook> list = addressBookService.list(queryWrapper);

        //SQL:select * from address_book where user_id = ? order by update_time desc
        return R.success(list);
    }
}
```



### ShoppingCartController

#### 添加到购物车

```java
/**
 * 添加到购物车
 * @param shoppingCart
 * @return
 */
@PostMapping("/add")
public R<ShoppingCart> add(@RequestBody ShoppingCart shoppingCart) {
    //获取当前用户Id
    Long userId = BaseContext.getCurrentId();
    shoppingCart.setUserId(userId);

    //查询当前菜品或套餐是否在购物车中
    Long dishId = shoppingCart.getDishId();
    LambdaQueryWrapper<ShoppingCart> queryWrapper = new LambdaQueryWrapper<>();
    queryWrapper.eq(ShoppingCart::getUserId, userId);

    if (dishId != null) {
        //添加的是菜品
        queryWrapper.eq(ShoppingCart::getDishId, dishId);
    } else {
        //添加的是套餐
        queryWrapper.eq(ShoppingCart::getSetmealId, shoppingCart.getSetmealId());
    }

    ShoppingCart one = shoppingCartService.getOne(queryWrapper);

    if (one != null) {
        //已经存在，在原来数量基础上加一
        one.setNumber(one.getNumber() + 1);
        shoppingCartService.updateById(one);
    } else {
        //不存在，添加到购物车，默认数量为1
        shoppingCart.setNumber(1);
        shoppingCartService.save(shoppingCart);
        one = shoppingCart;
    }
    return R.success(one);
}
```



#### 查询购物车

```java
/**
 * 查询购物车
 * @return
 */
@GetMapping("/list")
public R<List<ShoppingCart>> list() {
    Long currentId = BaseContext.getCurrentId();
    LambdaQueryWrapper<ShoppingCart> queryWrapper = new LambdaQueryWrapper<>();
    queryWrapper.eq(ShoppingCart::getUserId, currentId);

    List<ShoppingCart> list = shoppingCartService.list(queryWrapper);
    return R.success(list);
}
```



#### 减少数量

```java
    /**
     * 减少数量
     * @param shoppingCart
     * @return
     */
    @PostMapping("/sub")
    public R<ShoppingCart> sub(@RequestBody ShoppingCart shoppingCart) {
        LambdaQueryWrapper<ShoppingCart> queryWrapper = new LambdaQueryWrapper<>();
        queryWrapper.eq(ShoppingCart::getUserId, BaseContext.getCurrentId());
        Long dishId = shoppingCart.getDishId();
        Long setmealId = shoppingCart.getSetmealId();

        if (dishId != null) {
            //减少的是菜品
            queryWrapper.eq(ShoppingCart::getDishId, dishId);
        } else {
            //减少的是套餐
            queryWrapper.eq(ShoppingCart::getSetmealId, setmealId);
        }

        ShoppingCart one = shoppingCartService.getOne(queryWrapper);
        Integer number = one.getNumber();
        if (number > 1){
            one.setNumber(number - 1);
            shoppingCartService.updateById(one);
            return R.success(one);
        }else{
            shoppingCartService.removeById(one.getId());
            return R.success(new ShoppingCart());
        }
    }
```



#### 清空购物车

```java
/**
 * 清空购物车
 * @return
 */
@DeleteMapping("/clean")
public R<String> clean(){
    Long currentId = BaseContext.getCurrentId();
    LambdaQueryWrapper<ShoppingCart> queryWrapper = new LambdaQueryWrapper<>();
    queryWrapper.eq(ShoppingCart::getUserId,currentId);
    shoppingCartService.remove(queryWrapper);
    return R.success("清空成功");
}
```



#### 汇总

```java
@RestController
@Slf4j
@RequestMapping("/shoppingCart")
public class ShoppingCartController {

    @Autowired
    private ShoppingCartService shoppingCartService;

    /**
     * 添加到购物车
     * @param shoppingCart
     * @return
     */
    @PostMapping("/add")
    public R<ShoppingCart> add(@RequestBody ShoppingCart shoppingCart) {
        //获取当前用户Id
        Long userId = BaseContext.getCurrentId();
        shoppingCart.setUserId(userId);

        //查询当前菜品或套餐是否在购物车中
        Long dishId = shoppingCart.getDishId();
        LambdaQueryWrapper<ShoppingCart> queryWrapper = new LambdaQueryWrapper<>();
        queryWrapper.eq(ShoppingCart::getUserId, userId);

        if (dishId != null) {
            //添加的是菜品
            queryWrapper.eq(ShoppingCart::getDishId, dishId);
        } else {
            //添加的是套餐
            queryWrapper.eq(ShoppingCart::getSetmealId, shoppingCart.getSetmealId());
        }

        ShoppingCart one = shoppingCartService.getOne(queryWrapper);

        if (one != null) {
            //已经存在，在原来数量基础上加一
            one.setNumber(one.getNumber() + 1);
            shoppingCartService.updateById(one);
        } else {
            //不存在，添加到购物车，默认数量为1
            shoppingCart.setNumber(1);
            shoppingCartService.save(shoppingCart);
            one = shoppingCart;
        }
        return R.success(one);
    }

    /**
     * 查询购物车
     * @return
     */
    @GetMapping("/list")
    public R<List<ShoppingCart>> list() {
        Long currentId = BaseContext.getCurrentId();
        LambdaQueryWrapper<ShoppingCart> queryWrapper = new LambdaQueryWrapper<>();
        queryWrapper.eq(ShoppingCart::getUserId, currentId);

        List<ShoppingCart> list = shoppingCartService.list(queryWrapper);
        return R.success(list);
    }

    /**
     * 减少数量
     * @param shoppingCart
     * @return
     */
    @PostMapping("/sub")
    public R<ShoppingCart> sub(@RequestBody ShoppingCart shoppingCart) {
        LambdaQueryWrapper<ShoppingCart> queryWrapper = new LambdaQueryWrapper<>();
        queryWrapper.eq(ShoppingCart::getUserId, BaseContext.getCurrentId());
        Long dishId = shoppingCart.getDishId();
        Long setmealId = shoppingCart.getSetmealId();

        if (dishId != null) {
            //减少的是菜品
            queryWrapper.eq(ShoppingCart::getDishId, dishId);
        } else {
            //减少的是套餐
            queryWrapper.eq(ShoppingCart::getSetmealId, setmealId);
        }

        ShoppingCart one = shoppingCartService.getOne(queryWrapper);
        Integer number = one.getNumber();
        if (number > 1){
            one.setNumber(number - 1);
            shoppingCartService.updateById(one);
            return R.success(one);
        }else{
            shoppingCartService.removeById(one.getId());
            return R.success(new ShoppingCart());
        }
    }

    /**
     * 清空购物车
     * @return
     */
    @DeleteMapping("/clean")
    public R<String> clean(){
        Long currentId = BaseContext.getCurrentId();
        LambdaQueryWrapper<ShoppingCart> queryWrapper = new LambdaQueryWrapper<>();
        queryWrapper.eq(ShoppingCart::getUserId,currentId);
        shoppingCartService.remove(queryWrapper);
        return R.success("清空成功");
    }
}
```



### OrderController

```java
@RestController
@Slf4j
@RequestMapping("order")
public class OrderController {

    @Autowired
    private OrderService orderService;

    @PostMapping("/submit")
    public R<String> submit(@RequestBody Orders orders){
        orderService.submit(orders);
        return R.success("下单成功");
    }
}
```



## 定义过滤器

### LoginCheckFilter

```java
/**
 * 登录过滤器，检查用户是否已经登陆
 */
@Slf4j
//filterName：过滤器名称，urlPatterns：拦截路径
@WebFilter(filterName = "loginCheckFilter", urlPatterns = "/*")
public class LoginCheckFilter implements Filter {
    
    //因为equal方法无法识别通配符所以要用匹配器
    public static final AntPathMatcher PATH_MATCHER = new AntPathMatcher();

    //初始化方法,只在初始化时执行一次
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
    }

    //过滤方法，主要是对request和response进行一些处理，然后交给下一个过滤器或Servlet处理
    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        
        //把ServletRequest转成HttpServletRequest，这样可以调用一些独有方法
        HttpServletRequest request = (HttpServletRequest) servletRequest;
        HttpServletResponse response = (HttpServletResponse) servletResponse;

        //1.获取本次请求URI
        String requestURI = request.getRequestURI();

        //定义需要放行的请求路径数组
        String[] urls = new String[]{
                "/employee/login",
                "/employee/logout",
                "/backend/**",
                "/front/**"，
                "/user/sendMsg",//移动端发送短信
                "/user/login"//移动端登录
                "/doc.html",
                "/webjars/**",
                "/swagger-resources",
                "/v2/api-docs"
        };

        //2.判断本次请求是否需要处理
        boolean check = check(urls, requestURI);

        //3.如果不需要处理，则直接放行
        if (check){
            //filterChain过滤器链，调用filterChain.doFilter才可以访问下一个匹配的filter
            filterChain.doFilter(request,response);
            return;
        }

        //4-1.判断员工登录状态，如果已登录，则直接放行
        if (request.getSession().getAttribute("employee") != null){
            log.info("用户已登录，登录id为：{}", request.getSession().getAttribute("employee"));
            //把用户id放进线程局部变量中
            Long employeeId = (Long) request.getSession().getAttribute("employee");
            BaseContext.setCurrentId(employeeId);
            filterChain.doFilter(request,response);
            return;
        }

        //4-2.判断用户登录状态，如果已登录，则直接放行
        if (request.getSession().getAttribute("user") != null){
            log.info("用户已登录，登录id为：{}", request.getSession().getAttribute("user"));
            //把用户id放进线程局部变量中
            Long userId = (Long) request.getSession().getAttribute("user");
            BaseContext.setCurrentId(userId);
            filterChain.doFilter(request,response);
            return;
        }
        
        //5.如果未登录则返回未登录结果
        //JSON.toJSONString(user)：Java对象转JSON
        response.getWriter().write(JSON.toJSONString(R.error("NOTLOGIN")));
        return;
    }

    /**
     * 路径匹配，检查本次请求是否需要放行
     * @param urls
     * @param requestURI
     * @return
     */
    public boolean check(String[] urls, String requestURI){
        for (String url : urls) {
            boolean match = PATH_MATCHER.match(url, requestURI);
            if (match){
                return true;
            }
        }
        return false;
    }

    //销毁时调用
    @Override
    public void destroy() {
    }
}
```



## 定义异常处理器

### GlobalExceptionHandler

```java
/**
 * 全局异常处理
 * 当账号名重复时，违反了数据库的唯一约束条件，我们需要针对这个异常进行处理
 */
//@ControllerAdvice是为@ExceptionHandler修饰的方法所在的类提供的
@ControllerAdvice(annotations = {RestController.class, Controller.class})
//声明拦截的类，就是说加了这些注解的类会被拦截并处理
@ResponseBody//将方法返回值转为json格式的数据
@Slf4j
public class GlobalExceptionHandler{

    /**
     * 处理违反数据库唯一约束条件的异常
     * @return
     */
    //@ExceptionHandler用于捕获上边声明需要拦截的类中抛出的不同类型的异常，从而达到异常全局处理的目的
    @ExceptionHandler({SQLIntegrityConstraintViolationException.class})
    public R<String> exceptionHandler(SQLIntegrityConstraintViolationException ex){

        log.error(ex.getMessage());//Duplicate entry 'sss' for key 'idx_username'

        //如果异常信息里包含Duplicate entry，说明是账号名重复
        if(ex.getMessage().contains("Duplicate entry")){
            String[] split = ex.getMessage().split(" ");//用空格进行分割字符串
            String msg = split[2] + "已存在";//数组第三个就是异常信息里的重复内容
            return R.error(msg);
        }

        return R.error("未知错误");
    }
    
     /**
     * 处理删除时异常
     * 抛出自定义业务异常时，需要把异常消息返回给前端，在全局异常处理中把消息返回给前端
     * @param  ex
     * @return
     */
    @ExceptionHandler(CustomException.class)
    //用于捕获上边声明需要拦截的类中抛出的该类型的异常
    public R<String> exceptionHandler(CustomException ex){
        log.error(ex.getMessage());
        //把异常信息返回给前端
        return R.error(ex.getMessage());
    }
}
```

### CustomException

删除菜品分类或套餐分类时，如果已经关联菜品或套餐则不能删除，并抛出异常

```java
/**
 * 自定义业务异常
 */
//继承运行时异常
public class CustomException extends RuntimeException {
    //构造器
    public CustomException(String message) {
        super(message);
    }
}
```

## 定义MP配置类

### MybatisPlusConfig

配置MybatisPlus的分页插件

```java
/**
 * 配置MP的分页插件
 */
@Configuration //配置类
public class MybatisPlusConfig {

    @Bean
    public MybatisPlusInterceptor mpInterceptor(){

        //定义MP拦截器
        MybatisPlusInterceptor mpInterceptor = new MybatisPlusInterceptor();

        //添加具体拦截器:分页拦截器
        mpInterceptor.addInnerInterceptor(new PaginationInnerInterceptor());
        return mpInterceptor;
    }
}
```



##   定义元数据对象处理器

MybatisPlus公共字段自动填充，就是在插入或者更新的时候为指定字段赋予指定的值，使用它的好处是可以统一对这些字段进行处理，避免了重复代码

实现步骤：

1. 在实体类中的属性上加入@TableField注解，指定自动填充的策略
2. 按照框架要求编写元数据对象处理器，此类需要实现MetaObjectHandler接口

**ThreadLocal**

ThreadLocal并不是一个Thread，而是Thread的局部变量，当使用ThreadLocal维护变量时，ThreadLocal为每个使用该变量的线程提供独立的变量副本，所以每一个线程都可以独立地改变自己的副本，而不会影响其他线程所对应的副本。

ThreadLocal为每个线程提供单独一份存储空间，具有线程隔离的效果，只有在线程内才能获取到对应的值，线程外则不能访问。

ThreadLocal常用方法：

- public void set(T value) 设置当前线程的线程局部变量的值
- public T get() 返回当前线程所对应的线程局部变量的值

客户端发送的每次http请求，对应的在服务器端都会分配一个新的线程来处理，例如在处理过程中下面类中的方法都属于相同的一个线程，http请求为/employee，put方法

- LoginCheckFilter的doFilter方法
- EmployeeController的update方法
- MyMetaObjectHandler的updateFill方法

可以在上面三个方法中分别加入下面代码获取当前线程id

```java
long id = Thread.currentThread().getId();
```

可以在LoginCheckFilter的doFilter方法中获取当前登陆用户id，并调用ThreadLocal的set方法来设置当前的线程局部变量的值（用户id），然后再MyMetaObjectHandler的updateFill方法中调用ThreadLocal的get方法来获取当前线程所对应的线程局部变量的值。

1. 编写BaseContext工具类，基于ThreadLocal封装的工具类
2. 在LoginCheckFilter的doFilter方法中调用BaseContext来设置当前登录用户的id
3. 在MyMetaObjectHandler的方法中调用BaseContext来获取登录用户的id

### BaseContext

```java
/**
 * 编写BaseContext工具类，基于ThreadLocal封装的工具类
 * 设置和获取当前登录用户的id
 */
public class BaseContext {

    private static ThreadLocal<Long> threadLocal = new ThreadLocal<>();

    /**
     * 设置id
     * @param id
     */
    public static void setCurrentId(Long id){
        threadLocal.set(id);
    }

    /**
     * 获取id
     * @return
     */
    public static Long getCurrentId(){
        return threadLocal.get();
    }
}
```

### MyMetaObjectHandler

```java
/**
 *  MybatisPlus公共字段自动填充，就是在插入或者更新的时候为指定字段赋予指定的值
 *  按照框架要求编写元数据对象处理器，此类需要实现MetaObjectHandler接口
 */

@Component//定义bean
@Slf4j
public class MyMetaObjectHandler implements MetaObjectHandler {

     /**
     * 插入操作，自动填充
     * @param metaObject
     */
    @Override
    public void insertFill(MetaObject metaObject) {//metaObject里边封装着要插入的对象，如employee对象
        metaObject.setValue("createTime", LocalDateTime.now());
        metaObject.setValue("updateTime", LocalDateTime.now());
        metaObject.setValue("updateUser", BaseContext.getCurrentId());//从线程局部变量中取出用户id
        metaObject.setValue("createUser", BaseContext.getCurrentId());
    }

    /**
     * 更新操作自动填充
     * @param metaObject
     */
    @Override
    public void updateFill(MetaObject metaObject) {
        metaObject.setValue("updateTime", LocalDateTime.now());
        metaObject.setValue("updateUser", BaseContext.getCurrentId());//从线程局部变量中取出用户id
    }
}
```



## 定义数据传输对象DTO

1. 减少原生对象的多余参数。包括为了安全，有时候也为了节约流量。例如：密码，你就不能返回到前端。因为不安全。 其次假如说：获取博客列表的时候，也不能返回博客全文吧。顶多就返回标题，id，和前几句。
2. 减少重复代码。组装对象。假设用户的基本信息，详细信息，以及一些额外的附加数据，现在要返回给前台或者从前台接收，你可以选择拼成map返回。但是map的缺点是，无法复用。假如需要增加或者删除某个参数还好，假如某个参数名写错了，而你又有几十个地方用到了这个单词，那就需要改几十个。而DTO,你只需要改一处即可。其次，可以规范参数类型，因为map返回大多都是string直接返回，在存数据的时候也无法校验数据类型是否是想要的类型。假如前台要的单位是分，你后台直接传了个浮点回去，前端就会找你麻烦。但是如果用DTO，你直接把前端的数据要求放进DTO里，你如果直接把不符合要求的类型放进去，就会报错。同时，也更便于你在DTO类里直接写注释，便于别人阅读。



### DishDto

```java
@Data
public class DishDto extends Dish {

    //为了接收页面传递的flavors
    private List<DishFlavor> flavors = new ArrayList<>();

    //分类名称
    private String categoryName;

    private Integer copies;
}
```



### SetmealDto

```java
@Data
public class SetmealDto extends Setmeal {

    //套餐包含的菜品列表	
    private List<SetmealDish> setmealDishes;

    private String categoryName;
}
```

## 定义工具类

**短信发送工具类**

市面上很多第三方提供的短信服务，他们会和各个运营商对接，我们只需要注册成为会员，并按照提供的开发文档进行调用就可以发送短信。调用API或者群发助手，即可发送验证码，通知类和营销类短信。

[阿里云短信发送](https://help.aliyun.com/document_detail/112148.html)

导入maven坐标

```xml
        <dependency>
            <groupId>com.aliyun</groupId>
            <artifactId>aliyun-java-sdk-core</artifactId>
            <version>4.5.16</version>
        </dependency>

        <dependency>
            <groupId>com.aliyun</groupId>
            <artifactId>aliyun-java-sdk-dysmsapi</artifactId>
            <version>2.1.0</version>
        </dependency>
```

### SMSUtils

```java
public class SMSUtils{

    /**
     * 发送短信
     * @param signName 签名
     * @param phoneNumbers 手机号
     * @param param 参数
     */
    public static void sendMessage(String signName,String phoneNumbers,String param) {
        DefaultProfile profile = DefaultProfile.getProfile("cn-hangzhou", "LTAI4G29eFjgKFPDUYFMSkUM", "jVJOurME74scJHP3wN9zMnb5ygXMp2");
        /**
         "<region-id>",           // 地区ID
         "<access-key-id>",       // 申请的access-key-id
         "<access-key-secret>",   // 申请的access-key-secret
         **/
        IAcsClient client = new DefaultAcsClient(profile);

        SendSmsRequest request = new SendSmsRequest();
        request.setPhoneNumbers(phoneNumbers);//接收短信的手机号码
        request.setSignName(signName);//短信签名名称
        request.setTemplateCode("SMS_206750105");//短信模板CODE，在阿里云模板管理里查询
        request.setTemplateParam("{\"code\":\""+param+"\"}");//短信模板中变量对应的实际值，如${code}

        try {
            SendSmsResponse response = client.getAcsResponse(request);
            System.out.println("短信发送成功");
        } catch (ClientException e) {
            e.printStackTrace();
        }

    }
}
```



**验证码工具类**

手机验证码登录优点：

- 方便快捷，无需注册，直接登录
- 使用验证码作为登录凭证，无需密码
- 安全

手机号是区分不同用户的标识

### ValidateCodeUtils

```java
/**
 * 随机生成验证码工具类
 */
public class ValidateCodeUtils {
    /**
     * 随机生成验证码
     * @param length 长度为4位或者6位
     * @return
     */
    public static Integer generateValidateCode(int length){
        Integer code =null;
        if(length == 4){
            code = new Random().nextInt(9999);//生成随机数，最大为9999
            if(code < 1000){
                code = code + 1000;//保证随机数为4位数字
            }
        }else if(length == 6){
            code = new Random().nextInt(999999);//生成随机数，最大为999999
            if(code < 100000){
                code = code + 100000;//保证随机数为6位数字
            }
        }else{
            throw new RuntimeException("只能生成4位或6位数字验证码");
        }
        return code;
    }

    /**
     * 随机生成指定长度字符串验证码
     * @param length 长度
     * @return
     */
    public static String generateValidateCode4String(int length){
        Random rdm = new Random();
        String hash1 = Integer.toHexString(rdm.nextInt());
        String capstr = hash1.substring(0, length);
        return capstr;
    }
}
```



## 为什么需要AtomicInteger原子操作类？

对于Java中的运算操作，例如自增或自减，若没有进行额外的同步操作，在多线程环境下就是线程不安全的。num++解析为num=num+1，明显，这个操作不具备原子性，多线程并发共享这个变量时必然会出现问题。

# 前后端分离开发

**开发流程**

1.定制接口

这里的接口并不是interface，接口(API接口)就是一个http的请求地址，主要就是去定义：请求路径，请求方式，请求参数，响应数据等内容

2.前端开发：后端自测

2.后端开发：mock数据

3.连调：校验格式

4.提测：自动化测试

**前端技术栈**

开发工具：Visual Studio Code、hbuilder

技术框架

- nodejs：前端开发的基石
- VUE
- ElementUI
- mock：模拟数据进行测试
- webpack：前端打包

## YAPI

YAPI是高小、易用、功能强大的api管理平台，旨在为开发、产品、测试人员提供更优雅的接口管理服务。可以帮助开发者轻松创建、发布、维护API，YAPI还为用户提供了优秀的交互体验，开发人员只需利用平台提供的接口数据写入工具以及简单的点击操作就可以实现接口的管理。YAPI让接口开发更简单高效，让接口的管理更具可读性，可维护性，让团队协作更合理。

源码地址：https://github.com/YMFE/yapi

要使用YAPI需要自己进行部署

**功能**

- 添加项目
- 添加分类
- 添加接口
- 编辑接口
- 查看接口

## Swagger

使用Swagger你只需要按照它的规范取定义接口及接口相关的信息，再通过Swagger衍生出来的一系列项目和工具，就可以做到生成各种格式的接口文档，以及在线接口调试页面等

官网：https://swagger.io/

knife4j是为Java MVC框架集成Swagger生成Api文档的增强解决方案。

操作步骤

- 导入knife4j的maven坐标

  ```xml
  <dependency>
    <groupId>com.github.xiaoymin</groupId>
    <artifactId>knife4j-spring-boot-starter</artifactId>
    <version>3.0.2</version>
  </dependency>
  ```

- 导入knife4j相关配置类

  在WebMvcConfig类上加上@EnableSwagger2和@EnableKnife4j注解

  创建Docket对象

  ```java
      /**
       * 创建Docket对象
       * @return
       */
      @Bean
      public Docket createRestApi(){
        //文档类型
        return new Docket(DocumentationType.SWAGGER_2)
           .apiInfo(apiInfo())
           .select()
           .apis(RequestHandlerSelectors.basePackage("com.zxp.controller"))//controller包所在位置
           .paths(PathSelectors.any())
           .build();
      }
  
      /**
       * 接口文档的描述
       * @return
       */
      private ApiInfo apiInfo(){
          return new ApiInfoBuilder()
                  .title("tt外卖")
                  .version("1.0")
                  .description("外卖接口文档")
                  .build();
      }
  ```

- 设置静态资源，否则接口文档页面无法访问

  在WebMvcConfig中的addResourceHandlers方法中加入

  ```java
  //访问接口文档时需要请求doc.html，doc.html是框架已经写好的
  registry.addResourceHandler("doc.html").addResourceLocations("classpath:/META-INF/resources/");
  registry.addResourceHandler("/webjars/**").addResourceLocations("classpath:/META-INF/resources/webjars/");
  ```

- 在LoginCheckFilter中设置不需要处理的请求路径

  在不需要处理的请求路径中加上

  ```
  "/doc.html",
  "/webjars/**",
  "/swagger-resources",
  "/v2/api-docs"
  ```

**常用注解**

- @Api：用在请求的类上，例如Controller，表示对类的说明

  ```java
  @Api(tags = "套餐相关接口")
  public class SetmealController{}
  ```

- @ApiModel：用在类上，通常是实体类，例如Setmeal，表示一个返回响应数据的消息

  ```java
  @ApiModel("套餐")
  public class Setmeal{}
  ```

- @ApiModelProperty：用在属性上，例如id，描述响应类的属性

  ```java
  @ApiModelProperty("主键")
  private Long id;
  ```

- @ApiOperation：用在请求的方法上，说明方法的用途，作用

  ```java
  @ApiOperation(value = "新增套餐接口")
  public R<String> save(){}
  ```

- @ApiImplicitParams：用在请求的方法上，表示一组参数说明

- @ApiImplicitParam：用在@ApiImplicitParams注解中，指定一个请求参数的各个方面

  ```java
  @ApiImplicitParams({
      @ApiImplicitParam(name = "page",value = "页码",required = true),
      @ApiImplicitParam(name = "pageSize",value = "每页记录数",required = true),
      @ApiImplicitParam(name = "name",value = "套餐名称",required = false)
  })
  public R<Page> page(int page,int pageSize,String name){}
  ```

# 项目部署

**部署前端项目**

1. 在服务器A安装Nginx，将前端资源dist文件上传到Nginx的html目录

2. 修改Nginx的配置文件

   ```
   server{
     listen  80;
     server_name  localhost;
     
     location / {
       root   html/dist;  #指定根目录为dist
       index  index.html; #首页，这里的index.html是dist文件夹下的
     }
     
     #反向代理配置
     location ^~ /api/{
       rewirte ^/api/(.*)$ /$1 break;  #url重写
       proxy_pass http://B的ip:8080;
     }
   }
   ```

   前端项目一般会带一个固定的前缀，例如http://192.168.138.100/api/employee/login

   rewrite ^/api/(.*)$ /$1 break;的作用是把它变成http://192.168.138.100/employee/login

   proxy_pass http://B的ip:8080;的作用是把他转发到服务器B:8080

**部署后端项目**

1. 在服务器B中安装，jdk、git、maven、Mysql，使用git命令进行仓库克隆

2. 编写脚本

   ```sh
   #!/bin/sh
   echo =================================
   echo  自动化部署脚本启动
   echo =================================
    
   echo 停止原来运行中的工程
   APP_NAME=TT-0.0.1-SNAPSHOT.jar
    
   tpid=`ps -ef|grep $APP_NAME|grep -v grep|grep -v kill|awk '{print $2}'`
   if [ ${tpid} ]; then
       echo 'Stop Process...'
       kill -15 $tpid
   fi
   sleep 2
   tpid=`ps -ef|grep $APP_NAME|grep -v grep|grep -v kill|awk '{print $2}'`
   if [ ${tpid} ]; then
       echo 'Kill Process!'
       kill -9 $tpid
   else
       echo 'Stop Success!'
   fi
    
   echo 准备从Git仓库拉取最新代码
   cd /usr/local/TT
    
   echo 开始从Git仓库拉取最新代码
   git pull
   echo 代码拉取完成
    
   echo 开始打包
   output=`mvn clean package -Dmaven.test.skip=true`
    
   cd target
    
   echo 启动项目
   nohup java -jar TT-0.0.1-SNAPSHOT.jar &> tt.log &
   echo 项目启动完成
   ```

   