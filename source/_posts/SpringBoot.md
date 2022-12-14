---
title: SpringBoot笔记
date: 2022-05-12
tags: SpringBoot
categories: 笔记
description: SpringBoot笔记
cover: https://zhuxinpeng.oss-cn-hangzhou.aliyuncs.com/9.jpg
---
# 基础篇

把Tomcat服务器更换成Jetty服务器:排除Tomcat依赖更换为Jetty

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
        <exclusions>
            <exclusion>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-tomcat</artifactId>
            </exclusion>
        </exclusions>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-jetty</artifactId>
    </dependency>
</dependencies>
```

通过application.properties文件修改端口：

```properties
server.port=80
```

配置文件提示词消失：

- 打开Project Structure
- 选择Facets，并选择所在模块
- 点击绿叶子标志

# 运维篇

**端口被占用问题**

- 查询端口

  ```shell
  netstat -ano
  ```

- 查询指定端口

  ```shell
  netstat -ano |findstr "端口号"
  ```

- 根据进程PID查询进程名称

  ```shell
  tasklist |findstr "进程PID号"
  ```

- 根据PID杀死任务

  ```shell
  taskkill /F /PID "进程PID号"
  ```

- 根据进程名称杀死任务

  ```shell
  taskkill -f -t -im "进程名称"
  ```

**Linux快速启动工程**

1. 修改配置文件端口，数据库密码等

2. 把项目进行打包

3. 上传到Linux的usr/local/app并进入该文件夹

4. java -jar springBoot_ssm-0.0.1-SNAPSHOT.jar

5. 一般第四步改为后台启动并设定日志

   ```shell
   nohub java -jar springBoot_ssm-0.0.1-SNAPSHOT.jar > server.log 2>&1 &
   ```

6. 如果想要关闭

   ```shell
   ps -ef | grep "java -jar"
   ```

7. 获取对应uid：第二行

   ```shell
   kill -9 uid
   ```

**日志：**

```yaml
logging:
  level:
    root: debug
```

分组设置级别

```yaml
logging:
  group:
    ebank: com.zxp.dao,com.zxp.service
  level:
    root: debug  #日志级别为debug
    ebank: warn  #该组包下日志级别为warn
```

**注解添加日志**

- 添加lombok坐标

- 在需要日志的类上用@Slf4j注解

  相当于[private](https://so.csdn.net/so/search?q=private&spm=1001.2101.3001.7020) final Logger logger = LoggerFactory.getLogger(当前类名.class);

**日志输出格式**

![image-20211206123431222](D:\学习\SpringBoot\运维实用篇-资料\讲义\img\image-20211206123431222.png)

PID：进程ID，用于表明当前操作所处的进程

所属类/接口名：当前显示信息为SpringBoot重写后信息，名称过长时，简化包名为首字母

设置日志模板格式：

```yaml
logging:
 pattern:
  console: "%d - %m %n"  # %d:时间 %m:信息 %n:换行 %p:级别 %t:线程  %c:类名
  dateformat: MM-dd HH:mm:ss:SSS #日期格式
```

生成日志文件

```yaml
logging:
    file: 
      name: xxx.log
    logback:
      rollingpolicy:
        max-file-size: xxKB/MB   #日志文件达到多少时分割
        file-name-pattern: xx.%d{yyyy-MM-dd}.%i.log    #分割后的文件名格式
```

# 开发篇

## **热部署**

热部署简单说就是程序改了，不用重启，服务器会自己把更新后的程序给重新加载一遍

- 配置坐标

  ```xml
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-devtools</artifactId>
      <optional>true</optional>
  </dependency>
  ```

- 在设置种选择Compiler

- 勾选Build project automatically

- ctrl + alt + shift选择registry

- 勾选compiler.automake.....

**配置中默认不参与热部署的目录**

- /META-INF/maven
- /META-INF/resources
- /resources
- /static
- /public
- /templates

**设置不参与热部署的文件或文件夹**

```yaml
spring:
  devtools:
    restart:
      #设置不参与热部署的文件或文件夹
      exclude: static/**,public/**,config/application.yml
```

**关闭热部署**

```yaml
spring:
  devtools:
    restart:
      enabled: false
```

**使用@Bean注解定义第三方bean**

```java
@Bean
public DruidDataSource datasource(){
    DruidDataSource ds = new DruidDataSource();
    return ds;
}
```

```yaml
datasource:
  driverClassName: com.mysql.jdbc.Driver
```

```java
@Bean
@ConfigurationProperties(prefix = "datasource")
public DruidDataSource datasource(){
    DruidDataSource ds = new DruidDataSource();
    return ds;
}
```

@EnableConfigurationProperties:标注使用@ConfigurationProperties注解绑定属性的bean是哪些。

- 在配置类上开启@EnableConfigurationProperties注解，并标注要使用@ConfigurationProperties注解绑定属性的类

  ```java
  @SpringBootApplication
  @EnableConfigurationProperties(ServerConfig.class)
  public class Springboot13ConfigurationApplication {
  }
  ```

- 在对应的类上直接使用@ConfigurationProperties进行属性绑定

  ```java
  @Data
  @ConfigurationProperties(prefix = "servers")
  public class ServerConfig {
      private String ipAddress;
      private int port;
      private long timeout;
  }
  ```

- 使用@EnableConfigurationProperties注解时，spring会默认将其标注的类定义为bean，因此无需再次声明@Component注解

## **松散绑定**

springboot进行编程时人性化设计的一种体现，即配置文件中的命名格式与变量名的命名格式可以进行格式上的最大化兼容，@ConfigurationProperties绑定属性支持属性名宽松绑定

在ServerConfig中的ipAddress属性名

```java
@Component
@Data
@ConfigurationProperties(prefix = "servers")
public class ServerConfig {
    private String ipAddress;
}
```

可以与下面的配置属性名规则全兼容

```yaml
servers:
  ipAddress: 192.168.0.2       # 驼峰模式
  ip_address: 192.168.0.2      # 下划线模式
  ip-address: 192.168.0.2      # 烤肉串模式
  IP_ADDRESS: 192.168.0.2      # 常量模式
```

## **常用计量单位应用**

**Duration**：表示时间间隔，可以通过@DurationUnit注解描述时间单位，例如上例中描述的单位为小时（ChronoUnit.HOURS）

**DataSize**：表示存储空间，可以通过@DataSizeUnit注解描述存储空间单位，例如上例中描述的单位为MB（DataUnit.MEGABYTES）

```java
@Component
@Data
@ConfigurationProperties(prefix = "servers")
public class ServerConfig {
    @DurationUnit(ChronoUnit.HOURS)
    private Duration serverTimeOut;
    @DataSizeUnit(DataUnit.MEGABYTES)
    private DataSize dataSize;
}
```

## **数据校验**

在JAVAEE的JSR303规范中给出了具体的数据校验标准，开发者可以根据需要选择对应的校验框架

bean属性校验

- 导入坐标

  ```xml
  <!--1.导入JSR303规范-->
  <dependency>
      <groupId>javax.validation</groupId>
      <artifactId>validation-api</artifactId>
  </dependency>
  <!--使用hibernate框架提供的校验器做实现-->
  <dependency>
      <groupId>org.hibernate.validator</groupId>
      <artifactId>hibernate-validator</artifactId>
  </dependency>
  ```

- 在需要开启校验功能的类上使用注解@Validated开启校验功能

  ```java
  @Component
  @Data
  @ConfigurationProperties(prefix = "servers")
  //开启对当前bean的属性注入校验
  @Validated
  public class ServerConfig {
      //设置具体的规则
      @Max(value = 8888,message = "最大值不能超过8888")
      @Min(value = 202,message = "最小值不能低于202")
      private int port;
  }
  ```

## 测试

**加载测试专用属性**：

- 临时属性

  ```java
  //properties属性可以为当前测试用例添加临时的属性配置
  @SpringBootTest(properties = {"test.prop=testValue1"})
  public class PropertiesAndArgsTest {
  
      @Value("${test.prop}")
      private String msg;
      
      @Test
      void testProperties(){
          System.out.println(msg);
      }
  }
  ```

- 临时参数

  ```java
  //args属性可以为当前测试用例添加临时的命令行参数
  @SpringBootTest(args={"--test.prop=testValue2"})
  public class PropertiesAndArgsTest {
      
      @Value("${test.prop}")
      private String msg;
      
      @Test
      void testProperties(){
          System.out.println(msg);
      }
  }
  ```

**加载测试专用配置**

在测试环境中再添加一个配置类，然后启动测试环境时，生效此配置

- 在测试包test中创建专用的测试环境配置类

  ```java
  @Configuration
  public class MsgConfig {
      @Bean
      public String msg(){
          return "bean msg";
      }
  }
  ```

- 在启动测试环境时，导入测试环境专用的配置类，使用@Import注解即可实现

  ```java
  @SpringBootTest
  @Import({MsgConfig.class})
  public class ConfigurationTest {
  
      @Autowired
      private String msg;
  
      @Test
      void testConfiguration(){
          System.out.println(msg);
      }
  }
  ```

**Web环境模拟测试**

测试类中启动web环境:

```java
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class WebTest {	
}
```

- MOCK：根据当前设置确认是否启动web环境，例如使用了Servlet的API就启动web环境，属于适配性的配置
- DEFINED_PORT：使用自定义的端口作为web服务器端口
- RANDOM_PORT：使用随机端口作为web服务器端口
- NONE：不启动web环境

**测试类中发送请求**

- 在测试类中开启web虚拟调用功能，通过注解@AutoConfigureMockMvc实现此功能的开启

  ```java
  @SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
  //开启虚拟MVC调用
  @AutoConfigureMockMvc
  public class WebTest {
  }
  ```

- 定义发起虚拟调用的对象MockMVC，通过自动装配的形式初始化对象

  ```java
  @SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
  //开启虚拟MVC调用
  @AutoConfigureMockMvc
  public class WebTest {
  
      @Test
      void testWeb(@Autowired MockMvc mvc) {
      }
  }
  ```

- 创建一个虚拟请求对象，封装请求的路径，并使用MockMVC对象发送对应请求

  ```java
  @SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
  //开启虚拟MVC调用
  @AutoConfigureMockMvc
  public class WebTest {
  
      @Test
      void testWeb(@Autowired MockMvc mvc) throws Exception {
          //http://localhost:8080/books
          //创建虚拟请求，当前访问/books
    MockHttpServletRequestBuilder builder = MockMvcRequestBuilders.get("/books");
          //执行对应的请求
          mvc.perform(builder);
      }
  }
  ```

**web环境请求结果比对**

- 响应状态匹配

  ```java
  @Test
  void testStatus(@Autowired MockMvc mvc) throws Exception {
      MockHttpServletRequestBuilder builder = MockMvcRequestBuilders.get("/books");
      ResultActions action = mvc.perform(builder);
      //设定预期值 与真实值进行比较，成功测试通过，失败测试失败
      //定义本次调用的预期值
      StatusResultMatchers status = MockMvcResultMatchers.status();
      //预计本次调用时成功的：状态200
      ResultMatcher ok = status.isOk();
      //添加预计值到本次调用过程中进行匹配
      action.andExpect(ok);
  }
  ```

- 响应体匹配（非json数据格式）

  ```java
  @Test
  void testBody(@Autowired MockMvc mvc) throws Exception {
      MockHttpServletRequestBuilder builder = MockMvcRequestBuilders.get("/books");
      ResultActions action = mvc.perform(builder);
      //设定预期值 与真实值进行比较，成功测试通过，失败测试失败
      //定义本次调用的预期值
      ContentResultMatchers content = MockMvcResultMatchers.content();
      ResultMatcher result = content.string("springboot2");
      //添加预计值到本次调用过程中进行匹配
      action.andExpect(result);
  }
  ```

- 响应体匹配（json数据格式，开发中的主流使用方式）

  ```java
  @Test
  void testJson(@Autowired MockMvc mvc) throws Exception {
      MockHttpServletRequestBuilder builder = MockMvcRequestBuilders.get("/books");
      ResultActions action = mvc.perform(builder);
      //设定预期值 与真实值进行比较，成功测试通过，失败测试失败
      //定义本次调用的预期值
      ContentResultMatchers content = MockMvcResultMatchers.content();
      ResultMatcher result = content.json("{\"id\":1,\"name\":\"springboot2\",\"type\":\"springboot\"}");
      //添加预计值到本次调用过程中进行匹配
      action.andExpect(result);
  }
  ```

- 响应头信息匹配

  ```java
  @Test
  void testContentType(@Autowired MockMvc mvc) throws Exception {
      MockHttpServletRequestBuilder builder = MockMvcRequestBuilders.get("/books");
      ResultActions action = mvc.perform(builder);
      //设定预期值 与真实值进行比较，成功测试通过，失败测试失败
      //定义本次调用的预期值
      HeaderResultMatchers header = MockMvcResultMatchers.header();
      ResultMatcher contentType = header.string("Content-Type", "application/json");
      //添加预计值到本次调用过程中进行匹配
      action.andExpect(contentType);
  }
  ```

- 完整的信息匹配过程

  ```java
  @Test
  void testGetById(@Autowired MockMvc mvc) throws Exception {
      MockHttpServletRequestBuilder builder = MockMvcRequestBuilders.get("/books");
      ResultActions action = mvc.perform(builder);
  
      StatusResultMatchers status = MockMvcResultMatchers.status();
      ResultMatcher ok = status.isOk();
      action.andExpect(ok);
  
      HeaderResultMatchers header = MockMvcResultMatchers.header();
      ResultMatcher contentType = header.string("Content-Type", "application/json");
      action.andExpect(contentType);
  
      ContentResultMatchers content = MockMvcResultMatchers.content();
      ResultMatcher result = content.json("{\"id\":1,\"name\":\"springboot\",\"type\":\"springboot\"}");
      action.andExpect(result);
  }
  ```

**数据层测试回滚**

​    在打包的阶段由于test生命周期属于必须被运行的生命周期，如果跳过会给系统带来极高的安全隐患，所以测试用例必须执行。测试用例如果测试时产生了事务提交就会在测试过程中对数据库数据产生影响，进而产生垃圾数据。在原始测试用例中添加注解@Transactional即可实现当前测试用例的事务不提交。当程序运行后，只要注解@Transactional出现的位置存在注解@SpringBootTest，springboot就会认为这是一个测试程序，无需提交事务，所以也就可以避免事务的提交。

```java
@SpringBootTest
@Transactional
@Rollback(true)
public class DaoTest {
    @Autowired
    private BookService bookService;

    @Test
    void testSave(){
        Book book = new Book();
        book.setName("springboot3");
        book.setType("springboot3");
        book.setDescription("springboot3");

        bookService.save(book);
    }
}
```

**测试用例随机数据设定**

```yaml
testcase:
  book:
    id: ${random.int}
    id2: ${random.int(10)} #0-10的整数
    type: ${random.int(5,10)} #5-10的整数
    name: ${random.value} #随机字符串，MD5字符串，32位
    uuid: ${random.uuid}
    publishTime: ${random.long} 
```

数据的加载按照之前加载数据的形式，使用@ConfigurationProperties注解即可

```java
@Component
@Data
@ConfigurationProperties(prefix = "testcase.book")
public class BookCase {
    private int id;
    private int id2;
    private int type;
    private String name;
    private String uuid;
    private long publishTime;
}
```

## 数据层解决方案

### **SQL**

- 数据源技术：Druid
- 持久化技术：MyBatisPlus
- 数据库技术：MySQL

#### **数据源技术**

springboot提供了3款内嵌数据源技术，分别如下：

- HikariCP
- Tomcat提供DataSource
- Commons DBCP

第一种，HikartCP，这是springboot官方推荐的数据源技术，作为默认内置数据源使用

第二种，Tomcat提供的DataSource，如果不使用HikartCP，并且使用tomcat作为web服务器进行web程序的开发，使用这个。

第三种，DBCP，既不使用HikartCP也不使用tomcat的DataSource时，默认给你用这个。

- druid对应配置（导入druid-stater坐标）

  ```yaml
  spring:
    datasource:
      druid:	
     	  url: jdbc:mysql://localhost:3306/ssm_db?serverTimezone=UTC
        driver-class-name: com.mysql.cj.jdbc.Driver
        username: root
        password: root
  ```

- HikariCP：默认数据源直接把druid删掉就可以

  ```yaml
  spring:
    datasource:
      url: jdbc:mysql://localhost:3306/ssm_db?serverTimezone=UTC
      driver-class-name: com.mysql.cj.jdbc.Driver
      username: root
      password: root
  ```

  或者

  ```yaml
  spring:
    datasource:
      url: jdbc:mysql://localhost:3306/ssm_db?serverTimezone=UTC
      hikari:
        driver-class-name: com.mysql.cj.jdbc.Driver
        username: root
        password: root
        maximum-pool-size: 50 #连接池最大数量，可以不写
  ```

#### **持久化技术**

JdbcTemplate：springboot给开发者提供的一套现成的数据层技术

- 导入jdbc对应的坐标

  ```xml
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-jdbc</artifactId>
  </dependency
  ```

- 自动装配JdbcTemplate对象

  ```java
  @SpringBootTest
  class Springboot15SqlApplicationTests {
      @Test
      void testJdbcTemplate(@Autowired JdbcTemplate jdbcTemplate){
      }
  }
  ```

- 使用JdbcTemplate实现查询操作（非实体类封装数据的查询操作）

  ```java
  @Test
  void testJdbcTemplate(@Autowired JdbcTemplate jdbcTemplate){
      String sql = "select * from tbl_book";
      List<Map<String, Object>> maps = jdbcTemplate.queryForList(sql);
      System.out.println(maps);
  }
  ```

- 使用JdbcTemplate实现查询操作（实体类封装数据的查询操作）

  ```java
  @Test
  void testJdbcTemplate(@Autowired JdbcTemplate jdbcTemplate){
  
      String sql = "select * from tbl_book";
      RowMapper<Book> rm = new RowMapper<Book>() {
          @Override
          public Book mapRow(ResultSet rs, int rowNum) throws SQLException {
              Book temp = new Book();
              temp.setId(rs.getInt("id"));
              temp.setName(rs.getString("name"));
              temp.setType(rs.getString("type"));
              temp.setDescription(rs.getString("description"));
              return temp;
          }
      };
      List<Book> list = jdbcTemplate.query(sql, rm);
      System.out.println(list);
  }
  ```

- 使用JdbcTemplate实现增删改操作

  ```java
  @Test
  void testJdbcTemplateSave(@Autowired JdbcTemplate jdbcTemplate){
      String sql = "insert into tbl_book values(3,'springboot1','springboot2','springboot3')";
      jdbcTemplate.update(sql);
  }
  ```

#### **数据库技术**

springboot提供了3款内置的数据库

- H2
- HSQL
- Derby

以H2数据库为例讲解如何使用这些内嵌数据库

- 导入H2数据库对应的坐标，一共2个

  ```xml
  <dependency>
      <groupId>com.h2database</groupId>
      <artifactId>h2</artifactId>
  </dependency>
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-data-jpa</artifactId>
  </dependency>
  ```

- 将工程设置为web工程，启动工程时启动H2数据库

  ```xml
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-web</artifactId>
  </dependency>
  ```

- 通过配置开启H2数据库控制台访问程序，也可以使用其他的数据库连接软件操作

  ```yaml
  spring:
    h2:
      console:
        enabled: true
        path: /h2
  ```

- web端访问路径/h2，访问密码123456，如果访问失败，先配置下列数据源，启动程序运行后再次访问/h2路径就可以正常访问了

  ```yaml
  datasource:
    url: jdbc:h2:~/test
    hikari:
      driver-class-name: org.h2.Driver
      username: sa
      password: 123456
  ```

- 使用JdbcTemplate或MyBatisPlus技术操作数据库

- 线上运行一定记得关闭

### NoSQL

市面上常见的NoSQL解决方案

- Redis
- Mongo
- ES

#### **Redis**

Redis是一款采用key-value数据存储格式的内存级NoSQL数据库

- 支持多种数据存储格式
- 支持持久化
- 支持集群

应用场景：

- 缓存
- 任务队列
- 消息队列
- 分布式锁

Redis在windows环境使用

- 启动服务器

  ```SHELL
  redis-server.exe redis.windows.conf
  ```

- 启动客户端

  ```shell
  redis-cli.exe
  ```

- 基本操作

  ```
  set name itheima
  set age 12
  ---------------------
  set name itheima
  set age 12
  ```

  如果没有对应数据就会得到(nil)

- hash是一种一个名称下可以存储多个数据的存储模型，每个数据也可以有自己的二级存储名称

  ```
  hset a a1 aa1		#对外key名称是a，在名称为a的存储模型中，a1这个key中保存了数据aa1
  hset a a2 aa2
  ---------------
  hget a a1			#得到aa1
  hget a a2			#得到aa2
  ```

**SpringBoot整合Redis**

- 导入springboot整合redis的starter坐标(可以选择创建Boot项目时勾选Redis)

  ```xml
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-data-redis</artifactId>
  </dependency>
  ```

- 进行基础配置

  ```yaml
  spring:
    redis:
      host: localhost
      port: 6379
  ```

- 使用springboot整合redis的专用客户端接口操作，此处使用的是RedisTemplate

  ```java
  @SpringBootTest
  class Springboot16RedisApplicationTests {
      @Autowired
      private RedisTemplate redisTemplate;
      @Test
      void set() {
          ValueOperations ops = redisTemplate.opsForValue();
          ops.set("age",41);
      }
      @Test
      void get() {
          ValueOperations ops = redisTemplate.opsForValue();
          Object age = ops.get("name");
          System.out.println(age);
      }
      @Test
      void hset() {
          HashOperations ops = redisTemplate.opsForHash();
          ops.put("info","b","bb");
      }
      @Test
      void hget() {
          HashOperations ops = redisTemplate.opsForHash();
          Object val = ops.get("info", "b");
          System.out.println(val);
      }
  }
  ```

在操作redis时，需要先确认操作何种数据，根据数据种类得到操作接口。例如使用opsForValue()获取key value类型的数据操作接口，使用opsForHash()获取hash类型的数据操作接口

**StringRedisTemplate**

由于redis内部不提供java对象的存储格式，因此当操作的数据以对象的形式存在时，会进行转码，转换成字符串格式后进行操作。为了方便开发者使用基于字符串为数据的操作，springboot整合redis时提供了专用的API接口StringRedisTemplate.

```java
@SpringBootTest
public class StringRedisTemplateTest {
    @Autowired
    private StringRedisTemplate stringRedisTemplate;
    @Test
    void get(){
        ValueOperations<String, String> ops = stringRedisTemplate.opsForValue();
        String name = ops.get("name");
        System.out.println(name);
    }
}
```

**redis客户端选择**

springboot整合redis技术提供了多种客户端兼容模式，默认提供的是lettucs客户端技术，也可以根据需要切换成指定客户端技术，例如jedis客户端技术，切换成jedis客户端技术操作步骤

- 导入jedis坐标

  ```xml
  <dependency>
      <groupId>redis.clients</groupId>
      <artifactId>jedis</artifactId>
      <!--jedis坐标受springboot管理，无需提供版本号-->
  </dependency>
  ```

- 配置客户端技术类型，设置为jedis

  ```yaml
  spring:
    redis:
      host: localhost
      port: 6379
      client-type: jedis
  ```

- 根据需要设置对应的配置

  ```yaml
  spring:
    redis:
      host: localhost
      port: 6379
      client-type: jedis
      lettuce:
        pool:
          max-active: 16
      jedis:
        pool:
          max-active: 16
  ```

**lettcus与jedis区别**

- jedis连接Redis服务器是直连模式，当多线程模式下使用jedis会存在线程安全问题，解决方案可以通过配置连接池使每个连接专用，这样整体性能就大受影响
- lettcus基于Netty框架进行与Redis服务器连接，底层设计中采用StatefulRedisConnection.StatefulRedisConnection自身是线程安全的，可以保障并发访问安全问题，所以一个连接可以被多线程复用。当然lettcus也支持多连接实例一起工作

#### Mongodb

MongoDB是一个开源、高性能、无模式的文档型数据库，它是NoSQL数据库产品中的一种，是最像关系型数据库的非关系型数据库。

无模式:简单说就是作为一款数据库，没有固定的数据存储结构，第一条数据可能有A、B、C一共3个字段，第二条数据可能有D、E、F也是3个字段，第三条数据可能是A、C、E3个字段，也就是说数据的结构不固定，这就是无模式。

可以使用MongoDB作为数据存储的场景，但是并不是必须使用MongoDB的场景

- 淘宝用户数据
  - 存储位置：数据库
  - 特征：永久性存储，修改频度极低
- 游戏装备数据、游戏道具数据
  - 存储位置：数据库、Mongodb
  - 特征：永久性存储与临时存储相结合、修改频度较高
- 直播数据、打赏数据、粉丝数据
  - 存储位置：数据库、Mongodb
  - 特征：永久性存储与临时存储相结合，修改频度极高
- 物联网数据
  - 存储位置：Mongodb
  - 特征：临时存储，修改频度飞速

快速使用

- 启动服务器

  ```shell
  mongod --dbpath=..\data\db
  ```

- 启动客户端

  ```
  mongo --host=127.0.0.1 --port=27017
  ```

- 基本操作

  MongoDB虽然是一款数据库，但是它的操作并不是使用SQL语句进行的，可以用Navicatc操作

- 创建数据库：在左侧菜单中使用右键创建，输入数据库名称即可

- 创建集合：在Collections上使用右键创建，输入集合名称即可，集合等同于数据库中的表的作用

- 新增文档：（文档是一种类似json格式的数据，初学者可以先把数据理解为就是json数据）

  ```mysql
  db.集合名称.insert/save/insertOne(文档)
  ```

- 删除文档：

  ```mysql
  db.集合名称.remove(条件)
  ```

- 修改文档：

  ```mysql
  db.集合名称.update({条件}，{操作种类:{文档}})
  db.book.update({name:"springboot"},{$set:{name:"springboot2"}})
  //有多个数据名字相同时，修改遇到的第一条数据
  db.book.update({name:"springboot"},{$set:{name:"springboot2"}})
  //全部修改
  ```

- 查询文档：

  ```mysql
  基础查询
  查询全部：		   db.集合.find();
  查第一条：		   db.集合.findOne()
  查询指定数量文档：	db.集合.find().limit(10)					//查10条文档
  跳过指定数量文档：	db.集合.find().skip(20)					//跳过20条文档
  统计：			  	db.集合.count()
  排序：				db.集合.sort({age:1})						//按age升序排序
  投影：				db.集合名称.find(条件,{name:1,age:1})		 //仅保留name与age域
  
  条件查询
  基本格式：			db.集合.find({条件})
  模糊查询：			db.集合.find({域名:/正则表达式/})		  //等同SQL中的like，比like强大，可以执行正则所有规则
  条件比较运算：		   db.集合.find({域名:{$gt:值}})				//等同SQL中的数值比较操作，例如：name>18
  包含查询：			db.集合.find({域名:{$in:[值1，值2]}})		//等同于SQL中的in
  条件连接查询：		   db.集合.find({$and:[{条件1},{条件2}]})	   //等同于SQL中的and、or
  ```


**Spring整合Mongodb**

- 导入springboot整合MongoDB的starter坐标(直接选择相应模块)

  ```xml
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-data-mongodb</artifactId>
  </dependency>
  ```

- 进行基础配置

  ```yaml
  spring:
    data:
      mongodb:
        uri: mongodb://localhost/itheima
  ```

- 使用springboot整合MongoDB的专用客户端接口MongoTemplate来进行操作

  ```java
  @SpringBootTest
  class Springboot17MongodbApplicationTests {
      @Autowired
      private MongoTemplate mongoTemplate;
      @Test
      void contextLoads() {
          Book book = new Book();
          book.setId(2);
          book.setName("springboot2");
          book.setType("springboot2");
          book.setDescription("springboot2");
          mongoTemplate.save(book);
      }
      @Test
      void find(){
          List<Book> all = mongoTemplate.findAll(Book.class);
          System.out.println(all);
      }
  }
  ```

#### ES

ES（Elasticsearch）是一个分布式全文搜索引擎，重点是全文搜索。

要实现全文搜索的效果，不可能使用数据库中like操作去进行比对，这种效率太低了。ES设计了一种全新的思想，来实现全文搜索。具体操作过程如下：

1. 将被查询的字段的数据全部文本信息进行查分，分成若干个词

   例如“中华人民共和国”就会被拆分成三个词，分别是“中华”、“人民”、“共和国”，此过程有专业术语叫做分词。分词的策略不同，分出的效果不一样，不同的分词策略称为分词器。

2. 将分词得到的结果存储起来，对应每条数据的id

  - 例如id为1的数据中名称这一项的值是“中华人民共和国”，那么分词结束后，就会出现“中华”对应id为1，“人民”对应id为1，“共和国”对应id为1

  - 例如id为2的数据中名称这一项的值是“人民代表大会“，那么分词结束后，就会出现“人民”对应id为2，“代表”对应id为2，“大会”对应id为2

  - 此时就会出现如下对应结果，按照上述形式可以对所有文档进行分词。需要注意分词的过程不是仅对一个字段进行，而是对每一个参与查询的字段都执行，最终结果汇总到一个表格中

    | 分词结果关键字 | 对应id |
         | -------------- | ------ |
    | 中华           | 1      |
    | 人民           | 1,2    |
    | 共和国         | 1      |
    | 代表           | 2      |
    | 大会           | 2      |

3. 当进行查询时，如果输入“人民”作为查询条件，可以通过上述表格数据进行比对，得到id值1,2，然后根据id值就可以得到查询的结果数据了。

全文搜索中的分词结果关键字查询后得到的并不是整条的数据，而是数据的id，要想获得具体数据还要再次查询，因此这里为这种分词结果关键字起了一个全新的名称，叫做**倒排索引**。

**启动服务器**

```
双击elasticsearch.bat，访问http://localhost:9200
```

基本操作

ES中保存有我们要查询的数据，只不过格式和数据库存储数据格式不同而已。在ES中我们要先创建倒排==索引==，这个索引的功能有点类似于数据库的表，然后将数据添加到倒排索引中，添加的数据称为文档。所以要进行ES的操作要先创建索引，再添加文档，这样才能进行后续的查询操作。

要操作ES可以通过Rest风格的请求来进行，也就是说发送一个请求就可以执行一个操作。比如新建索引，删除索引这些操作都可以使用发送请求的形式来进行。

- 创建索引，books是索引名称，下同

  ```cmd
  PUT请求		http://localhost:9200/books
  ```

  发送请求后，看到如下信息即索引创建成功

  ```json
  {
      "acknowledged": true,
      "shards_acknowledged": true,
      "index": "books"
  }
  ```

  重复创建已经存在的索引会出现错误信息

- 查询索引

  ```cmd
  GET请求		http://localhost:9200/books
  ```

- 删除索引

  ```
  DELETE请求	http://localhost:9200/books
  ```

- 创建索引并指定分词器

  前面创建的索引是未指定分词器的，可以在创建索引时添加请求参数，设置分词器，目前国内较为流行的分词器是IK分词器

- 使用IK分词器创建索引格式

  ```json
  PUT请求		http://localhost:9200/books
  
  请求参数如下（注意是json格式的参数）
  {
      "mappings":{                 #定义mappings属性，替换创建索引时对应的mappings属性
          "properties":{			 #定义索引中包含的属性设置
              "id":{           		#设置索引中包含id属性
                  "type":"keyword"	#当前属性可以被直接搜索
              },
              "name":{				#设置索引中包含name属性
                  "type":"text",	 	#当前属性是文本信息，参与分词 
                  "analyzer":"ik_max_word",	 #使用IK分词器进行分词
                  "copy_to":"all"				#分词结果拷贝到all属性中
              },
              "type":{
                  "type":"keyword"
              },
              "description":{
                  "type":"text",
                  "analyzer":"ik_max_word",
                  "copy_to":"all"
              },
              "all":{ #定义属性，用来描述多个字段的分词结果集合，当前属性可以参与查询
                  "type":"text",
                  "analyzer":"ik_max_word"
              }
          }
      }
  }
  ```

ES中称数据为文档，下面进行文档操作

- 添加文档，有三种方式

  ```json
  POST请求	http://localhost:9200/books/_doc		#使用系统生成id
  POST请求	http://localhost:9200/books/_create/1	#使用指定id
  POST请求	http://localhost:9200/books/_doc/1		#使用指定id，不存在创建，存在更新（版本递增）
  
  文档通过请求参数传递，数据格式json
  {
      "name":"springboot",
      "type":"springboot",
      "description":"springboot"
  }  
  ```

- 查询文档

  ```json
  GET请求	http://localhost:9200/books/_doc/1		 #查询单个文档 		
  GET请求	http://localhost:9200/books/_search		 #查询全部文档
  ```

- 条件查询

  ```json
  GET请求	http://localhost:9200/books/_search?q=name:springboot	# q=查询属性名:查询属性值
  ```

- 删除文档

  ```json
  DELETE请求	http://localhost:9200/books/_doc/1
  ```

- 修改文档(直接覆盖，没有给的数据直接移除)

  ```json
  PUT请求	http://localhost:9200/books/_doc/1
  
  文档通过请求参数传递，数据格式json
  {
      "name":"springboot",
      "type":"springboot",
      "description":"springboot"
  }
  ```

- 修改文档（部分）

  ```json
  POST请求	http://localhost:9200/books/_update/1
  
  文档通过请求参数传递，数据格式json
  {			
      "doc":{                      #部分更新并不是对原始文档进行更新，而是对原始文档对象                                  中的doc属性中的指定属性更新
          "name":"springboot"		#仅更新提供的属性值，未提供的属性值不参与更新操作
      }
  }
  ```

springboot整合ES

- 导入springboot整合ES的starter坐标

  ```xml
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-data-elasticsearch</artifactId>
  </dependency>
  ```

- 进行基础配置

  ```yaml
  spring:
    elasticsearch:
      rest:
        uris: http://localhost:9200
  ```

- 使用springboot整合ES的专用客户端接口ElasticsearchRestTemplate来进行操作

  ```java
  @SpringBootTest
  class Springboot18EsApplicationTests {
      @Autowired
      private ElasticsearchRestTemplate template;
  }
  ```

上述操作形式是ES早期的操作方式，使用的客户端被称为Low Level Client，这种客户端操作方式性能方面略显不足，于是ES开发了全新的客户端操作方式，称为High Level Client。高级别客户端与ES版本同步更新，但是springboot最初整合ES的时候使用的是低级别客户端，所以企业开发需要更换成高级别的客户端模式。

**高级别客户端方式进行springboot整合ES**

- 导入springboot整合ES高级别客户端的坐标，此种形式目前没有对应的starter

  ```xml
  <dependency>
      <groupId>org.elasticsearch.client</groupId>
      <artifactId>elasticsearch-rest-high-level-client</artifactId>
  </dependency>
  ```

- 使用编程的形式设置连接的ES服务器，并获取客户端对象

  ```java
  @SpringBootTest
  class Springboot18EsApplicationTests {
      private RestHighLevelClient client;
        @Test
        void testCreateClient() throws IOException {
            HttpHost host = HttpHost.create("http://localhost:9200");
            RestClientBuilder builder = RestClient.builder(host);
            client = new RestHighLevelClient(builder);
    
            client.close();//手动关闭
        }
  }
  ```

- 使用客户端对象操作ES，例如创建索引

  ```java
  @SpringBootTest
  class Springboot18EsApplicationTests {
      private RestHighLevelClient client;
        @Test
        void testCreateIndex() throws IOException {
            HttpHost host = HttpHost.create("http://localhost:9200");
            RestClientBuilder builder = RestClient.builder(host);
            client = new RestHighLevelClient(builder);
            
            CreateIndexRequest request = new CreateIndexRequest("books");
            client.indices().create(request, RequestOptions.DEFAULT); 
            
            client.close();
        }
  }
  ```

- 无论进行ES何种操作，第一步永远是获取RestHighLevelClient对象，最后一步永远是关闭该对象的连接，因此可以改为

  ```java
  @SpringBootTest
  class Springboot18EsApplicationTests {
      @BeforeEach		//在测试类中每个操作运行前运行的方法
      void setUp() {
          HttpHost host = HttpHost.create("http://localhost:9200");
          RestClientBuilder builder = RestClient.builder(host);
          client = new RestHighLevelClient(builder);
      }
  
      @AfterEach		//在测试类中每个操作运行后运行的方法
      void tearDown() throws IOException {
          client.close();
      }
  
      private RestHighLevelClient client;
  
      @Test
      void testCreateIndex() throws IOException {
          CreateIndexRequest request = new CreateIndexRequest("books");
          client.indices().create(request, RequestOptions.DEFAULT);
      }
  }
  ```

- 创建索引（IK分词器）(这里也会执行上一步的测试前后方法)

  ```java
  @Test
  void testCreateIndexByIK() throws IOException {
      CreateIndexRequest request = new CreateIndexRequest("books");
      String json = "{\n" +
              "    \"mappings\":{\n" +
              "        \"properties\":{\n" +
              "            \"id\":{\n" +
              "                \"type\":\"keyword\"\n" +
              "            },\n" +
              "            \"name\":{\n" +
              "                \"type\":\"text\",\n" +
              "                \"analyzer\":\"ik_max_word\",\n" +
              "                \"copy_to\":\"all\"\n" +
              "            },\n" +
              "            \"type\":{\n" +
              "                \"type\":\"keyword\"\n" +
              "            },\n" +
              "            \"description\":{\n" +
              "                \"type\":\"text\",\n" +
              "                \"analyzer\":\"ik_max_word\",\n" +
              "                \"copy_to\":\"all\"\n" +
              "            },\n" +
              "            \"all\":{\n" +
              "                \"type\":\"text\",\n" +
              "                \"analyzer\":\"ik_max_word\"\n" +
              "            }\n" +
              "        }\n" +
              "    }\n" +
              "}";
      //设置请求中的参数
      request.source(json, XContentType.JSON);
      client.indices().create(request, RequestOptions.DEFAULT);
  }
  ```

- 添加文档

  ```java
  @Test
  //添加文档
  void testCreateDoc() throws IOException {
      Book book = bookDao.selectById(1);
      IndexRequest request = new IndexRequest("books").id(book.getId().toString());
      String json = JSON.toJSONString(book);
      request.source(json,XContentType.JSON);
      client.index(request,RequestOptions.DEFAULT);
  }
  ```

- **批量添加文档**

  ```java
  @Test
  //批量添加文档
  void testCreateDocAll() throws IOException {
      List<Book> bookList = bookDao.selectList(null);
      BulkRequest bulk = new BulkRequest();
      for (Book book : bookList) {
          IndexRequest request = new IndexRequest("books").id(book.getId().toString());
          String json = JSON.toJSONString(book);
          request.source(json,XContentType.JSON);
          bulk.add(request);
      }
      client.bulk(bulk,RequestOptions.DEFAULT);
  }
  ```

- **按id查询文档**:根据id查询文档使用的请求对象是GetRequest。

  ```java
  @Test
  //按id查询
  void testGet() throws IOException {
      GetRequest request = new GetRequest("books","1");
      GetResponse response = client.get(request, RequestOptions.DEFAULT);
      String json = response.getSourceAsString();
      System.out.println(json);
  }
  ```

- **按条件查询文档**

  ```java
  @Test
  //按条件查询
  void testSearch() throws IOException {
      SearchRequest request = new SearchRequest("books");
  
      SearchSourceBuilder builder = new SearchSourceBuilder();
      builder.query(QueryBuilders.termQuery("all","spring"));
      request.source(builder);
  
      SearchResponse response = client.search(request, RequestOptions.DEFAULT);
      SearchHits hits = response.getHits();
      for (SearchHit hit : hits) {
          String source = hit.getSourceAsString();
          //System.out.println(source);
          Book book = JSON.parseObject(source, Book.class);
          System.out.println(book);
      }
  }
  ```

  按条件查询文档使用的请求对象是SearchRequest，查询时调用SearchRequest对象的termQuery方法，需要给出查询属性名，此处支持使用合并字段，也就是前面定义索引属性时添加的all属性。

## 整合第三方技术

### Spring Cache

Spring Cache是一个框架，实现了基于注解的缓存功，只需要一个简单的注解，就能实现缓存功能。Spring Cache提供了一层抽象，底层可以切换不同的cache实现。**具体就是通过CacheManager**接口来统一不同的缓存技术。

CacheManager是Spring提供的各种缓存技术抽象接口。

针对不同的缓存技术需要实现不同的CacheManager：

- EhCacheManager：使用EhCache作为缓存技术
- GuavaCacheManager：使用Google的GuavaCache作为缓存技术
- RedisCacheManager：使用Redis作为缓存技术

企业级应用主要作用是信息处理，当需要读取数据时，由于受限于数据库的访问效率，导致整体系统性能偏低。为了改善上述现象，开发者通常会在应用程序与数据库之间建立一种临时的数据存储机制，该区域中的数据在内存中保存，读写速度较快，可以有效解决数据库访问效率低下的问题。这一块临时存储数据的区域就是缓存。

缓存是一种介于数据永久存储介质与应用程序之间的数据临时存储介质，使用缓存可以有效的减少低速数据读取过程的次数（例如磁盘IO），提高系统性能。此外缓存不仅可以用于提高永久性存储介质的数据读取效率，还可以提供临时的数据存储空间。

SpringBoot缓存技术

- 导入springboot提供的缓存技术对应的starter

  ```xml
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-cache</artifactId>
  </dependency>
  ```

- 启用缓存，在启动类上方标注注解@EnableCaching配置springboot程序中可以使用缓存

  ```java
  @SpringBootApplication
  //开启缓存功能
  @EnableCaching
  public class Springboot19CacheApplication {
      public static void main(String[] args) {
          SpringApplication.run(Springboot19CacheApplication.class, args);
      }
  }
  ```

- 设置操作的数据是否使用缓存

  ```java
  @Service
  public class BookServiceImpl implements BookService {
      @Autowired
      private BookDao bookDao;
  
      @Cacheable(value="cacheSpace",key="#id")
      public Book getById(Integer id) {
          return bookDao.selectById(id);
      }
  }
  ```

​    在业务方法上面使用注解@Cacheable声明当前方法的返回值放入缓存中，其中要指定缓存的存储位置，以及缓存中保存当前方法返回值对应的名称。上例中value属性描述缓存的存储位置，可以理解为是一个存储空间名，key属性描述了缓存中保存数据的名称，使用#id读取形参中的id值作为缓存名称.使用@Cacheable注解后，执行当前操作，如果发现对应名称在缓存中没有数据，就正常读取数据，然后放入缓存；如果对应名称在缓存中有数据，就终止当前业务方法执行，直接返回缓存中的数据。

**手机验证码案例**

手机验证码案例需求如下：

- 输入手机号获取验证码，组织文档以短信形式发送给用户（页面模拟）
- 输入手机号和验证码验证结果

步骤

- 导入springboot提供的缓存技术对应的starter

  ```xml
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-cache</artifactId>
  </dependency>
  ```

- 启用缓存，在引导类上方标注注解@EnableCaching配置springboot程序中可以使用缓存

  ```java
  @SpringBootApplication
  //开启缓存功能
  @EnableCaching
  public class Springboot19CacheApplication {
      public static void main(String[] args) {
          SpringApplication.run(Springboot19CacheApplication.class, args);
      }
  }
  ```

- 定义验证码对应的实体类，封装手机号与验证码两个属性

  ```java
  @Data
  public class SMSCode {
      private String tele;
      private String code;
  }
  ```

- 定义验证码功能的业务层接口与实现类

  ```java
  public interface SMSCodeService {
      public String sendCodeToSMS(String tele);
      public boolean checkCode(SMSCode smsCode);
  }
  
  @Service
  public class SMSCodeServiceImpl implements SMSCodeService {
      @Autowired
      private CodeUtils codeUtils;
  
      @CachePut(value = "smsCode", key = "#tele")
      public String sendCodeToSMS(String tele) {
          String code = codeUtils.generator(tele);
          return code;
      }
  
      public boolean checkCode(SMSCode smsCode) {
          //取出内存中的验证码与传递过来的验证码比对，如果相同，返回true
          String code = smsCode.getCode();
          String cacheCode = codeUtils.get(smsCode.getTele());
          return code.equals(cacheCode);
      }
  }
  ```

  获取验证码后，当验证码失效时必须重新获取验证码，因此在获取验证码的功能上不能使用@Cacheable注解，@Cacheable注解是缓存中没有值则放入值，缓存中有值则取值。此处的功能仅仅是生成验证码并放入缓存，并不具有从缓存中取值的功能，因此不能使用@Cacheable注解，应该使用仅具有向缓存中保存数据的功能，使用@CachePut注解即可。

- 定义验证码的生成策略与根据手机号读取验证码的功能

  ```java
  @Component
  public class CodeUtils {
      private String [] patch = {"000000","00000","0000","000","00","0",""};
  
      public String generator(String tele){
          int hash = tele.hashCode();
          int encryption = 20206666;
          long result = hash ^ encryption;
          long nowTime = System.currentTimeMillis();
          result = result ^ nowTime;
          long code = result % 1000000;
          code = code < 0 ? -code : code;
          String codeStr = code + "";
          int len = codeStr.length();
          return patch[len] + codeStr;
      }
      //采用bean的形式进行获取
      @Cacheable(value = "smsCode",key="#tele")
      public String get(String tele){
          return null;
      }
  }
  ```

- 定义验证码功能的web层接口，一个方法用于提供手机号获取验证码，一个方法用于提供手机号和验证码进行校验

  ```java
  @RestController
  @RequestMapping("/sms")
  public class SMSCodeController {
      @Autowired
      private SMSCodeService smsCodeService;
      
      @GetMapping
      public String getCode(String tele){
          String code = smsCodeService.sendCodeToSMS(tele);
          return code;
      }
      
      @PostMapping
      public boolean checkCode(SMSCode smsCode){
          return smsCodeService.checkCode(smsCode);
      }
  }
  ```

SpringBoot提供的缓存技术除了默认缓存方案（Simple）还可以对其他缓存技术进行整合，统一接口，方便缓存技术的开发与管理。

- Generic
- JCache
- ==Ehcache==
- Hazelcast
- Infinispan
- Couchbase
- ==Redis==
- Caffenine
- Simple
- ==memcached==

#### Spring Cache常用注解

- @EnableCaching：在启动类上使用，开启缓存注解功能

- @Cacheable：在方法执行前spring先查看缓存中是否有数据，如果有数据，则直接返回缓存数据；若没有数据，调用方法并将方法返回值放到缓存中。

  ```java
  @Cacheable(value = "userCache",key = "#id",unless = "result == null")
  //condition：当满足条件时进行缓存，condition无法调用返回结果
  //unless：满足条件则不缓存，可以调用返回结果
  public User getById(Long id){
      return userService.getById(id);
  }
  ```

- @CachePut：将方法的返回值放到缓存中

  ```java
  @CachePut(value = "smsCode", key = "#tele")
  //“smsCode”缓存名称，每个缓存名称下可以有多个key
  public String sendCodeToSMS(String tele) {
      String code = codeUtils.generator(tele);
      return code;
  }
  @CachePut(value = "userCache",key = "#result.id") //#result.id：获取返回值的属性
  public User save(User user){
      return user;
  }
  ```

- @CacheEvict：将一条或多条数据从缓存中删除

  ```java
  @CacheEvict(value = "userCache",key = "#id")
  public void delete(Long id){
  }
  @CacheEvict(value = "userCache",key = "#p0")//p0：获取第一个参数 p1：获取第二个参数
  public void delete(Long id){
  }
  @CacheEvict(value = "setmealCache",allEntries = true)//删除该value类型的所有缓存
  ```

#### **SpringBoot整合Ehcache缓存**

- 导入Ehcache的坐标

  ```xml
  <dependency>
      <groupId>net.sf.ehcache</groupId>
      <artifactId>ehcache</artifactId>
  </dependency>
  ```

- 配置缓存技术实现使用Ehcache

  ```yaml
  spring:
    cache:
      type: ehcache
      ehcache:
        config: ehcache.xml
  ```

  由于ehcache的配置有独立的配置文件格式，因此还需要指定ehcache的配置文件，以便于读取相应配置

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <ehcache xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:noNamespaceSchemaLocation="http://ehcache.org/ehcache.xsd"
           updateCheck="false">
      <diskStore path="D:\ehcache" />
  
      <!--默认缓存策略 -->
      <!-- external：是否永久存在，设置为true则不会被清除，此时与timeout冲突，通常设置为false-->
      <!-- diskPersistent：是否启用磁盘持久化-->
      <!-- maxElementsInMemory：最大缓存数量-->
      <!-- overflowToDisk：超过最大缓存数量是否持久化到磁盘-->
      <!-- timeToIdleSeconds：最大不活动间隔，设置过长缓存容易溢出，设置过短无效果，可用于记录时效性数据，例如验证码-->
      <!-- timeToLiveSeconds：最大存活时间s-->
      <!-- memoryStoreEvictionPolicy：缓存清除策略-->
      LRU：挑选距离上次使用最长的数据淘汰
      LFU：挑选最近使用次数最少的数据淘汰
      <defaultCache
          eternal="false"
          diskPersistent="false"
          maxElementsInMemory="1000"
          overflowToDisk="false"
          timeToIdleSeconds="60"
          timeToLiveSeconds="60"
          memoryStoreEvictionPolicy="LRU" />
  
      <cache
          name="smsCode"
          eternal="false"
          diskPersistent="false"
          maxElementsInMemory="1000"
          overflowToDisk="false"
          timeToIdleSeconds="10"
          timeToLiveSeconds="10"
          memoryStoreEvictionPolicy="LRU" />
  </ehcache>
  ```

  这个设定需要保障ehcache中有一个缓存空间名称叫做smsCode的配置，前后要统一。在企业开发过程中，通过设置不同名称的cache来设定不同的缓存策略，应用于不同的缓存数据。


#### **SpringBoot整合Redis缓存**

- 导入redis和cache的坐标

  ```xml
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-data-redis</artifactId>
  </dependency>
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-cache</artifactId>
  </dependency>
  ```

- 配置缓存技术实现使用redis

  ```yaml
  spring:
    #Redis配置
    redis:
      host: 107.182.19.176
      port: 6379
      password: 123456
      database: 0 #Redis默认提供16个数据库，默认使用0号数据库 	
    cache:
      redis:
        time-to-time: 1800000 #设置缓存过期时间，单位毫秒
  ```

  如果需要对redis作为缓存进行配置，注意不是对原始的redis进行配置，而是配置redis作为缓存使用相关的配置，隶属于spring.cache.redis节点下，注意不要写错位置了。

  ```yaml
  spring:
    redis:
      host: localhost
      port: 6379
    cache:
      type: redis
      redis:
        use-key-prefix: false #是否使用前缀
        key-prefix: sms_ #指定前缀
        cache-null-values: false #是否缓存空值
        time-to-live: 10s #最大存活时间
  ```

**SpringBoot整合Memcached缓存**

管理员权限下在所在文件夹

```shell
memcached.exe -d install
```

```shell
memcached.exe -d start		# 启动服务
memcached.exe -d stop		# 停止服务
```

由于memcached未被springboot收录为缓存解决方案，因此使用memcached需要通过手工硬编码的方式来使用.

memcached目前提供有三种客户端技术，分别是Memcached Client for Java、SpyMemcached和Xmemcached，其中性能指标各方面最好的客户端是Xmemcached，本次整合就使用这个作为客户端实现技术了。

**Xmemcached**

- 导入xmemcached的坐标

  ```xml
  <dependency>
      <groupId>com.googlecode.xmemcached</groupId>
      <artifactId>xmemcached</artifactId>
      <version>2.4.7</version>
  </dependency>
  ```

- 配置memcached，制作memcached的配置类//memcached默认对外服务端口11211

  ```java
  @Configuration
  public class XMemcachedConfig {
      @Bean
      public MemcachedClient getMemcachedClient() throws IOException {
          MemcachedClientBuilder memcachedClientBuilder = new XMemcachedClientBuilder("localhost:11211");
          MemcachedClient memcachedClient = memcachedClientBuilder.build();
          return memcachedClient;
      }
  }
  ```

- 使用xmemcached客户端操作缓存，注入MemcachedClient对象

  ```java
  @Service
  public class SMSCodeServiceImpl implements SMSCodeService {
      @Autowired
      private CodeUtils codeUtils;
      @Autowired
      private MemcachedClient memcachedClient;
  
      public String sendCodeToSMS(String tele) {
          String code = codeUtils.generator(tele);
          try {
              memcachedClient.set(tele,10,code);//key,过期时间，value
          } catch (Exception e) {
              e.printStackTrace();
          }
          return code;
      }
  
      public boolean checkCode(SMSCode smsCode) {
          String code = null;//扩大code范围
          try {
              code = memcachedClient.get(smsCode.getTele()).toString();//从缓存获取
          } catch (Exception e) {
              e.printStackTrace();
          }
          return smsCode.getCode().equals(code);
      }
  }
  ```

上述代码中对于服务器的配置使用硬编码写死到了代码中，可以做成独立的配置属性。

**定义配置属性**

- 定义配置类，加载必要的配置属性，读取配置文件中memcached节点信息

  ```java
  @Component
  @ConfigurationProperties(prefix = "memcached")
  @Data
  public class XMemcachedProperties {
      private String servers;
      private int poolSize;
      private long opTimeout;
  }
  ```

- 定义memcached节点信息

  ```yaml
  memcached:
    servers: localhost:11211 
    poolSize: 10 #连接池数量
    opTimeout: 3000 #超时时间
  ```

- 在memcached配置类中加载信息

  ```java
  @Configuration
  public class XMemcachedConfig {
      @Autowired
      private XMemcachedProperties props;
      @Bean
      public MemcachedClient getMemcachedClient() throws IOException {
          MemcachedClientBuilder memcachedClientBuilder = new XMemcachedClientBuilder(props.getServers());
          memcachedClientBuilder.setConnectionPoolSize(props.getPoolSize());
          memcachedClientBuilder.setOpTimeout(props.getOpTimeout());
          MemcachedClient memcachedClient = memcachedClientBuilder.build();
          return memcachedClient;
      }
  }
  ```


**总结**

1. memcached安装后需要启动对应服务才可以对外提供缓存功能，安装memcached服务需要基于windows系统管理员权限
2. 由于springboot没有提供对memcached的缓存整合方案，需要采用手工编码的形式创建xmemcached客户端操作缓存
3. 导入xmemcached坐标后，创建memcached配置类，注册MemcachedClient对应的bean，用于操作缓存
4. 初始化MemcachedClient对象所需要使用的属性可以通过自定义配置属性类的形式加载

redis和mongodb需要安装独立的服务器，连接时需要输入对应的服务器地址，这种是远程缓存，Ehcache是一个典型的内存级缓存，因为它什么也不用安装，启动后导入jar包就有缓存功能了。

#### SpringBoot整合jetcache缓存

jetcache严格意义上来说，并不是一个缓存解决方案，只能说他算是一个缓存框架，然后把别的缓存放到jetcache中管理，这样就可以支持AB缓存一起用了

jetcache支持的缓存方案本地缓存支持两种，远程缓存支持两种，分别如下

- 本地缓存（Local）
  - LinkedHashMap
  - Caffeine
- 远程缓存（Remote）
  - Redis
  - Tair

**LinkedHashMap+Redis实现本地与远程缓存方案同时使用**

**jetcache远程缓存方案**

- 导入springboot整合jetcache对应的坐标starter，当前坐标默认使用的远程方案是redis

  ```xml
  <dependency>
      <groupId>com.alicp.jetcache</groupId>
      <artifactId>jetcache-starter-redis</artifactId>
      <version>2.6.2</version>
  </dependency>
  ```

- 远程方案基本配置

  ```yaml
  jetcache:
    remote:
      default:
        type: redis
        host: localhost
        port: 6379
        poolConfig: #poolConfig是必配项，否则会报错
          maxTotal: 50
  ```

- 启用缓存，在引导类上方标注注解@EnableCreateCacheAnnotation配置springboot程序中可以使用注解的形式创建缓存

  ```java
  @SpringBootApplication
  //jetcache启用缓存的主开关
  @EnableCreateCacheAnnotation
  public class Springboot20JetCacheApplication {
      public static void main(String[] args) {
          SpringApplication.run(Springboot20JetCacheApplication.class, args);
      }
  }
  ```

- 创建缓存对象Cache，并使用注解@CreateCache标记当前缓存的信息，然后使用Cache对象的API操作缓存，put写缓存，get读缓存。

  ```java
  @Service
  public class SMSCodeServiceImpl implements SMSCodeService {
      @Autowired
      private CodeUtils codeUtils;
      
      //name:指定前缀 exipire:过期时间 timeUnit：时间单位默认秒
      @CreateCache(name="jetCache_",expire = 10,timeUnit = TimeUnit.SECONDS)
      private Cache<String ,String> jetCache;//缓存空间
  
      public String sendCodeToSMS(String tele) {
          String code = codeUtils.generator(tele);
          jetCache.put(tele,code);
          return code;
      }
  
      public boolean checkCode(SMSCode smsCode) {
          String code = jetCache.get(smsCode.getTele());
          return smsCode.getCode().equals(code);
      }
  }
  ```

- 上述方案中使用的是配置中定义的default缓存，其实这个default是个名字，可以随便写，也可以随便加。例如再添加一种缓存解决方案，参照如下配置进行：

  ```yaml
  jetcache:
    remote:
      default:
        type: redis
        host: localhost
        port: 6379
        poolConfig:
          maxTotal: 50
      sms:
        type: redis
        host: localhost
        port: 6379
        poolConfig:
          maxTotal: 50
  ```

  如果想使用名称是sms的缓存，需要再创建缓存时指定参数area，声明使用对应缓存即可

  ```java
  @Service
  public class SMSCodeServiceImpl implements SMSCodeService {
      @Autowired
      private CodeUtils codeUtils;
      
      @CreateCache(area="sms",name="jetCache_",expire = 10,timeUnit = TimeUnit.SECONDS)
      private Cache<String ,String> jetCache;
  
      public String sendCodeToSMS(String tele) {
          String code = codeUtils.generator(tele);
          jetCache.put(tele,code);
          return code;
      }
  
      public boolean checkCode(SMSCode smsCode) {
          String code = jetCache.get(smsCode.getTele());
          return smsCode.getCode().equals(code);
      }
  }
  ```

**纯本地方案**

- 导入springboot整合jetcache对应的坐标starter

  ```xml
  <dependency>
      <groupId>com.alicp.jetcache</groupId>
      <artifactId>jetcache-starter-redis</artifactId>
      <version>2.6.2</version>
  </dependency>
  ```

- 本地缓存基本配置

  ```yaml
  jetcache:
    local:
      default:
        type: linkedhashmap
        keyConvertor: fastjson
  ```

  为了加速数据获取时key的匹配速度，jetcache要求指定key的类型转换器。简单说就是，如果你给了一个Object作为key的话，我先用key的类型转换器给转换成字符串，然后再保存。等到获取数据时，仍然是先使用给定的Object转换成字符串，然后根据字符串匹配。由于jetcache是阿里的技术，这里推荐key的类型转换器使用阿里的fastjson。

- 启用缓存

  ```java
  @SpringBootApplication
  //jetcache启用缓存的主开关
  @EnableCreateCacheAnnotation
  public class Springboot20JetCacheApplication {
      public static void main(String[] args) {
          SpringApplication.run(Springboot20JetCacheApplication.class, args);
      }
  }
  ```

- 创建缓存对象Cache时，标注当前使用本地缓存

  ```java
  @Service
  public class SMSCodeServiceImpl implements SMSCodeService {
      @CreateCache(name="jetCache_",expire = 1000,timeUnit = TimeUnit.SECONDS,cacheType = CacheType.LOCAL)
      private Cache<String ,String> jetCache;
  
      public String sendCodeToSMS(String tele) {
          String code = codeUtils.generator(tele);
          jetCache.put(tele,code);
          return code;
      }
  
      public boolean checkCode(SMSCode smsCode) {
          String code = jetCache.get(smsCode.getTele());
          return smsCode.getCode().equals(code);
      }
  }
  ```

  cacheType控制当前缓存使用本地缓存还是远程缓存，配置cacheType=CacheType.LOCAL即使用本地缓存。

**本地+远程方案**

将两种配置合并

```yaml
jetcache:
  areaInCacheName: false #default或者sms等名字是否被储存
  local:
    default:
      type: linkedhashmap
      keyConvertor: fastjson
      limit: 100 #缓存数据量
  remote:
    default:
      type: redis
      host: localhost
      port: 6379
      poolConfig:
        maxTotal: 50
    sms:
      type: redis
      host: localhost
      port: 6379
      poolConfig:
        maxTotal: 50
```

cacheType如果不进行配置，默认值是REMOTE，即仅使用远程缓存方案。关于jetcache的配置，参考以下信息

| 属性                                                      | 默认值 | 说明                                                         |
| --------------------------------------------------------- | ------ | ------------------------------------------------------------ |
| jetcache.statIntervalMinutes                              | 0      | 统计间隔，0表示不统计                                        |
| jetcache.hiddenPackages                                   | 无     | 自动生成name时，隐藏指定的包名前缀                           |
| jetcache.[local\|remote].${area}.type                     | 无     | 缓存类型，本地支持linkedhashmap、caffeine，远程支持redis、tair |
| jetcache.[local\|remote].${area}.keyConvertor             | 无     | key转换器，当前仅支持fastjson                                |
| jetcache.[local\|remote].${area}.valueEncoder             | java   | 仅remote类型的缓存需要指定，可选java和kryo                   |
| jetcache.[local\|remote].${area}.valueDecoder             | java   | 仅remote类型的缓存需要指定，可选java和kryo                   |
| jetcache.[local\|remote].${area}.limit                    | 100    | 仅local类型的缓存需要指定，缓存实例最大元素数                |
| jetcache.[local\|remote].${area}.expireAfterWriteInMillis | 无穷大 | 默认过期时间，毫秒单位                                       |
| jetcache.local.${area}.expireAfterAccessInMillis          | 0      | 仅local类型的缓存有效，毫秒单位，最大不活动间隔              |

以上方案仅支持手工控制缓存，但是springcache方案中的方法缓存特别好用，给一个方法添加一个注解，方法就会自动使用缓存。jetcache也提供了对应的功能，即方法缓存。

**方法缓存**

jetcache提供了方法缓存方案，在对应的操作接口上方使用注解@Cached即可

- 导入springboot整合jetcache对应的坐标starter

  ```xml
  <dependency>
      <groupId>com.alicp.jetcache</groupId>
      <artifactId>jetcache-starter-redis</artifactId>
      <version>2.6.2</version>
  </dependency>
  ```

- 配置缓存

  ```yaml
  jetcache:
    local:
      default:
        type: linkedhashmap
        keyConvertor: fastjson
    remote:
      default:
        type: redis
        host: localhost
        port: 6379
        keyConvertor: fastjson #key的类型转换方式
        valueEncode: java #值进入redis时是java类型	
        valueDecode: java #值从redis中读取时转换成java
        poolConfig:
          maxTotal: 50
      sms:
        type: redis
        host: localhost
        port: 6379
        poolConfig:
          maxTotal: 50
  ```

  由于redis缓存中不支持保存对象，因此需要对redis设置当Object类型数据进入到redis中时如何进行类型转换。

- 为了实现Object类型的值进出redis，需要保障Object类型的数据必须实现序列化接口。

  ```java
  @Data
  public class Book implements Serializable {
      private Integer id;
      private String type;
      private String name;
      private String description;
  }
  ```

- 启用缓存时开启方法缓存功能，并配置basePackages，说明在哪些包中开启方法缓存

  ```java
  @SpringBootApplication
  //jetcache启用缓存的主开关
  @EnableCreateCacheAnnotation
  //开启方法注解缓存
  @EnableMethodCache(basePackages = "com.itheima")
  public class Springboot20JetCacheApplication {
      public static void main(String[] args) {
          SpringApplication.run(Springboot20JetCacheApplication.class, args);
      }
  }
  ```

- 使用注解@Cached标注当前方法使用缓存

  ```java
  @Service
  public class BookServiceImpl implements BookService {
      @Autowired
      private BookDao bookDao;
      
      @Override
      @Cached(name="book_",key="#id",expire = 3600,cacheType = CacheType.REMOTE)
      public Book getById(Integer id) {
          return bookDao.selectById(id);
      }
  }
  ```


**远程方案的数据同步**

由于远程方案中redis保存的数据可以被多个客户端共享，这就存在了数据同步问题。jetcache提供了3个注解解决此问题，分别在更新、删除操作时同步缓存数据，和读取缓存时定时刷新数据

**更新缓存**

```java
@CacheUpdate(name="book_",key="#book.id",value="#book")
public boolean update(Book book) {
    return bookDao.updateById(book) > 0;
}
```

**删除缓存**

```java
@CacheInvalidate(name="book_",key = "#id")
public boolean delete(Integer id) {
    return bookDao.deleteById(id) > 0;
}
```

**定时刷新缓存**

```java
@Cached(name="book_",key="#id",expire = 3600,cacheType = CacheType.REMOTE)
@CacheRefresh(refresh = 5)//刷新时间
public Book getById(Integer id) {
    return bookDao.selectById(id);
}
```

数据报表

jetcache还提供有简单的数据报表功能，帮助开发者快速查看缓存命中信息，只需要添加一个配置即可

```yaml
jetcache:
  statIntervalMinutes: 1 #设置后，每1分钟在控制台输出缓存数据命中信息
```

### 任务

定时任务是企业级开发中必不可少的组成部分，诸如长周期业务数据的计算，例如年度报表，诸如系统脏数据的处理，再比如系统性能监控报告，还有抢购类活动的商品上架，这些都离不开定时任务。

#### Quartz

Quartz技术是一个比较成熟的定时任务框架，springboot对其进行整合后，简化了一系列的配置，将很多配置采用默认设置

- 工作（Job）：用于定义具体执行的工作
- 工作明细（JobDetail）：用于描述定时工作相关的信息
- 触发器（Trigger）：描述了工作明细与调度器的对应关系
- 调度器（Scheduler）：用于描述触发工作的执行规则，通常使用cron表达式定义规则

**springboot整合Quartz**

- 导入springboot整合Quartz的starter

  ```xml
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-quartz</artifactId>
  </dependency>
  ```

- 定义任务Bean，按照Quartz的开发规范制作，继承QuartzJobBean

  ```java
  public class MyQuartz extends QuartzJobBean {
      @Override
      protected void executeInternal(JobExecutionContext context) throws JobExecutionException {
          System.out.println("quartz task run...");
      }
  }
  ```

- 创建Quartz配置类，定义工作明细（JobDetail）与触发器的（Trigger）bean

  ```java
  @Configuration
  public class QuartzConfig {
      @Bean
      public JobDetail printJobDetail(){
          //绑定具体的工作
          //storeDurably:用来持久化
          return JobBuilder.newJob(MyQuartz.class).storeDurably().build();
      }
      @Bean
      public Trigger printJobTrigger(){
  //秒 分 小时 日 月 星期(?根据日期自动匹配星期 *任意时间 0/5从零秒开始每五秒执行一次)
  ScheduleBuilder schedBuilder = CronScheduleBuilder.cronSchedule("0/5 * * * * ?");
          //绑定对应的工作明细
          return TriggerBuilder.newTrigger().forJob(printJobDetail()).withSchedule(schedBuilder).build();
      }
  }
  ```

#### Task

- 开启定时任务功能，在引导类上开启定时任务功能的开关，使用注解@EnableScheduling

  ```java
  @SpringBootApplication
  //开启定时任务功能
  @EnableScheduling
  public class Springboot22TaskApplication {
      public static void main(String[] args) {
          SpringApplication.run(Springboot22TaskApplication.class, args);
      }
  }
  ```

- 定义Bean，在对应要定时执行的操作上方，使用注解@Scheduled定义执行的时间，执行时间的描述方式还是cron表达式

  ```java
  @Component
  public class MyBean {
      @Scheduled(cron = "0/1 * * * * ?")
      public void print(){
          System.out.println(Thread.currentThread().getName()+" :spring task run...");
      }
  }
  ```

- 想对定时任务进行相关配置，可以通过配置文件进行

  ```yaml
  spring:
    task:
     	scheduling:
        pool:
         	size: 1							# 任务调度线程池大小 默认 1
        thread-name-prefix: ssm_      	# 调度线程名称前缀 默认 scheduling-      
          shutdown:
            await-termination: false		# 线程池关闭时等待所有任务完成
            await-termination-period: 10s	# 调度线程关闭前最大等待时间，确保最后一定关闭
  ```

### 邮件

发邮件是java程序的基本操作，springboot整合javamail其实就是简化开发

- SMTP(Simple Mail Transfer Protocol)：简单邮件传输协议，用于**发送**电子邮件的传输协议
- POP3（Post Office Protocol - Version 3）：用于**接收**电子邮件的标准协议
- IMAP（Internet Mail Access Protocol）：互联网消息协议，是POP3的替代协议

SMPT是发邮件的标准，POP3是收邮件的标准，IMAP是对POP3的升级

**发送简单邮件**

- 导入springboot整合javamail的starter

  ```xml
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-mail</artifactId>
  </dependency>
  ```

- 配置邮箱的登录信息

  ```yaml
  spring:
    mail:
      host: smtp.126.com #可以换成别的邮箱
      username: xxxx@126.com
      password: #再邮箱设置中开启smtp会获得授权码
  ```

- 使用JavaMailSender接口发送邮件

  ```java
  @Service
  public class SendMailServiceImpl implements SendMailService {
      @Autowired
      private JavaMailSender javaMailSender;
  
      //发送人
      private String from = "test@qq.com";
      //接收人
      private String to = "test@126.com";
      //标题
      private String subject = "测试邮件";
      //正文
      private String context = "测试邮件正文内容";
  
      @Override
      public void sendMail() {
          SimpleMailMessage message = new SimpleMailMessage();
          message.setFrom(from+"(小甜甜)");//括号里相当于昵称
          message.setTo(to);
          message.setSubject(subject);
          message.setText(context);
          javaMailSender.send(message);
      }
  }
  ```

  将发送邮件的必要信息（发件人、收件人、标题、正文）封装到SimpleMailMessage对象中，可以根据规则设置发送人昵称等。

**发送多组件邮件**

**发送网页正文邮件**

```java
@Service
public class SendMailServiceImpl2 implements SendMailService {
    @Autowired
    private JavaMailSender javaMailSender;

    //发送人
    private String from = "test@qq.com";
    //接收人
    private String to = "test@126.com";
    //标题
    private String subject = "测试邮件";
    //正文
    private String context = "<img src='ABC.JPG'/><a href='https://www.itcast.cn'>点开有惊喜</a>";

    public void sendMail() {
        try {
            MimeMessage message = javaMailSender.createMimeMessage();
            MimeMessageHelper helper = new MimeMessageHelper(message);
            helper.setFrom(to+"(小甜甜)");
            helper.setTo(from);
            helper.setSubject(subject);
            helper.setText(context,true);		//配置true才可以解析html语言	

            javaMailSender.send(message);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

**发送带有附件的邮件**

```java
@Service
public class SendMailServiceImpl2 implements SendMailService {
    @Autowired
    private JavaMailSender javaMailSender;

    //发送人
    private String from = "test@qq.com";
    //接收人
    private String to = "test@126.com";
    //标题
    private String subject = "测试邮件";
    //正文
    private String context = "测试邮件正文";

    public void sendMail() {
        try {
            MimeMessage message = javaMailSender.createMimeMessage();
            //此处设置支持附件
            MimeMessageHelper helper = new MimeMessageHelper(message,true);
           
            helper.setFrom(to+"(小甜甜)");
            helper.setTo(from);
            helper.setSubject(subject);
            helper.setText(context);

            //添加附件
            File f1 = new File("springboot_23_mail-0.0.1-SNAPSHOT.jar");
            File f2 = new File("resources\\logo.png");

            helper.addAttachment(f1.getName(),f1);//文件名称和文件对象
            helper.addAttachment("最靠谱的培训结构.png",f2);

            javaMailSender.send(message);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### 消息

从广义角度来说，消息其实就是信息，但是和信息又有所不同。信息通常被定义为一组数据，而消息除了具有数据的特征之外，还有消息的来源与接收的概念。通常发送消息的一方称为消息的生产者，接收消息的一方称为消息的消费者。

对于消息的生产者与消费者的工作模式，还可以将消息划分成两种模式，同步消费与异步消息。

所谓同步消息就是生产者发送完消息，等待消费者处理，消费者处理完将结果告知生产者，然后生产者继续向下执行业务。这种模式过于卡生产者的业务执行连续性，在现在的企业级开发中，上述这种业务场景通常不会采用消息的形式进行处理。

所谓异步消息就是生产者发送完消息，无需等待消费者处理完毕，生产者继续向下执行其他动作。比如生产者发送了一个日志信息给日志系统，发送过去以后生产者就向下做其他事情了，无需关注日志系统的执行结果。日志系统根据接收到的日志信息继续进行业务执行，是单纯的记录日志，还是记录日志并报警，这些和生产者无关，这样生产者的业务执行效率就会大幅度提升。并且可以通过添加多个消费者来处理同一个生产者发送的消息来提高系统的高并发性，改善系统工作效率，提高用户体验。一旦某一个消费者由于各种问题宕机了，也不会对业务产生影响，提高了系统的高可用性。

**Java处理消息的标准规范**（异步消息）

- JMS
- AMQP
- MQTT

#### JMS

JMS（Java Message Service）,这是一个规范，作用等同于JDBC规范，提供了与消息服务相关的API接口。

JMS规范中规范了消息有两种模型。分别是**点对点模型**和**发布订阅模型**。

- **点对点模型**：peer-2-peer，生产者会将消息发送到一个保存消息的容器中，通常使用队列模型，使用队列保存消息。一个队列的消息只能被一个消费者消费，或未被及时消费导致超时。这种模型下，生产者和消费者是一对一绑定的。
- **发布订阅模型**：publish-subscribe，生产者将消息发送到一个保存消息的容器中，也是使用队列模型来保存。但是消息可以被多个消费者消费，生产者和消费者完全独立，相互不需要感知对方的存在。

**JMS消息种类**

- TextMessage
- MapMessage
- BytesMessage
- StreamMessage
- ObjectMessage
- Message （只有消息头和属性）

JMS的设计是J2EE规范，站在Java开发的角度思考问题。但是现实往往是复杂度很高的。

JMS实现：ActiveMQ、Redis、HornetMQ、RabbitMQ、RocketMQ（没有完全遵守JMS规范）

#### AMQP

AMQP（advanced message queuing protocol）：一种协议（高级消息队列协议，也是消息代理规范），规范了网络交换的数据格式，兼容JMS操作。

优点：具有跨平台性，服务器供应商，生产者，消费者可以使用不同的语言来实现

**AMQP消息模型**

- direct exchange
- fanout exchange
- topic exchange
- headers exchange
- system exchange

**AMQP消息种类：byte[]**：有效解决跨平台的问题

目前实现了AMQP协议的消息中间件技术也很多，而且都是较为流行的技术，例如：RabbitMQ、StormMQ、RocketMQ

#### MQTT

MQTT（Message Queueing Telemetry Transport）消息队列遥测传输，专为小设备设计，是物联网（IOT）生态系统中主要成分之一。

**购物订单发送手机短信案例**

手机验证码案例需求如下：

- 执行下单业务时（模拟此过程），调用消息服务，将要发送短信的订单id传递给消息中间件

- 消息处理服务接收到要发送的订单id后输出订单id（模拟发短信）

  由于不涉及数据读写，仅开发业务层与表现层，其中短信处理的业务代码独立开发，代码如下：

**订单业务**

- 业务层接口

  ```java
  public interface OrderService {
      void order(String id);
  }
  ```

  模拟传入订单id，执行下订单业务，参数为虚拟设定，实际应为订单对应的实体类

- 业务层实现

  ```java
  @Service
  public class OrderServiceImpl implements OrderService {
      @Autowired
      private MessageService messageService;
      
      @Override
      public void order(String id) {
          //一系列操作，包含各种服务调用，处理各种业务
          System.out.println("订单处理开始");
          //短信消息处理
          messageService.sendMessage(id);
          System.out.println("订单处理结束");
          System.out.println();
      }
  }
  ```

  业务层转调短信处理的服务MessageService

- 表现层服务

  ```java
  @RestController
  @RequestMapping("/orders")
  public class OrderController {
  
      @Autowired
      private OrderService orderService;
  
      @PostMapping("{id}")
      public void order(@PathVariable String id){
          orderService.order(id);
      }
  }
  ```

  表现层对外开发接口，传入订单id即可（模拟）

**短信处理业务**

- 业务层接口

  ```java
  public interface MessageService {
      void sendMessage(String id);
      String doMessage();
  }
  ```

  短信处理业务层接口提供两个操作，发送要处理的订单id到消息中间件，另一个操作目前暂且设计成处理消息，实际消息的处理过程不应该是手动执行，应该是自动执行，到具体实现时再进行设计

- 业务层实现

  ```java
  @Service
  public class MessageServiceImpl implements MessageService {
      private ArrayList<String> msgList = new ArrayList<String>();
  
      @Override
      public void sendMessage(String id) {
          System.out.println("待发送短信的订单已纳入处理队列，id："+id);
          msgList.add(id);
      }
  
      @Override
      public String doMessage() {
          String id = msgList.remove(0);
          System.out.println("已完成短信发送业务，id："+id);
          return id;
      }
  }
  ```

  短信处理业务层实现中使用集合先模拟消息队列，观察效果

- 表现层服务

  ```java
  @RestController
  @RequestMapping("/msgs")
  public class MessageController {
  
      @Autowired
      private MessageService messageService;
  
      @GetMapping
      public String doMessage(){
          String id = messageService.doMessage();
          return id;
      }
  }
  ```

  短信处理表现层接口暂且开发出一个处理消息的入口，但是此业务是对应业务层中设计的模拟接口，实际业务不需要设计此接口。

#### ActiveMQ

ActiveMQ是MQ产品中的元老级产品，早期标准MQ产品之一，在AMQP协议没有出现之前，占据了消息中间件市场的绝大部分份额，后期因为AMQP系列产品的出现，迅速走弱，目前仅在一些线上运行的产品中出现，新产品开发较少采用。

**启动服务器**

在对应目录下打开cmd

```shell
activemq-admin.bat start
```

默认对外服务端口61616

**访问web管理服务**

```
http://127.0.0.1:8161/
```

首先输入访问用户名和密码，初始化用户名和密码相同，均为：admin

**SpringBoot整合ActiveMQ**

- 导入springboot整合ActiveMQ的starter

  ```xml
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-activemq</artifactId>
  </dependency>
  ```

- 配置ActiveMQ的服务器地址

  ```yaml
  spring:
    activemq:
      broker-url: tcp://localhost:61616
    #指定消息默认存储位置
    jms: 
      template:
        default-destination: zxp
  ```

- 使用JmsMessagingTemplate操作ActiveMQ

  ```java
  @Service
  public class MessageServiceActivemqImpl implements MessageService {
      @Autowired
      private JmsMessagingTemplate messagingTemplate;
  
      @Override
      public void sendMessage(String id) {
          System.out.println("待发送短信的订单已纳入处理队列，id："+id);
          messagingTemplate.convertAndSend("order.queue.id",id);
          //order.queue.id：消息在队列存储的名称
      }
  
      @Override
      public String doMessage() {
          String id = messagingTemplate.receiveAndConvert("order.queue.id",String.class);
          System.out.println("已完成短信发送业务，id："+id);
          return id;
      }
  }
  ```

  发送消息需要先将消息的类型转换成字符串，然后再发送，所以是convertAndSend，定义消息发送的位置，和具体的消息内容，此处使用id作为消息内容。

  接收消息需要先将消息接收到，然后再转换成指定的数据类型，所以是receiveAndConvert，接收消息除了提供读取的位置，还要给出转换后的数据的具体类型。

- 使用消息监听器在服务器启动后，监听指定位置，当消息出现后，立即消费消息

  ```java
  @Component
  public class MessageListener {
      //@JmsListener定义当前方法监听ActiveMQ中指定名称的消息队列
      @JmsListener(destination = "order.queue.id")
      //如果当前消息队列处理完还需要继续向下传递当前消息到另一个队列中使用注解@SendTo即可，这样即可构造连续执行的顺序消息队列，传递的是当前方法的返回值
      @SendTo("order.other.queue.id")
      public String receive(String id){
          System.out.println("已完成短信发送业务，id："+id);
          return "new:"+id;
      }
  }
  ```

- 切换消息模型由点对点模型到发布订阅模型，修改jms配置即可

  ```yaml
  spring:
    activemq:
      broker-url: tcp://localhost:61616
    jms:
      pub-sub-domain: true #pub-sub-domain默认值为false，即点对点模型，修改为true后就是发布订阅模型。
  ```

#### RabbitMQ

Erlang安装

windows版安装包下载地址：[https](https://www.erlang.org/downloads)[://www.erlang.org/downloads](https://www.erlang.org/downloads)

下载完毕后得到exe安装文件，一键傻瓜式安装，安装完毕需要重启，安装的过程中可能会出现依赖Windows组件的提示，根据提示下载安装即可，都是自动执行的

Erlang安装后需要配置环境变量，否则RabbitMQ将无法找到安装的Erlang。需要配置项如下，作用等同JDK配置环境变量的作用。

- ERLANG_HOME
- PATH

**安装RabbitMQ后**

启动服务器

```
rabbitmq-service.bat start		# 启动服务
rabbitmq-service.bat stop		# 停止服务
rabbitmqctl status				# 查看服务状态
```

启动rabbitmq的过程实际上是开启rabbitmq对应的系统服务，需要管理员权限方可执行。activemq与rabbitmq有一个端口冲突问题，请确保另一个处于关闭状态。

**访问web管理服务**

RabbitMQ也提供有web控制台服务，但是此功能是一个插件，需要先启用才可以使用。

```shell
rabbitmq-plugins.bat list						  # 查看当前所有插件的运行状态
rabbitmq-plugins.bat enable rabbitmq_management    # 启动rabbitmq_management插件
```

启动插件后可以在插件运行状态中查看是否运行，运行后通过浏览器即可打开服务后台管理界面

```
http://localhost:15672
```

首先输入访问用户名和密码，初始化用户名和密码相同，均为：guest

**SpringBoot整合RabbitMQ(direct模型)**

RabbitMQ满足AMQP协议，因此不同的消息模型对应的制作不同，先使用最简单的direct模型开发。

- 导入springboot整合amqp的starter，amqp协议默认实现为rabbitmq方案

  ```xml
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-amqp</artifactId>
  </dependency>
  ```

- 配置RabbitMQ的服务器地址

  ```yaml
  spring:
    rabbitmq:
      host: localhost
      port: 5672
  ```

- 初始化直连模式系统设置

  由于RabbitMQ不同模型要使用不同的交换机，因此需要先初始化RabbitMQ相关的对象，例如队列，交换机等

  ```java
  @Configuration
  public class RabbitConfigDirect {
      @Bean
      public Queue directQueue(){
          return new Queue("direct_queue");
      }
      @Bean
      public Queue directQueue2(){
          return new Queue("direct_queue2");
      }
      @Bean
      public DirectExchange directExchange(){
          return new DirectExchange("directExchange");
      }
      @Bean
      public Binding bindingDirect(){
          return BindingBuilder.bind(directQueue()).to(directExchange()).with("direct");
      }
      @Bean
      public Binding bindingDirect2(){
          return BindingBuilder.bind(directQueue2()).to(directExchange()).with("direct2");
      }
  }
  ```

  队列Queue与直连交换机DirectExchange创建后，还需要绑定他们之间的关系Binding，这样就可以通过交换机操作对应队列。

- 使用AmqpTemplate操作RabbitMQ

  ```java
  @Service
  public class MessageServiceRabbitmqDirectImpl implements MessageService {
      @Autowired
      private AmqpTemplate amqpTemplate;
  
      @Override
      public void sendMessage(String id) {
          System.out.println("待发送短信的订单已纳入处理队列（rabbitmq direct），id："+id);
          amqpTemplate.convertAndSend("directExchange","direct",id);
      }
  }
  ```

- 使用消息监听器在服务器启动后，监听指定位置，当消息出现后，立即消费消息

  ```java
  @Component
  public class MessageListener {
      //使用注解@RabbitListener定义当前方法监听RabbitMQ中指定名称的消息队列
      @RabbitListener(queues = "direct_queue")
      public void receive(String id){
          System.out.println("已完成短信发送业务(rabbitmq direct)，id："+id);
      }
  }
  ```

SpringBoot整合RabbitMQ(topic模型)

1. 同上

2. 同上

3. 初始化主题模式系统设置

   ```java
   @Configuration
   public class RabbitConfigTopic {
       @Bean
       public Queue topicQueue(){
           return new Queue("topic_queue");
       }
       @Bean
       public Queue topicQueue2(){
           return new Queue("topic_queue2");
       }
       @Bean
       public TopicExchange topicExchange(){
           return new TopicExchange("topicExchange");
       }
       @Bean
       public Binding bindingTopic(){
           return BindingBuilder.bind(topicQueue()).to(topicExchange()).with("topic.*.id");
       }
       @Bean
       public Binding bindingTopic2(){
           return BindingBuilder.bind(topicQueue2()).to(topicExchange()).with("topic.orders.*");
       }
   }
   ```

   主题模式支持routingKey匹配模式，*表示匹配一个单词，#表示匹配任意内容，这样就可以通过主题交换机将消息分发到不同的队列中，详细内容请参看RabbitMQ系列课程。

   | **匹配键**        | **topic.\*.\*** | **topic.#** |
      | ----------------- | --------------- | ----------- |
   | topic.order.id    | true            | true        |
   | order.topic.id    | false           | false       |
   | topic.sm.order.id | false           | true        |
   | topic.sm.id       | false           | true        |
   | topic.id.order    | true            | true        |
   | topic.id          | false           | true        |
   | topic.order       | false           | true        |

4. 使用AmqpTemplate操作RabbitMQ

   ```java
   @Service
   public class MessageServiceRabbitmqTopicImpl implements MessageService {
       @Autowired
       private AmqpTemplate amqpTemplate;
   
       @Override
       public void sendMessage(String id) {
           System.out.println("待发送短信的订单已纳入处理队列（rabbitmq topic），id："+id);
           amqpTemplate.convertAndSend("topicExchange","topic.orders.id",id);
       }
   }
   ```

   发送消息后，根据当前提供的routingKey与绑定交换机时设定的routingKey进行匹配，规则匹配成功消息才会进入到对应的队列中。

5. 使用消息监听器在服务器启动后，监听指定队列

   ```java
   @Component
   public class MessageListener {
       //使用注解@RabbitListener定义当前方法监听RabbitMQ中指定名称的消息队列
       @RabbitListener(queues = "topic_queue")
       public void receive(String id){
           System.out.println("已完成短信发送业务(rabbitmq topic 1)，id："+id);
       }
       @RabbitListener(queues = "topic_queue2")
       public void receive2(String id){
           System.out.println("已完成短信发送业务(rabbitmq topic 22222222)，id："+id);
       }
   }
   ```

#### RocketMQ

RocketMQ由阿里研发，后捐赠给apache基金会，目前是apache基金会顶级项目之一，也是目前市面上的MQ产品中较为流行的产品之一，它遵从AMQP协议。

**安装**

windows版安装包下载地址：[https://rocketmq.apache.org](https://rocketmq.apache.org/)

RocketMQ安装后需要配置环境变量，具体如下：

- ROCKETMQ_HOME
- PATH
- NAMESRV_ADDR(建议): 127.0.0.1:9876

**RocketMQ工作模式**

在RocketMQ中，处理业务的服务器称为broker，生产者与消费者不是直接与broker联系的，而是通过命名服务器进行通信。broker启动后会通知命名服务器自己已经上线，这样命名服务器中就保存有所有的broker信息。当生产者与消费者需要连接broker时，通过命名服务器找到对应的处理业务的broker，因此命名服务器在整套结构中起到一个信息中心的作用。并且broker启动前必须保障命名服务器先启动。

![image-20220228175123790](D:\学习\SpringBoot\开发实用篇—资料\img\image-20220228175123790.png)

**启动服务器**

```shell
mqnamesrv		# 启动命名服务器
mqbroker		# 启动broker
```

运行bin目录下的mqnamesrv命令即可启动命名服务器，默认对外服务端口9876。

运行bin目录下的mqbroker命令即可启动broker服务器，如果环境变量中没有设置NAMESRV_ADDR则需要在运行mqbroker指令前通过set指令设置NAMESRV_ADDR的值，并且每次开启均需要设置此项。

**测试服务器启动状态**

RocketMQ提供有一套测试服务器功能的测试程序，运行bin目录下的tools命令即可使用。

```shell
tools org.apache.rocketmq.example.quickstart.Producer		# 生产消息
tools org.apache.rocketmq.example.quickstart.Consumer		# 消费消息
```

**SpringBoot整合RocketMQ（异步消息）**

- 导入springboot整合RocketMQ的starter，此坐标不由springboot维护版本

  ```xml
  <dependency>
      <groupId>org.apache.rocketmq</groupId>
      <artifactId>rocketmq-spring-boot-starter</artifactId>
      <version>2.2.1</version>
  </dependency>
  ```

- 配置RocketMQ的服务器地址

  ```yaml
  rocketmq:
    name-server: localhost:9876
    producer:
      group: group_rocketmq #设置默认的生产者消费者所属组group
  ```

- 使用RocketMQTemplate操作RocketMQ

  ```java
  @Service
  public class MessageServiceRocketmqImpl implements MessageService {
      @Autowired
      private RocketMQTemplate rocketMQTemplate;
  
      @Override
      public void sendMessage(String id) {
          System.out.println("待发送短信的订单已纳入处理队列（rocketmq），id："+id);
          SendCallback callback = new SendCallback() {
              @Override
              public void onSuccess(SendResult sendResult) {
                  System.out.println("消息发送成功");
              }
              @Override
              public void onException(Throwable e) {
                  System.out.println("消息发送失败！！！！！");
              }
          };
          //使用asyncSend方法发送异步消息
          rocketMQTemplate.asyncSend("order_id",id,callback);
      }
  }
  ```

- 使用消息监听器在服务器启动后，监听指定位置，当消息出现后，立即消费消息

  ```java
  @Component
  //使用注解@RocketMQMessageListener定义当前类监听RabbitMQ中指定组、指定名称的消息队列。
  @RocketMQMessageListener(topic = "order_id",consumerGroup = "group_rocketmq")
  public class MessageListener implements RocketMQListener<String> {
      @Override
      public void onMessage(String id) {
          System.out.println("已完成短信发送业务(rocketmq)，id："+id);
      }
  }
  ```

  RocketMQ的监听器必须按照标准格式开发，实现RocketMQListener接口，泛型为消息类型。

#### KafKa

Kafka，一种高吞吐量的分布式发布订阅消息系统，提供实时消息功能。Kafka技术并不是作为消息中间件为主要功能的产品，但是其拥有发布订阅的工作模式，也可以充当消息中间件来使用，而且目前企业级开发中其身影也不少见。

**安装**

windows版安装包下载地址：[https://](https://kafka.apache.org/downloads)[kafka.apache.org/downloads](https://kafka.apache.org/downloads)

**启动服务器**

kafka服务器的功能相当于RocketMQ中的broker，kafka运行还需要一个类似于命名服务器的服务。在kafka安装目录中自带一个类似于命名服务器的工具，叫做zookeeper，它的作用是注册中心

```shell
zookeeper-server-start.bat ..\..\config\zookeeper.properties		# 启动zookeeper
kafka-server-start.bat ..\..\config\server.properties				# 启动kafka
```

运行bin目录下的windows目录下的zookeeper-server-start命令即可启动注册中心，默认对外服务端口2181。

运行bin目录下的windows目录下的kafka-server-start命令即可启动kafka服务器，默认对外服务端口9092。

**创建主题**

和之前操作其他MQ产品相似，kakfa也是基于主题操作，操作之前需要先初始化topic

```shell
# 创建topic
kafka-topics.bat --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic itheima
# 查询topic
kafka-topics.bat --zookeeper 127.0.0.1:2181 --list					
# 删除topic
kafka-topics.bat --delete --zookeeper localhost:2181 --topic itheima
```

**测试服务器启动状态**

Kafka提供有一套测试服务器功能的测试程序，运行bin目录下的windows目录下的命令即可使用

```shell
kafka-console-producer.bat --broker-list localhost:9092 --topic itheima							# 测试生产消息
kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic itheima --from-beginning	# 测试消息消费
```

**SpringBoot整合Kafka**

- 导入springboot整合Kafka的starter，此坐标由springboot维护版本

  ```xml
  <dependency>
      <groupId>org.springframework.kafka</groupId>
      <artifactId>spring-kafka</artifactId>
  </dependency>
  ```

- 配置Kafka的服务器地址

  ````yaml
  spring:
    kafka:
      bootstrap-servers: localhost:9092
      consumer:
        group-id: order #设置默认的生产者消费者所属组id。
  ````

- 使用KafkaTemplate操作Kafka

  ```java
  @Service
  public class MessageServiceKafkaImpl implements MessageService {
      @Autowired
      private KafkaTemplate<String,String> kafkaTemplate;
  
      @Override
      public void sendMessage(String id) {
          System.out.println("待发送短信的订单已纳入处理队列（kafka），id："+id);
          //使用send方法发送消息，需要传入topic名称
          kafkaTemplate.send("itheima2022",id);
      }
  }
  ```

- 使用消息监听器在服务器启动后，监听指定位置，当消息出现后，立即消费消息

  ```java
  @Component
  public class MessageListener {
      //@KafkaListener定义当前方法监听Kafka中指定topic的消息
      @KafkaListener(topics = "itheima2022")
      public void onMessage(ConsumerRecord<String,String> record){
          System.out.println("已完成短信发送业务(kafka)，id："+record.value());
      }
  }
  ```

  接收到的消息封装在对象ConsumerRecord中，获取数据从ConsumerRecord对象中获取即可

### 监控

监控就是通过软件的方式展示另一个软件的运行情况，运行的情况则通过各种各样的指标数据反馈给监控人员。例如网络是否顺畅、服务器是否在运行、程序的功能是否能够整百分百运行成功，内存是否够用，等等等等。

**监控的意义**

由于现在的互联网程序大部分都是基于微服务的程序，一个程序的运行需要若干个服务来保障，因此第一个要监控的指标就是服务是否正常运行，也就是**监控服务状态是否处理宕机状态**。一旦发现某个服务宕机了，必须马上给出对应的解决方案，避免整体应用功能受影响。其次，由于互联网程序服务的客户量是巨大的，当客户的请求在短时间内集中达到服务器后，就会出现各种程序运行指标的波动。比如内存占用严重，请求无法及时响应处理等，这就是第二个要监控的重要指标，**监控服务运行指标**。虽然软件是对外提供用户的访问需求，完成对应功能的，但是后台的运行是否平稳，是否出现了不影响客户使用的功能隐患，这些也是要密切监控的，此时就需要在不停机的情况下，监控系统运行情况，日志是一个不错的手段。如果在众多日志中找到开发者或运维人员所关注的日志信息，简单快速有效的过滤出要看的日志也是监控系统需要考虑的问题，这就是第三个要监控的指标，**监控程序运行日志**。虽然我们期望程序一直平稳运行，但是由于突发情况的出现，例如服务器被攻击、服务器内存溢出等情况造成了服务器宕机，此时当前服务不能满足使用需要，就要将其重启甚至关闭，如果快速控制服务器的启停也是程序运行过程中不可回避的问题，这就是第四个监控项，**管理服务状态**。

#### Spring Boot Admin

Spring Boot Admin，这是一个开源社区项目，用于管理和监控SpringBoot应用程序。这个项目中包含有客户端和服务端两部分，而监控平台指的就是服务端。我们做的程序如果需要被监控，将我们做的程序制作成客户端，然后配置服务端地址后，服务端就可以通过HTTP请求的方式从客户端获取对应的信息，并通过UI界面展示对应信息。

**服务端开发**

- 导入springboot admin对应的starter，版本与当前使用的springboot版本保持一致，并将其配置成web工程

  ```xml
  <dependency>
      <groupId>de.codecentric</groupId>
      <artifactId>spring-boot-admin-starter-server</artifactId>
      <version>2.5.4</version>
  </dependency>
  
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-web</artifactId>
  </dependency>
  ```

- 在引导类上添加注解@EnableAdminServer，声明当前应用启动后作为SpringBootAdmin的服务器使用

  ```java
  @SpringBootApplication
  @EnableAdminServer
  public class Springboot25AdminServerApplication {
      public static void main(String[] args) {
          SpringApplication.run(Springboot25AdminServerApplication.class, args);
      }
  }
  ```

  访问localhost:8080/applications就可以打开界面

**客户端开发**

- 导入springboot admin对应的starter，版本与当前使用的springboot版本保持一致，并将其配置成web工程

  ```xml
  <dependency>
      <groupId>de.codecentric</groupId>
      <artifactId>spring-boot-admin-starter-client</artifactId>
      <version>2.5.4</version>
  </dependency>
  
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-web</artifactId>
  </dependency>
  ```

- 设置当前客户端将信息上传到哪个服务器上，通过yml文件配置

  ```yaml
  spring:
    boot:
      admin:
        client:
          url: http://localhost:8080
  server:
    port: xx #防止和上边端口冲突
  ```

做到这里，这个客户端就可以启动了，启动后再次访问服务端程序，由于当前没有设置开放哪些信息给监控服务器，所以目前看不到什么有效的信息。下面需要再客户端做两组配置就可以看到信息了

1. 开放指定信息给服务器看

2. 允许服务器以HTTP请求的方式获取对应的信息

   配置如下：

   ```yaml
   server:
     port: 80
   spring:
     boot:
       admin:
         client:
           url: http://localhost:8080
   management:
     endpoint:
       health:
         show-details: always #可以看健康信息
     endpoints:
       web:
         exposure:
           include: "*" #查看所有13组信息
   ```

springbootadmin的客户端默认开放了13组信息给服务器，但是这些信息除了一个之外，其他的信息都不让通过HTTP请求查看，只能看到一个内容，就是健康信息。

但是即便如此我们看到健康信息中也没什么内容，原因在于健康信息中有一些信息描述了你当前应用使用了什么技术等信息，如果无脑的对外暴露功能会有安全隐患。通过配置就可以开放所有的健康信息明细查看了。

```yaml
management:
  endpoint:
    health:
      show-details: always
```

目前除了健康信息，其他信息都查阅不了。原因在于其他12种信息是默认不提供给服务器通过HTTP请求查阅的，所以需要开启查阅的内容项，使用*表示查阅全部。记得带引号。

```yaml
endpoints:
  web:
    exposure:
      include: "*"
```

**监控原理**

#### Actuator

Actuator，可以称为端点，描述了一组监控信息，SpringBootAdmin提供了多个内置端点，通过访问端点就可以获取对应的监控信息，也可以根据需要自定义端点信息。通过发送请求路劲**/actuator**可以访问应用所有端点信息，如果端点中还有明细信息可以发送请求**/actuator/端点名称**来获取详细信息。

| ID               | 描述                                                         | 默认启用 |
| ---------------- | ------------------------------------------------------------ | -------- |
| auditevents      | 暴露当前应用程序的审计事件信息。                             | 是       |
| beans            | 显示应用程序中所有 Spring bean 的完整列表。                  | 是       |
| caches           | 暴露可用的缓存。                                             | 是       |
| conditions       | 显示在配置和自动配置类上评估的条件以及它们匹配或不匹配的原因。 | 是       |
| configprops      | 显示所有 @ConfigurationProperties 的校对清单。               | 是       |
| env              | 暴露 Spring ConfigurableEnvironment 中的属性。               | 是       |
| flyway           | 显示已应用的 Flyway 数据库迁移。                             | 是       |
| health           | 显示应用程序健康信息                                         | 是       |
| httptrace        | 显示 HTTP 追踪信息（默认情况下，最后 100 个  HTTP 请求/响应交换）。 | 是       |
| info             | 显示应用程序信息。                                           | 是       |
| integrationgraph | 显示 Spring Integration 图。                                 | 是       |
| loggers          | 显示和修改应用程序中日志记录器的配置。                       | 是       |
| liquibase        | 显示已应用的 Liquibase 数据库迁移。                          | 是       |
| metrics          | 显示当前应用程序的指标度量信息。                             | 是       |
| mappings         | 显示所有 @RequestMapping 路径的整理清单。                    | 是       |
| scheduledtasks   | 显示应用程序中的调度任务。                                   | 是       |
| sessions         | 允许从 Spring Session 支持的会话存储中检索和删除用户会话。当使用 Spring Session 的响应式 Web 应用程序支持时不可用。 | 是       |
| shutdown         | 正常关闭应用程序。                                           | 否       |
| threaddump       | 执行线程 dump。                                              | 是       |
| heapdump         | 返回一个 hprof 堆 dump 文件。                                | 是       |
| jolokia          | 通过 HTTP 暴露 JMX bean（当  Jolokia 在 classpath 上时，不适用于 WebFlux）。 | 是       |
| logfile          | 返回日志文件的内容（如果已设置 logging.file 或 logging.path 属性）。支持使用 HTTP Range 头来检索部分日志文件的内容。 | 是       |
| prometheus       | 以可以由 Prometheus 服务器抓取的格式暴露指标。               | 是       |

上述端点每一项代表被监控的指标，如果对外开放则监控平台可以查询到对应的端点信息，如果未开放则无法查询对应的端点信息。通过配置可以设置端点是否对外开放功能。使用enable属性控制端点是否对外开放。其中health端点为默认端点，不能关闭。

```yaml
management:
  endpoint:
    health:						# 端点名称
      show-details: always
    info:						# 端点名称
      enabled: true				# 是否开放
```

上述端点开启后，就可以通过端点对应的路径查看对应的信息了。但是此时还不能通过HTTP请求查询此信息，还需要开启通过HTTP请求查询的端点名称，使用“*”可以简化配置成开放所有端点的WEB端HTTP请求权限。

```yaml
management:
  endpoints:
    web:
      exposure:
        include: "*"  #这里是设定是否可以查询到信息，如果端口每开放，即使设定可以查询也查询不出来
```

整体上来说，对于端点的配置有两组信息，一组是endpoints开头的，对所有端点进行配置，一组是endpoint开头的，对具体端点进行配置。

```yaml
management:
  endpoint:		# 具体端点的配置
    health:
      show-details: always
    info:
      enabled: true
  endpoints:	# 全部端点的配置
    web:
      exposure:
        include: "*"
    enabled-by-default: true #默认开启多少端点
```

#### **自定义监控指标**

##### **INFO端点**

info端点描述了当前应用的基本信息，可以通过两种形式快速配置info端点的信息

- 配置形式

  ```yaml
  info:
    appName: @project.artifactId@
    version: @project.version@
    company: 传智教育
    author: itheima
  ```

- 编程形式

  通过配置的形式只能添加固定的数据，如果需要动态数据还可以通过配置bean的方式为info端点添加信息，此信息与配置信息共存

  ```java
  @Component
  public class InfoConfig implements InfoContributor {
      @Override
      public void contribute(Info.Builder builder) {
          builder.withDetail("runTime",System.currentTimeMillis());		//添加单个信息
          Map infoMap = new HashMap();		
          infoMap.put("buildTime","2006");
          builder.withDetails(infoMap);									//添加一组信息
      }
  }
  ```

##### **Health端点**

health端点描述当前应用的运行健康指标，即应用的运行是否成功。通过编程的形式可以扩展指标信息。

```java
@Component
public class HealthConfig extends AbstractHealthIndicator {
    //实现第一个方法	
    @Override
    protected void doHealthCheck(Health.Builder builder) throws Exception {
        boolean condition = true;
        if(condition) {
            builder.status(Status.UP);					//设置运行状态为启动状态
            builder.withDetail("runTime", System.currentTimeMillis());
            Map infoMap = new HashMap();
            infoMap.put("buildTime", "2006");
            builder.withDetails(infoMap);
        }else{
            builder.status(Status.OUT_OF_SERVICE);		//设置运行状态为不在服务状态
            builder.withDetail("上线了吗？","你做梦");
        }
    }
}
```

##### Metrics端点

metrics端点描述了性能指标，除了系统自带的监控性能指标，还可以自定义性能指标。

```java
@Service
public class BookServiceImpl extends ServiceImpl<BookDao, Book> implements IBookService {
    @Autowired
    private BookDao bookDao;

    private Counter counter;

    public BookServiceImpl(MeterRegistry meterRegistry){
        counter = meterRegistry.counter("用户付费操作次数：");
    }

    @Override
    public boolean delete(Integer id) {
        //每次执行删除业务等同于执行了付费业务
        counter.increment();
        return bookDao.deleteById(id) > 0;
    }
}
```

##### 自定义端点

可以根据业务需要自定义端点，方便业务监控

```java
@Component
@Endpoint(id="pay",enableByDefault = true)
public class PayEndpoint {
    @ReadOperation
    public Object getPay(){
        Map payMap = new HashMap();
        payMap.put("level 1","300");
        payMap.put("level 2","291");
        payMap.put("level 3","666");
        return payMap;
    }
}
```

由于此端点数据spirng boot admin无法预知该如何展示，所以通过界面无法看到此数据，通过HTTP请求路径可以获取到当前端点的信息，但是需要先开启当前端点对外功能，或者设置当前端点为默认开发的端点。

