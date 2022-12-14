---
title: SSM笔记
date: 2021-07-21
tags: SSM
categories: 笔记
description: SSM笔记
cover: https://zhuxinpeng.oss-cn-hangzhou.aliyuncs.com/12.jpg
---

SSM包含：Spring、SpringMVC、Maven高级、SpringBoot、MyBatisPlus

# Spring

最常见的Spring有：Spring Framework、Spring Boot、Spring Cloud

Spring环境配置：在pom文件中导入Spring坐标

```xml
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.2.10.RELEASE</version>
```

Spring Framework是Spring生态圈中最基础的项目，是其他项目的根基

Spring Framework系统框架：

- Core Container：核心容器（Beans、Core、Context、SpEL）
- AOP：面向切面编程
- Aspects：AOP思想实现
- Data Acess/Integration：数据访问/集成（JDBC、ORM、OXM、JMS、Transactions事务）
- Web：web开发（WebSocket、Servlet、Web、Portlet）
- Test：单元测试与集成测试

## Core Container

Core Container：容器基本操作

Core Container（核心容器）：Beans、Core、Context、SpEL

- IoC(控制反转):从主动new对象转换为由外部提供对象，对象的创建控制权由程序转移到外部
- IoC容器:负责对象的创建，初始化等一系列工作，被创建或管理的对象在IoC容器中统称为Bean
- Bean:被创建或管理的对象在IoC容器中统称为Bean
- DI(依赖注入):在容器中建立bean与bean之间的依赖关系的整个过程

### IoC

- 在resources文件夹下创建spring config文件

- 配置bean：id表示bean的名字，class属性表示给bean定义类型，name表示别名

  ```xml
  <bean id="bookDao" name="service service2" class="com.zxp.dao.impl.BookDaoImpl"/>
  ```

- 获取IoC容器：

  ```java
  ApplicationContext ctx = new ClassPathXmlApplicationContext("spring config文件");
  ```

- 创建对象

  ```java
  BookDao bookDao = (BookDao) ctx.getBean("bookDao/别名");
  ```

- 配置关系：

  ```xml
      <bean id="bookDao" class="com.zxp.dao.impl.BookDaoImpl"/>
      <bean id="bookService" class="com.zxp.service.impl.BookServiceImpl">
          <property name="bookDao" ref="bookDao"/>
      </bean>
  ```

  property标签表示配置当前bean的属性

  name属性表示配置哪一个具体的属性

  ref属性表示参照哪一个bean

### Bean

- id：bean的名称

- name：别名

- class：bean定义类型

- scope：作用范围

  singleton：单例（默认）

  prototype：非单例


```xml
<bean id="xxx" name="xxx xyx" class="x.x.x.x" scope="singleton/prototype"/>
```

**适合交给容器进行管理的bean：**

- 表现层对象：如servlet
- 业务层对象：如service
- 数据层对象：如dao
- 工具对象

不适合交给容器管理的bean：封装实体的域对象

**Bean构造对象的方法：**

- 使用构造方法：Spring创建bean调用的是无参构造器
- 使用静态工厂

```xml
<bean id="xxx" class="com.zxp.factory.xxxFactory" factory-method="getxxx"/>
```

id：bean的名称   class：工厂类   factory-method：调用工厂的方法

- 使用实例工厂

 ```xml
<bean id="xxxFactory" class="com.zxp.factory.xxxFactory"/>
<bean id="xxx" factory-method="getxxx" fatory-bean="xxxFactory"/>
 ```

```java
Xxx xxx = (Xxx)ctx.getBean("xxx")
```

- 使用FactoryBean实例化bean

```java
public class UserDaoFactoryBean implements FactoryBean<UserDao>{
   public UserDao getObject() throws Exception{
      return new UserDaoImpl();
   }
    public Class<?> getObjectType(){
       return UserDao.class;
    }
    public boolean isSingleton(){
        return true/false;     //单例/非单例
    }
}
<bean id="xxx" class="com.zxp.factory.xxxFactoryBean"/>
```

```java
Xxx xxx = (Xxx)ctx.getBean("xxx")
```

**bean生命周期控制：**

- 提供生命周期控制方法，并在bean定义时配置对应方法
- 接口控制，实现类继承InitializingBean和DisposableBean接口

**bean生命周期**：

- 初始化容器
  1. 创建对象（内存分配）
  2. 执行构造方法
  3. 执行属性注入（set操作）
  4. 执行bean初始化方法
- 使用bean：执行业务操作
- 关闭/销毁容器：执行bean销毁方法

### DI

DI(依赖注入):在容器中建立bean与bean之间的依赖关系的整个过程

**依赖注入方式：**

- setter注入

  - 简单类型

    ```xml
    <bean id="xxx" class="com.zxp.dao.impl.xxxImpl"/>
       <property name="age" value="10"/>
    </bean>
    ```

  - 引用类型

    ```xml
    <bean id="bookService" class="com.zxp.dao.impl.BookServiceImpl"/>
       <property name="xxx" ref="xxx"/>
    </bean>
    ```

- 构造器注入

  - 简单类型

    ```xml
    <bean id="bookService" class="com.zxp.service.impl.BookServiceImpl">
       <constructor-arg name="age" value="10"/>
    </bean>
    ```

  - 引用类型

    ```xml
    <bean id="xxx" class="com.zxp.dao.impl.xxxImpl"/>
       <constructor-arg name="bookDao(有参构造器中的形参)" ref="bookDao"/>
    </bean>
    ```


**依赖自动装配：**IoC容器根据bean所依赖的资源在容器中自动查找并注入bean中的过程称为自动装配

自动装配方式：

- 按类型

  ```xml
  <bean id="bookDao" class="com.zxp.dao.impl.BookDaoImpl"/>
  <bean id="bookService" class="com.zxp.service.impl.BookServiceImpl" autowire="byType"/>
  ```

- 按名称

- 按构造方法

- 不启用自动装配

依赖自动装配特征：

- 自动装配用于引用类型依赖注入，不能对简单类型进行操作
- 使用按类型装配时必须保障容器中相同类型的bean唯一，推荐使用
- 使用按名称装配时必须保障容器中具有指定名称的bean，因变量名与配置耦合，不推荐使用
- 自动装配优先级低于setter注入与构造器注入，同时出现时自动装配配置失效

**集合注入**

- 数组

  ```xml 
  <property name="array">
     <array>
       <value>100</value>
       <value>200</value>
     </array>
  </property>
  ```

- List

  ```xml
  <property name="list">
     <list>
       <value>100</value>
       <value>200</value>
     </list>
  </property>
  ```

- Set

  ```xml
  <property name="set">
     <set>
       <value>100</value>
       <value>200</value>
     </set>
  </property>
  ```

- Map

  ```xml
  <property name="map">
     <map>
       <entry key="age" value="10">
       <entry key="country" value="singapore">
     </map>
  </property>
  ```

- Property

  ```xml
  <property name="properties">
     <props>
       <prop key="age">10</prop>
       <prop key="country">singapore</prop>
     </props>
  </property>
  ```

Spring文件加载外部文件

- 开启context命名空间

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:context="http://www.springframework.org/schema/context"
         xsi:schemaLocation="
         http://www.springframework.org/schema/beans
         http://www.springframework.org/schema/beans/spring-beans.xsd
         http://www.springframework.org/schema/context
         http://www.springframework.org/schema/context/spring-context.xsd">
  </beans>
  ```

- 使用context加载properties文件

  ```xml
  <context:property-placeholder location="jdbc.properties"/>
  ```

- 使用属性占位符${}读取properties文件中的属性

  ```
  <property name="url" value="${jdbc.url}"/>
  ```



- 加载多个properties文件：

  ```xml
  <context:property-placeholder location="jdbc.properties,jdbc2.properties"/>
  ```

- 加载所有properties文件

  ```xml
  <context:property-placeholder location="*.properties"/>
  ```

- 加载所有properties文件标准格式

  ```xml
  <context:property-placeholder location="classpath:*.properties"/>
  ```

- 从类路径或jar包中搜索并加载properties文件

  ```xml
  <context:property-placeholder location="classpath*:*.properties"/>
  ```

  **注解开发定义bean**

  1. 核心配置文件种通过组件扫描加载bean

     ```xml
     <context:component-scan base-package="com.zxp"/>
     ```

  2. 使用@Component定义bean

     ```java
     @Component("bookDao")
     public class BookDaoImpl implements BookDao{}
     @Component
     public class BookDaoImpl implements BookDao{}
     ```

  3. 写注解时根据名字获取ctx.get("bookDao")，不写名字根据类获取ctx.get(BookDao.class)

**Spring提供@Component注解的三个衍生注解(本质没有区别)**

- @Controller：用于表现层bean定义
- @Service：用于业务层bean定义
- @Repository：用于数据层bean定义

**纯注解开发模式：**

1. 定义config.SpringConfig类

2. 在类上加上@Configuration注解

3. 继续加上@ComponentScan("com.zxp")注解，只能添加一次，多个用","隔开

4. 加载配置类初始化容器

   ```java
   ApplicationContext ctx = new AnnotationConfigApplicationContext(SpringConf.class)
   ```

- 注解开发控制单例或非单例：在bean的上方加上@Scope("singleton/prototype")默认单例


- 注解开发控制生命周期：在控制方法上加上注解，初始化@PostConstruct，销毁前@PreDestroy


- 注解开发依赖注入引用类型：在成员变量上加上@Autowired，此时不需要set方法也可以，当一个成员变量有多个实现类时加上@Qualifier("bean的名称")

  ```java
  @Component
  public class BookServiceImpl implements BookService{
  @Autowired
  @Qualifier("bean的名称")
  private BookDao bookDao;}
  ```

- 注解开发依赖注入普通类型：

  ```java
  @Value("xiaohaha")
  private String name;
  ```

- 注解开发SpringConfig类加载配置文件：@PropertySource("文件名")//不支持使用通配符

  ```java
  @Value("${name}")//name来自于配置文件
  private String name;
  ```

- 注解开发管理第三方bean

  - 在SpringConfig文件中定义一个方法获得要管理的对象

  - 添加@Bean表示当前对象返回值为bean

    ```java
    @Bean
    public DataSource dataSource(){
       DruidDataSource ds = new DruidDataSource();
       ds.setxxx();
       ds.setxxx();
       return ds;
    }
    ```

  - 或者单独定义一个类在SpringConfig种用@Import导入该类，加载多个用{}数组

- 注解开发第三方依赖注入

  - 简单类型：定义成员变量，并用@Value赋值

    ```java
    @Value("10")
    private int age;
    @Bean
    public DataSource dataSource(){
       DruidDataSource ds = new DruidDataSource();
       ds.setAge(age);
       return ds;
    }
    ```

  - 引用类型：只需要为bean定义方法设置形参就会自动匹配

## Data Acess/Integration

Data Acess/Integration：整合数据层技术MyBatis

Data Acess/Integration：数据访问/数据集成（JDBC、ORM、OXM、JMS、Transactions事务）

### Mybatis

**Spring整合Mybatis**

- pom文件：spring-context，druid，mybatis，mysql，spring-jdbc，mybatis-spring

  - spring-context：Spring坐标
  - druid：数据库连接池
  - spring-jdbc：spring操作数据库有关
  - mybatis-spring：Spring整合Mybatis的坐标

- 注解开发：

  - 生成SpringConfig文件

    ```java
    @Configuration
    @ComponentScan("com.zxp")
    @PropertySource("classpath:jdbc.properties")
    @Import({JdbcConfig.class,MybatisConfig.class})
    public class SpringConfig {}
    ```

  - 生成JdbcConfig文件

    ```java
    public class JdbcConfig {
        @Value("${jdbc.driver}")
        private String driver;
        @Value("${jdbc.url}")
        private String url;
        @Value("${jdbc.username}")
        private String username;
        @Value("${jdbc.password}")
        private String password;
        
        @Bean
        public DataSource dataSource(){
            DruidDataSource ds = new DruidDataSource();
            ds.setDriverClassName(driver);
            ds.setUrl(url);
            ds.setUsername(username);
            ds.setPassword(password);
            return ds;}}
    ```

  - 生成MybatisConfig文件

    ```java
    public class MybatisConfig {
        @Bean
        public SqlSessionFactoryBean sqlSessionFactory(DataSource dataSource){
            SqlSessionFactoryBean ssfb = new SqlSessionFactoryBean();
            ssfb.setTypeAliasesPackage("com.zxp.domain");
            ssfb.setDataSource(dataSource);
            return ssfb; }
        @Bean
        public MapperScannerConfigurer mapperScannerConfigurer(){
            MapperScannerConfigurer msc = new MapperScannerConfigurer();
            msc.setBasePackage("com.zxp.dao");
            return msc;}} 
    ```

Spring整合Junit

- pom文件：在原来基础上导入junit和spring-test
- 在测试类上设置类运行器@RunWith(SpringJUnit4ClassRunner.class)
- 配置Spring环境@ContextConfiguration(classes = SpringConfig.class)

## AOP和Aspects

AOP和Aspects：AOP基础操作和实用开发

AOP：面向切面编程

Aspects：AOP思想实现

### AOP

AOP：在不惊动原始设计的基础上为其进行功能增强

AOP核心概念

- 连接点：代表所有方法
- 切入点：要追加功能的方法
- 通知：共性功能
- 通知类：定义通知的类
- 切面：切入点与通知之间的关系

注解开发AOP

1. 导入坐标：spring-context(AOP包含在内)，aspectjweaver

2. 制作连接点方法：所有方法

3. 制作共性功能：

   ```java
   public class MyAdvice{
     public void method(){
       System.out.println(System.currentTimeMills());
   }}
   ```

4. 定义切入点

   ```java
   public class MyAdvice{
       @Pointcut("execution(void com.itheima.dao.BookDao.update())")
       private void pt(){} //方法名任意
   }
   ```

5. 绑定切入点与通知的关系（切面）

   ```java
       //设置在切入点pt()的前面运行当前操作（前置通知）
       @Before("pt()")
       public void method(){
           System.out.println(System.currentTimeMillis());
       }
   ```

6. 在该类上加上@Component变成Spring控制的bean，再加上@Aspect当作AOP处理

   ```java
   @Component//通知类必须配置成Spring管理的bean
   @Aspect//设置当前类为切面类类
   public class MyAdvice {}
   ```

7. 在SpringConfig中加上@EnableAspectJAutoProxy（对应@Aspect）

**AOP的原理是通过原始对象的代理对象实现**，若是切入点与通知匹配，则使用代理对象，若不匹配则使用原始对象

![image-20220922175938788](C:\Users\98110\AppData\Roaming\Typora\typora-user-images\image-20220922175938788.png)

![image-20220922180114248](C:\Users\98110\AppData\Roaming\Typora\typora-user-images\image-20220922180114248.png)

*表示任意但是必须有且只有一个，..也表示任意，可以一个也没有，也可以很多

书写技巧：

- 描述切入点通常描述接口，而不描述实现类
- 书写包名尽量不用..匹配效率过低

AOP通知类型

- 前置通知：@Before

- 后置通知：@After

- 环绕通知：@Around

  ```java
      @Around("pt()")
      public Object around(ProceedingJoinPoint pjp){
          System.out.println("around before advice");
          Object ret = pjp.proceed();//强制抛异常
          System.out.println("around after advice");
          return ret;
      }
  ```

- 返回后通知

- 抛出异常后通知

获得被追加功能方法的信息：

```java
    @Around("pt()")
    public Object around(ProceedingJoinPoint pjp){
        Signature signature = pjp.getSignature();
        signature.getDeclaringTypeName();//获取被追加的类名	
        signature.getName();//获取被追加的方法名
        Object ret = pjp.proceed();//强制抛异常
        return ret;
    }
```

获取被追加功能方法的参数

- JoinPoint：适用于前置，后置，返回后，抛出异常后

  ```java
  @After("pt()")
  public void after(JoinPoint jp){
   Object[] args = jp.getArgs();}
  ```

- ProceedingJoinPoint：适用于环绕通知

  ```java
  @After("pt()")
  public void after(ProceedingJoinPoint pjp){
   Object[] args = pjp.getArgs();}
  ```


## Transactions

Transactions：事务实用开发

事务作用：在数据层保障一系列的数据库操作同成功同失败

Spring事务作用：在数据层或业务层保障一系列的数据库操作同成功同失败

Spring事务：

- 在方法接口上用注解@Transactional标注事务

- 在jdbc配置文件中配置事务管理器，并交给Spring管理

  ```java
      @Bean
      public PlatformTransactionManager transactionManager(DataSource dataSource){
          DataSourceTransactionManager ds = new DataSourceTransactionManager();
          ds.setDataSource(dataSource);
          return ds;
      }
  ```

- 在Spring配置文件中引入事务：用@EnableTransactionManagement标注

注解式事务可以添加到方法上表示当前方法开启事务，也可以添加到接口上表示当前接口所有方法开启事务。

事务管理员：管理事务的总方法，事务协调员：被管理的方法

```java
@Transactional
public void transfer(String out,String in,Double money){
   accountDao.outMoney(out,money);
   accountDao.inMoney(in,money);}
```

事务相关配置

![image-20220924093357995](C:\Users\98110\AppData\Roaming\Typora\typora-user-images\image-20220924093357995.png)

回滚事务的异常：error系，运行时异常

```java
@Transaction(rollbackFor = {IOException.class}) //遇到IO异常也回滚
```

事务传播行为：事务协调员对事务管理员所携带事务的处理态度

![image-20220924095553658](C:\Users\98110\AppData\Roaming\Typora\typora-user-images\image-20220924095553658.png)

# SpringMVC

SpringMVC技术与Servlet技术功能等同，均属于web层开发技术，是一种web框架层技术

## SpringMVC入门案例

- 导入SpringMVC(spring-webmvc)和Servlet坐标

- 创建SpringMVC控制器类（等同于Servlet功能）

  ```java
  @Controller
  @RequestMapping("/user")
  public class UserController{
     @RequestMapping("/save")//访问路径
     @ResponseBody//将java对象返回值转为json格式的数据
     public String save(){
        System.out.println("user save");
        return "{'info':'springmvc'}";
     }
  }
  ```

- 初始化SPringMVC环境(同Spring环境)，设定SpringMVC加载对应的bean

  ```java
  @Configuration
  @ComponentScan("com.zxp.controller")
  public class SpringMvcConfig{}
  ```

- 初始化Servlet容器，加载SpringMVC环境，并设置SpringMVC技术处理的请求

  ```java
  public class ServletContainersInitConfig extends AbstractDispatcherServletInitializer {
      /**
       * 加载SpringMVC配置
       * @return
       */
      @Override
      protected WebApplicationContext createServletApplicationContext() {
          AnnotationConfigWebApplicationContext ctx = new AnnotationConfigWebApplicationContext();
          ctx.register(SpringMVCConfig.class);
          return ctx;
      }
      /**
       * 设置哪些请求归SpringMVC处理
       * @return
       */
      @Override
      protected String[] getServletMappings() {
          return new String[]{"/"};     //所有请求归SpringMVC处理
      }
      /**
       * 加载Spring容器配置
       * @return
       */
      @Override
      protected WebApplicationContext createRootApplicationContext() {
                  AnnotationConfigWebApplicationContext ctx = new AnnotationConfigWebApplicationContext();
          ctx.register(SpringConfig.class);
          return ctx;
      }
  }
  ```

Spring配置文件如何避免扫描Controller:

- 采用数组形式：

  ```java
  @Configuration
  @ComponentScan({"com.zxp.service","com.zxp.dao"})
  public class SpringConfig {}
  ```

- 排除

  ```java
  @Configuration
  @ComponentScan(value="com.zxp",excludeFilters = @ComponentScan.Filter(
       type = FilterType.ANNOTATION, //排除种类
       classes = Controller.class
  ))
  public class SpringConfig {}
  ```

简化开发Servlet容器

```java
public class ServletContainersInitConfig extends AbstractAnnotationConfigDispatcherServletInitializer {
    @Override
    protected Class<?>[] getRootConfigClasses() {
        return new Class[]{SpringConfig.class};
    }
    @Override
    protected Class<?>[] getServletConfigClasses() {
        return new Class[]{SpringMVCConfig.class};
    }
    @Override
    protected String[] getServletMappings() {
        return new String[]{"/"};
    }
}
```

## Get和Post请求

**PostMan**是一款功能强大的网页调试与发送网页Http请求的Chrome插件

作用：常用于接口测试

**Get请求发普通参数**

```java
@Controller
@RequestMapping("/user")
public class UserController{
   @RequestMapping("/save")//访问路径
   @ResponseBody//把返回的信息整体作为内容给到外边
   public String save(String name，int age){ //接受的参数,不区分get和post
      return "{'info':'springmvc'}";
   }
}
```

中文乱码处理：在ServletContainersInitConfig设置过滤器

```java
@Override
protected Filter[] getServletFilters(){
    CharacterEncodingFilter filter = new CharacterEncodingFilter();
    filter.setEncoding("UTF-8");
    return new Filter[]{filter};
}
```

映射

```java
public String save(@RequestParam("name")String name，int age){ //接受的参数
```

```java
public String save(User user){ //User包含name和age属性
```

集合

```java
public String save(@RequestParam List<String> likes)
```



@RequestParam(value=”参数名”,required=”true/false”,defaultValue=””)

1. value：参数名
2. required：是否包含该参数，默认为true，表示该请求路径中必须包含该参数，如果不包含就报错。
3. defaultValue：默认参数值，如果设置了该值，required=true将失效，自动为false,如果没有传该参数，就使用默认值



获取url中的参数

- @Pathariable：/dish/1587357731009540098

  ```java
  @GetMapping("/{id}")
  public R<DishDto> get(@PathVariable Long id) {//@PathVariable：把请求路径占位符中的id解析出来
  ```

- @RequestParam：/dish/status/0?ids=1413342036832100354,1397862198033297410

  ```java
  @PostMapping("/status/{status}")
  public R<String> updateStatus(@PathVariable Integer status, @RequestParam List<Long> ids) {
  ```

- 根据名字直接获取：/dish/page?page=1&pageSize=10&name=米饭

  ```java
  @GetMapping("/page")///dish/page?page=1&pageSize=10&name=米饭
  public R<IPage> page(int page, int pageSize, String name) {
  ```

- 封装成对象获取：setmeal/list/categoryId=145451513312&status=1

  ```java
  @GetMapping("/list")//setmeal/list?categoryId=1587734123836489730&status=1
  public R<List<Setmeal>> list(Setmeal setmeal){//setmeal包含categoryId和status属性
  ```

- json格式数据

  - @RequestBody Dish dish：直接转成对应的对象，注意json的所有字段必须都能在对象的属性中找到，否则接收不到数据

  - @RequestBody Dto dto：json的字段比对应的字段多时，可以使用DTO添加字段使得属性和字段名对应

  - @RequestBody Map map：把所有的json数据以键值对的形式储存

    json：{phone: "13777777777", code: "8678"}

    map：“phone" - > "13777777777"，"code" -> "8678"



json数据传递

- 导入json坐标

- Body选择raw，再选择json

- 在SpringMvcConfig中加入@EnableWebMvc：开启由json数据转换为对象功能

- ```java
  public String save(@RequestBody User user)
  ```

@RequestParam：用于接受url地址传参，表单传参

@RequestBody：主要用来接收前端传递给后端的json字符串中的数据

**日期类型参数传递：**

```java
@RequestMapping("/dataParam")
@ResponseBody
public String dataParam(Date date,@DateTimeFormat(patter="yyyy-MM-dd") Date date1){
    System.out.println(date);
    System.out.println(date1(yyyy-MM-dd));
    return "{'module':'data param'}"
}
```

**响应页面**

```java
@RequestMapping("/toJumpPage")
public String toJumpPage(){
    return "page.jsp"; //要跳转的页面}
```

**响应文本数据**

```java
@RequestMapping("/toText")
@ResponseBody
public String toText(){
    return "response text"; }
```

**响应pojo对象**

```java
@RequestMapping("/toJsonPOJO")
@ResponseBody
public String toJsonPOJO(){
 User user = new user();
 user.setName("zxp");
 user.setAge(15);
 return user;}
```

## REST

REST优点：隐藏资源的访问行为，无法通过地址得知资源是何种操作，简化书写

REST风格：按照访问资源时使用行为动作区分对资源进行了何种操作

- http://localhost/users       查询全部用户信息 GET
- http://localhost/users/参数  查询指定用户信息 GET
- http://localhost/users       添加用户信息     POST
- http://localhost/users       修改用户信息     PUT
- http://localhost/users/参数  删除用户信息     DELETE

根据REST风格对资源进行访问称为RESTful

例子

```java
   @RequestMapping(value = "/users/{id}",method = RequestMethod.DELETE)
   @ResponseBody
   public String delete(@PathVariable Integer id){
      return "{'module':'user delete'}";}}
```

@PathVariable：映射URL绑定的占位符

@RestController = @Controller + @ResponseBody

@DELETEMapping("/{id}") = @RequestMapping(value  = "/{id}",method = RequestMethod.DELETE)

绑定页面时需要定义SpringMvcSupport

## 异常

**异常现象**：所有异常均抛出到表现层进行处理（AOP）

- 框架内部抛出的异常：因使用不合规导致
- 数据层抛出的异常：因外部服务器故障导致
- 业务层抛出的异常：因业务逻辑书写错误导致
- 表消除抛出的异常：因数据收集，校验等规则导致
- 工具类抛出的异常：因工具类书写不够严谨不够健壮导致

```java
//@ControllerAdvice是为@ExceptionHandler修饰的方法所在的类提供的
@ControllerAdvice(annotations = {RestController.class, Controller.class})
//声明拦截的类，就是说加了这些注解的类会被拦截并处理
public class ProjectExceptionAdvice {
    //@ExceptionHandler用于捕获声明拦截的类中抛出的不同类型的异常，从而达到异常全局处理的目的
    @ExceptionHandler(Exception.class)//设置异常类型
    public void doException(Exception ex){
        System.out.println("");}}
```

异常分类：

- 业务异常：
  - 规范的用户行为产生的异常
  - 不规范的用户行为产生的异常
- 系统异常：项目运行过程中可预计且无法避免的异常
- 其他异常：编程人员未预期到的异常

axios发送get请求：

```html
axios.get("/ssm/books").then((res)=>{  
});
```

axios发送post请求：

```html
axios.post("/ssm/books",数据).then((res) =>{
});
```

## 拦截器

拦截器(Interceptor)是一种动态拦截方法调用的机制，在SpringMVC中动态拦截控制器方法的执行

作用：

- 在指定的方法调用前后执行预先设定的代码
- 阻止原始方法的执行

拦截器与过滤器的区别

- 归属不同：Filter输入Servlet技术,Interceptor属于SpringMVC技术
- 拦截内容不同：Filter对所有访问进行增强，Interceptor仅针对SpringMVC的访问增强

在表现层定义拦截器：

```java
@Component
public class ProjectInterceptor implements HandlerInterceptor {
    //alt + insert覆盖前三个方法
    //被拦截之前
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("preHandle");
        return true;//改为false为终止操作，后面两方法不执行
    }
    //拦截之后
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("postHandel");
    }
    //拦截和post之后
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("afterCompletion");
    }
}
在SpringMvcSupport中重写addInterceptors方法
@Override
protected void addInterceptors(InterceptorRegistry registry) {
  registry.addInterceptor(projectInterceptor).addPathPatterns("/books");//拦截路径
}
```

# Maven高级

Maven分模块开发：

1. 在主程序导入分模块的坐标
2. 把分模块用Maven指令进行install

## 依赖

可选依赖：在坐标中添加<optional>true<optional>可以隐藏使用的依赖，将不具有传递性

排除依赖：再导入别的模块坐标时，添加上<exclusions>排除依赖

```xml
<exclusions>
    <exclusion>
        <groupId>log4j</groupId>
        <artifactId>log4j</artifactId>
    </exclusion>
</exclusions>
```

## 聚合

聚合：将多个模块组织成一个整体，同时进行项目构建的过程

聚合工程：通常是一个不具有业务功能的"空"工程（有且仅有一个pom文件）

作用：使用聚合工程可以将多个工程编组，通过对聚合工程进行构建，实现对所包含的模块进行同步构建，当工程中某个模块发生更新（变更）时，必须保障工程中与已更新模块关联的模块同步更新，此时可以使用聚合工程来解决批量模块同步构建的问题。

打包方式

- 默认打包方式：jar
- web打包方式：war
- 聚合工程打包方式：pom

聚合工程pom文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.zxp</groupId>
    <artifactId>maven_01_parent</artifactId>
    <version>1.0-RELEASE</version>
    <packaging>pom</packaging>
    
    <!--设置管理的模块名称-->
    <modules>
        <module>../maven_02_ssm</module>
        <module>../maven_03_pojo</module>
        <module>../maven_04_dao</module>
    </modules>
</project>
```

## 继承

继承：常见于依赖关系的继承。

作用：

- 简化配置
- 减少版本冲突

继承步骤：

- 创建一个空的Maven项目并将其打包方式设置为pom（可以和聚合放在一起）

- 在父工程中定义需要的依赖坐标

- 在子项目中设置其父工程

  ```xml
  <!--配置当前工程继承自parent工程-->
  <parent>
      <groupId>com.zxp</groupId>
      <artifactId>maven_01_parent</artifactId>
      <version>1.0-RELEASE</version>
      <!--设置父项目pom.xml位置路径-->
      <relativePath>../maven_01_parent/pom.xml</relativePath>
  </parent>
  ```

- 在父工程定义依赖管理：子项目可以选择性继承

  ```xml
  <!--定义依赖管理-->
  <dependencyManagement>
      <dependencies>
          <dependency>
              <groupId>junit</groupId>
              <artifactId>junit</artifactId>
              <version>4.12</version>
              <scope>test</scope>
          </dependency>
      </dependencies>
  </dependencyManagement>
  ```

- 子项目的添加junit的依赖

  ```xml
  <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <scope>test</scope>
  </dependency>
  ```

## 属性

- 父工程中定义属性

  ```xml
  <properties>
      <spring.version>5.2.10.RELEASE</spring.version>
      <junit.version>4.12</junit.version>
      <mybatis-spring.version>1.3.0</mybatis-spring.version>
  </properties>
  ```

- 修改依赖的version

  ```xml
  <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-core</artifactId>
      <version>${spring.version}</version>
  </dependency>
  ```

**配置文件加载属性**

- 父工程定义属性

  ```xml
  <properties>
     <jdbc.url>jdbc:mysql://127.1.1.1:3306/ssm_db</jdbc.url>
  </properties>
  ```

- jdbc.properties文件中引用属性

  ```properties
  jdbc.driver=com.mysql.jdbc.Driver
  jdbc.url=${jdbc.url}
  jdbc.username=root
  jdbc.password=root
  ```

- 设置maven过滤文件范围

  ```xml
  <build>
      <resources>
          <!--设置资源目录-->
          <resource>
              <directory>../maven_02_ssm/src/main/resources</directory>
              <!--设置能够解析${}，默认是false -->
              <filtering>true</filtering>
          </resource>
      </resources>
  </build>
  ```

- 配置多个项目

  ```xml
  <build>
      <resources>
          <!--
  			${project.basedir}: 当前项目所在目录,子项目继承了父项目，
  			相当于所有的子项目都添加了资源目录的过滤
  		-->
          <resource>
              <directory>${project.basedir}/src/main/resources</directory>
              <filtering>true</filtering>
          </resource>
      </resources>
  </build>
  ```

**../**：返回文件的上一级

打包的过程中如果报如下错误:Error assembling WAR

原因：Maven发现你的项目为web项目，就会去找web项目的入口web.xml[配置文件配置的方式]，发现没有找到，就会报错。

解决方案: 配置maven打包war时，忽略web.xml检查

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-war-plugin</artifactId>
            <version>3.2.3</version>
            <configuration>
                <failOnMissingWebXml>false</failOnMissingWebXml>
            </configuration>
        </plugin>
    </plugins>
</build>
```

**版本管理**

- SNAPSHOT（快照版本）
  - 项目开发过程中临时输出的版本，称为快照版本
  - 快照版本会随着开发的进展不断更新
- RELEASE（发布版本）
  - 项目开发到一定阶段里程碑后，向团队外部发布较为稳定的版本，这种版本所对应的构件文件是稳定的
  - 即便进行功能的后续开发，也不会改变当前发布版本内容，这种版本称为发布版本

## 多环境开发

配置多环境

```xml
<profiles>
    <!--开发环境-->
    <profile>
        <id>dev</id>
        <properties>
            <jdbc.url>jdbc:mysql://127.1.1.1:3306/ssm_db</jdbc.url>
        </properties>
        <!--设定是否为默认启动环境-->
        <activation>
            <activeByDefault>true</activeByDefault>
        </activation>
    </profile>
    <!--生产环境-->
    <profile>
        <id>pro</id>
        <properties>
            <jdbc.url>jdbc:mysql://127.2.2.2:3306/ssm_db</jdbc.url>
        </properties>
    </profile>
    <!--测试环境-->
    <profile>
        <id>test</id>
        <properties>
            <jdbc.url>jdbc:mysql://127.3.3.3:3306/ssm_db</jdbc.url>
        </properties>
    </profile>
</profiles>
```

**命令行实现环境切换**

mvn  指令(instal) -P 环境id(pro)

**跳过测试**

- 方式一:IDEA工具实现跳过测试，Maven右上角闪电按钮跳过所有测试

- 方式二：在父工程中的pom.xml中添加测试插件配置

  ```xml
  <build>
      <plugins>
          <plugin>
              <!--测试插件名称，可以在执行Maven时在控制台找到-->
            <artifactId>maven-surefire-plugin</artifactId>
            <version>2.12.4</version>
            <configuration>
              <skipTests>false</skipTests>//true为跳过所有测试，false时根据规则跳过
                  <!--排除掉不参与测试的内容--><includes>为包含
                  <excludes>//排除的测试
                      <exclude>**/BookServiceTest.java</exclude>
                  </excludes>
              </configuration>
          </plugin>
      </plugins>
  </build>
  ```

- 方式三：使用Maven的命令行，`mvn 指令(例如install) -D skipTests`

  - 该命令可以不借助IDEA，直接使用cmd命令行进行跳过测试，需要注意的是cmd要在pom.xml所在目录下进行执行。

## 私服

- 启动Nexus：使用cmd进入到解压目录下的`nexus-3.30.1-01\bin`，输入nexus.exe /run nexus
- 浏览器访问：http://localhost:8081
- 账号：admin 密码：admin

| 仓库类别 | 英文名称 |          功能           | 关联操作 |
| :------: | :------: | :---------------------: | :------: |
| 宿主仓库 |  hosted  | 保存自主研发+第三方资源 |   上传   |
| 代理仓库 |  proxy   |    代理连接中央仓库     |   下载   |
|  仓库组  |  group   | 为仓库编组简化下载操作  |   下载   |

**本地仓库访问私服配置**

- 私服上配置仓库
  1. 点击设置
  2. 点击仓库
  3. 新建仓库
  4. 选择maven2(hosted)
  5. 创建release（发布版本）和snapshot（临时版本）
  6. 将新建的两个仓库添加到仓库组中

**配置本地Maven对私服的访问权限**

```xml
<servers>
    <server>
        <id>itheima-snapshot</id>
        <username>admin</username>
        <password>admin</password>
    </server>
    <server>
        <id>itheima-release</id>
        <username>admin</username>
        <password>admin</password>
    </server>
</servers>
```

**配置私服访问路径**

```xml
<mirrors>
    <mirror>
        <!--配置仓库组的ID-->
        <id>maven-public</id>
        <!--*代表所有内容都从私服获取-->
        <mirrorOf>*</mirrorOf>
        <!--私服仓库组maven-public的访问路径-->
        <url>http://localhost:8081/repository/maven-public/</url>
    </mirror>
</mirrors>
```

**配置工程上传私服的具体位置**

```xml
 <!--配置当前工程保存在私服中的具体位置-->
<distributionManagement>
    <repository>
        <!--使用该id对应的用户名和密码-->
        <id>zxp-release</id>
         <!--release版本上传仓库的具体地址-->
        <url>http://localhost:8081/repository/zxp-release/</url>
    </repository>
    <snapshotRepository>
        <!--id对应的用户名和密码-->
        <id>zxp-snapshot</id>
        <!--snapshot版本上传仓库的具体地址-->
        <url>http://localhost:8081/repository/zxp-snapshot/</url>
    </snapshotRepository>
</distributionManagement>
```

**发布资源到私服**：Maven选择deploy命令

私服使用的不是Tomact服务器而是用的jetty

Jetty比Tomacat更轻量级，可扩展性更强

# SpringBoot

开发步骤：

- 创建新模块，选择Spring初始化，并配置模块相关基础信息
- 选择当前模块需要使用的技术集（如SpringWeb）
- 开发控制器类等
- 运行自动生成的Application类

**以后需要使用技术，只需要引入该技术对应的起步依赖即可**(starter标志)

配置文件格式:

- 修改`application.properties`文件：server.port=80

- 添加application.yml文件：

  ```yaml
   server:
   port: 80（80前有空格）
  ```

- 添加application.yaml文件：

  ```yaml
   server:
   port: 80（80前有空格）
  ```

**spring-boot-maven-plugin报红**是因为确实SpringBoot的版本号

```xml
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <version>2.7.4</version>
```

## yaml(多环境)

**yaml语法规则**(.yml和.yaml都属于yaml)

* 大小写敏感

* 属性层级关系使用多行描述，每行结尾使用冒号结束

* 使用缩进表示层级关系，同层级左侧对齐，只允许使用空格（不允许使用Tab键）

  空格的个数并不重要，只要保证同层级的左侧对齐即可。

* 属性值前面添加空格（属性名与属性值之间使用冒号+空格作为分隔）

* \# 表示注释

核心规则：数据前面要加空格与冒号隔开

数组数据使用减号作为数据开始符号，每行书写一个数据，减号与数据间空格分隔

```yaml
enterprise:
  name: itcast
  age: 16
  tel: 4006184000
  subject:
    - Java
    - 前端
    - 大数据
```

读取yaml文件数据（以上边数据为例）：

- 使用 `@Value("表达式")` 注解可以读取数据，引用方式：`${一级属性名.二级属性名……}`

  ```java
      @Value("${lesson}")
      private String lesson;
      @Value("${server.port}")
      private Integer port;
      @Value("${enterprise.subject[0]}")
      private String subject_00;
  ```

- Environment对象:将配置文件中所有的数据封装到 `Environment` 对象中,通过调用 `Environment` 对象的 `getProperty(String name)` 方法获取数据

  ```java
   @Autowired
      private Environment env;
     
      @GetMapping("/{id}")
      public String getById(@PathVariable Integer id){
          System.out.println(env.getProperty("lesson"));
          System.out.println(env.getProperty("enterprise.name"));
          System.out.println(env.getProperty("enterprise.subject[0]"));
          return "hello , spring boot!";
      }
  ```

- 封装到自定义的实体类对象中

  - 在类上添加 `@Component` 注解,将实体类 `bean` 的创建交给 `Spring` 管理
  - 使用 `@ConfigurationProperties` 注解表示加载配置文件,可以使用 `prefix` 属性指定只加载指定前缀的数据

  - 在 `BookController` 中进行注入

  ```java
  @Component
  @ConfigurationProperties(prefix = "enterprise")
  public class Enterprise { 
      private String name;
      private int age;
      private String tel;
      private String[] subject;}
  ```

  ```java
      @Autowired
      private Enterprise enterprise;
  ```

**yaml文件中的数据引用**

```yaml
baseDir: /usr/local/fire

dataDir: ${baseDir}/data
```

**多环境配置**:application.yml配置文件

```yaml
#设置启用的环境
spring:
  profiles:
    active: dev

---
#开发
spring:
  config:
    activate:
      on-profile: dev
server:
  port: 80
---
#生产
spring:
  config:
    activate:
      on-profile: pro
server:
  port: 81
---
#测试
spring:
  config:
    activate:
      on-profile: test
server:
  port: 82
---
```

**多环境开发多文件版**

主启动配置文件application.yml

```yaml
spring:
  profiles:
    active:  dev
```

环境分类配置文件application-pro.yml

```yaml
server:
 port: 80
```

环境分类配置文件application-dev.yml

```yaml
server:
 port: 81
```

环境分类配置文件application-test.yml

```yaml
server:
 port: 80
```

多环境开发分组管理：

```yaml
spring:
  profiles:
    active: dev
    group: 
    "dev": decxx,devxy
    "pro": proxx,proxy
    "test": testxx,testxy
```

**命令行启动参数设置**

使用 `SpringBoot` 开发的程序以后都是打成 `jar` 包，先使用Maven进行Clean再进行Package，在打包目录（target文件）下打开cmd，通过 `java -jar xxx.jar` 的方式启动服务

```shell
java -jar xxx.jar
```

`SpringBoot` 提供了在运行 `jar` 时设置开启指定的环境的方式

```shell
java –jar xxx.jar --spring.profiles.active=test
```

修改临时端口号：

```shell
java –jar xxx.jar –-server.port=0611
```

同时设置多个配置，比如即指定启用哪个环境配置，又临时指定端口

```shell
java –jar springboot.jar –-server.port=88 –-spring.profiles.active=test
```

**Maven与SpringBoot开发环境兼容性问题**（Maven主导）

```yaml
#设置启用的环境
spring:
  profiles:
    active: ${profile.active}
```

pom文件中：

```xml
<profiles>
    <!--开发环境-->
    <profile>
        <id>dev</id>
        <properties>
        <profile.active>dev</profile.active>
        </properties>
        <!--设定是否为默认启动环境-->
        <activation>
            <activeByDefault>true</activeByDefault>
        </activation>
    </profile>
    <!--生产环境-->
    <profile>
        <id>pro</id>
        <properties>
            <profile.active>pro</profile.active>
        </properties>
    </profile>
    <!--测试环境-->
    <profile>
        <id>test</id>
        <properties>
            <profile.active>test</profile.active>
        </properties>
    </profile>
```

pom文件中需要添加插件才能被yaml文件引用

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-resources-plugin</artifactId>
    <version>3.2.0</version>
    <configuration>
        <encoding>UTF-8</encoding>
        <useDefaultDelimiters>true</useDefaultDelimiters>
    </configuration>
</plugin>
```

**配置文件优先级：**

`SpringBoot` 中配置文件放置位置：级别越高优先级越高

* 1级：classpath：application.yml
* 2级：classpath：config/application.yml
* 3级：本地file ：application.yml
* 4级：本地file ：config/application.yml

## SpringBoot整合Junit

- 创建Spring Initializr模块，不选择依赖

- 在AoolicationTests里进行测试

  ```java
  @SpringBootTest
  class Springboot07TestApplicationTests {
  
      @Autowired
      private BookService bookService;
  
      @Test
      public void save() {
          bookService.save();
      }
  }
  ```

如果不是在默认目录下要加上

```java
@SpringBootTest(classes = xxxApplication.class)//初始生成的类
```

或者添加注解(不经常用)

```java
@SpringBootTest
@ContextConfiguration(classes = xxxApplication.class)
class Springboot07TestApplicationTests {}
```



## SpringBoot整合Mybatis

1. 创建Spring Initializr模块，在SQL中选择Mybatis和Mysql依赖

2. 把application的后缀从properties改为yml

  ```yaml
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/ssm
    username: root
    password: 1234
mybatis:
  type-aliases-package: com.zxp.user.pojo #指定返回值类型所在包
  configuration:
    map-underscore-to-camel-case: true 
    #开启驼峰功能:在映射实体或者属性时，将数据库中表名和字段名中的下划线去掉，按照驼峰命名法映射
    #例如:address_book—> addressBook
  ```

3. 定义实体类：Book

4. 定义dao接口

  ```java
package com.zxp.dao;
import com.zxp.domain.Book;
import org.apache.ibatis.annotations.Mapper;
import org.apache.ibatis.annotations.Select;
@Mapper
public interface BookDao {
    @Select("select * from tbl_book where id = #{id}")
    public Book getById(Integer id);
}
  ```

Mybatis会扫描接口并创建接口的代码对象交给 Spring 管理，但是现在并没有告诉 Mybatis哪个是 `dao` 接口，需要在BookDao接口上使用 @Mapper

@Mapper是mybatis自身带的注解。在spring程序中，mybatis需要找到对应的mapper，在编译时生成动态代理类，与数据库进行交互，这时需要用到@Mapper注解

但是有时候当我们有很多mapper接口时，就需要写很多@Mappe注解，这样很麻烦，有一种简便的配置化方法便是在启动类上使用@MapperScan注解。

```java
@MapperScan("com.zxp.mapper")
public class TtApplication {
    public static void main(String[] args) {
        SpringApplication.run(TtApplication.class, args);
        log.info("项目启动成功");
    }
```

5. 定义测试类

6. 使用druid数据源

- 导入 `Druid` 依赖

  ```xml
  <dependency>
      <groupId>com.alibaba</groupId>
      <artifactId>druid</artifactId>
      <version>1.1.16</version>
  </dependency>
  ```

- 在 `application.yml` 配置文件配置

  ```yaml
  spring:
    datasource:
      driver-class-name: com.mysql.cj.jdbc.Driver
      url: jdbc:mysql://localhost:3306/ssm_db?serverTimezone=UTC
      username: root
      password: root
      type: com.alibaba.druid.pool.DruidDataSource
  ```

SpringBoot低版本The server time zone错误：加上时区

```yaml
url: jdbc:mysql://localhost:3306/ssm_db?serverTimezone=UTC
```

**在映射实体或者属性时，将数据库中表名和字段名中的下划线去掉，按照驼峰命名法映射**

例如:address_book—> addressBook

```yaml
mybatis:
  configuration:
    map-underscore-to-camel-case: true
```

# MyBatisPlus

MybatisPlus(简称MP)是基于MyBatis框架基础上开发的增强型工具，旨在简化开发、提供效率。

**在映射实体或者属性时，将数据库中表名和字段名中的下划线去掉，按照驼峰命名法映射**

例如:address_book—> addressBook

```yaml
mybatis-plus:
  configuration:
    map-underscore-to-camel-case: true
```

**入门案例**：

1. 创建Spring Initializr模块，在SQL中选择Mysql依赖

2. 导入mybatis-plus-boot-starter和druid坐标

3. 定义user类和UserDao类(map)

   ```java
   package com.zxp.dao;
   
   import com.baomidou.mybatisplus.core.mapper.BaseMapper;
   import com.zxp.domain.User;
   import org.apache.ibatis.annotations.Mapper;
   
   @Mapper
   public interface UserDao extends BaseMapper<User> {
   }
   ```

4. 定义service

   ```java
   @Service
   public class UserServiceImpl extends ServiceImpl<UserMapper, User> implements UserService{
   }
   ```

   ```java
   public interface EmployeeService extends IService<Employee> {
   }
   ```

5. 测试

   ```java
   @SpringBootTest
   class MybatisPlusApplicationTests {
   
       @Autowired
       private UserDao userDao;
   
       @Test
       void testGetAll() {
           User user = userDao.selectById(1);
           System.out.println(user);
       }
   
   }
   ```

MP的特性:

- 无侵入：只做增强不做改变，不会对现有工程产生影响
- 强大的 CRUD 操作：内置通用 Mapper，少量配置即可实现单表CRUD 操作
- 支持 Lambda：编写查询条件无需担心字段写错
- 支持主键自动生成
- 内置分页插件

## 增删改查

[ Service CRUD 接口](https://baomidou.com/pages/49cc81/#service-crud-%E6%8E%A5%E5%8F%A3)

[Mapper CRUD 接口](https://baomidou.com/pages/49cc81/#mapper-crud-%E6%8E%A5%E5%8F%A3)

- 新增:int insert (T t)//新增成功后返回1，失败返回0

  ```java
      @Test
      void testSave(){
          User user = new User();
          user.setName("a");
          userDao.insert(user);
      }
  ```

- 删除:int deleteById (Serializable id)//Serializable：参数类型(String等的父类)

  ```java
      @Test
      void testDelete(){
          userDao.deleteById(3);
      }
  ```

- 修改:int updateById(T t);

  ```java
  @Test
      void testUpdate(){
          User user = new User();
          user.setId(1L);//L表示这是一个long类型的数字
          user.setName("b");
          userDao.updateById(user);
      }
  ```

- 根据ID查询:T selectById (Serializable id)

  ```java
      @Test
      void testGetById() {
          User user = userDao.selectById(1);
          System.out.println(user);
      }
  ```

- 查询所有:List<T> selectList(Wrapper<T> queryWrapper)
  //Wrapper：用来构建条件查询的条件，目前我们没有可直接传为Null

  ```java
      @Test
      void testGetAll(){
          List<User> userList = userDao.selectList(null);
          System.out.println(userList);
      }
  ```

- 分页查询

  ```java
      @Test
      void testGetByPage(){
          IPage page = new Page(1,2);//第一个参数为当前页码，第二个参数为每页显示的条数
          userDao.selectPage(page,null);
          System.out.println("当前页码：" + page.getCurrent());
          System.out.println("每页显示条数：" + page.getSize());
          System.out.println("总页数" + page.getPages());
          System.out.println("总条数" + page.getTotal());
          System.out.println("数据" + page.getRecords());
      }
  ```

  需要在配置文件MpConfig中添加分页拦截器才能正常运行

  ```java
  @Configuration
  public class MpConfig {
      @Bean
      public MybatisPlusInterceptor mpInterceptor(){
          //定义Mp拦截器
          MybatisPlusInterceptor mpInterceptor = new MybatisPlusInterceptor();
          //添加具体拦截器
          mpInterceptor.addInnerInterceptor(new PaginationInnerInterceptor());
          return mpInterceptor;
      }
  }
  ```

```java
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
```

## Lombok

Lombok：一个Java类库，提供了一组注解，简化POJO实体类开发

添加lombok依赖

```xml
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <!--<version>1.18.12</version>-->
</dependency>
```

Lombok常见的注解有:

* @Setter:为模型类的属性提供setter方法
* @Getter:为模型类的属性提供getter方法
* @ToString:为模型类的属性提供toString方法
* @EqualsAndHashCode:为模型类的属性提供equals和hashcode方法
* ==@Data:是个组合注解，包含上面的注解的功能==
* ==@NoArgsConstructor:提供一个无参构造函数==
* ==@AllArgsConstructor:提供一个包含所有参数的构造函数==

**开启MP日志**：在application.yml中添加

```yaml
#开启mp的日志（输出到控制台）
mybatis-plus:
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
```

条件查询三种格式：

```java
    @Test
    void testGetAll(){
        QueryWrapper qw = new QueryWrapper();
        qw.lt("age",18);//lt：小于
        List<User> userList = userDao.selectList(qw);
        System.out.println(userList);
    }

    //lambda格式按条件查询
    @Test
    void testGetAll(){
        QueryWrapper<User> qw = new QueryWrapper<User>();
        //由于直接写字段名容易写错，因此采用直接获取实体类字段名的方式
        qw.lambda().lt(User::getAge, 10);//添加条件
        List<User> userList = userDao.selectList(qw);
        System.out.println(userList);
    }

    @Test
    void testGetAll(){
        LambdaQueryWrapper<User> lqw = new LambdaQueryWrapper<User>();
        lqw.lt(User::getAge, 10);
        List<User> userList = userDao.selectList(lqw);
        System.out.println(userList);
    }
```

## 多条件查询

```java
    @Test
    void testGetAll(){
        LambdaQueryWrapper<User> lqw = new LambdaQueryWrapper<User>();
        //小于30且大于10
        lqw.lt(User::getAge, 30).lqw.gt(User::getAge, 10);
        //小于10或者大于30
        lqw.lt(User::getAge, 10).or().gt(User::getAge, 30);
        List<User> userList = userDao.selectList(lqw);
        System.out.println(userList);
    }
```

## null判定

![1631024145264](D:\学习\SSM\资料\基础框架8笔记\Mybatisplus笔记\assets\1631024145264.png)

后台如果想接收前端的两个数据，该如何接收?

新建一个模型类,让其继承User类，并在其中添加age2属性

```java
@Data//Lombok注解
public class UserQuery extends User {
    private Integer age2;
}
```

进行null判定

```java
@SpringBootTest
class Mybatisplus02DqlApplicationTests {

    @Autowired
    private UserDao userDao;
    
    @Test
    void testGetAll(){
        //模拟页面传递过来的查询数据
        UserQuery uq = new UserQuery();
        uq.setAge(10);
        uq.setAge2(30);
        LambdaQueryWrapper<User> lqw = new LambdaQueryWrapper<User>();
        lqw.lt(null!=uq.getAge2(),User::getAge, uq.getAge2());
        lqw.gt(null!=uq.getAge(),User::getAge, uq.getAge());
        List<User> userList = userDao.selectList(lqw);
        System.out.println(userList);
    }
}
```

## 查询投影

**查询指定字段**

```java
    @Test
    void testGetAll(){
        LambdaQueryWrapper<User> lqw = new LambdaQueryWrapper<User>();
        //只查询Id和Name
        //由于直接写字段名容易写错，因此采用直接获取实体类字段名的方式,User::getId相当于"id"
        lqw.select(User::getId,User::getName);
        List<User> userList = userDao.selectList(lqw);
        System.out.println(userList);}
    
    //不使用lambda表达式        
    @Test
    void testGetAll(){
        QueryWrapper<User> lqw = new QueryWrapper<User>();
        lqw.select("id","name","age","tel");
        List<User> userList = userDao.selectList(lqw);
        System.out.println(userList);}
```

**聚合查询**

- count:总记录数

- max:最大值

- min:最小值

- avg:平均值

- sum:求和

  ```java
      @Test
      void testGetAll(){
          QueryWrapper<User> lqw = new QueryWrapper<User>();
          //查询总数
          lqw.select("count(*) as count");
          //求年龄最大值
          lqw.select("max(age) as maxAge");
          //求年龄总和
          lqw.select("sum(age) as sumAge");
          //求年龄平均值
          lqw.select("avg(age) as avgAge");
          
          List<Map<String, Object>> userMap = userDao.selectMaps(lqw);
          System.out.println(userList);
      }
  ```

**分组查询**

```java
    @Test
    void testGetAll(){
        QueryWrapper<User> lqw = new QueryWrapper<User>();
        lqw.select("count(*) as count,tel");
        lqw.groupBy("tel");
        List<Map<String, Object>> list = userDao.selectMaps(lqw);
        System.out.println(list);
    }
```

## 查询条件

**查询条件**

```java
    @Test
    void testGetAll(){
        LambdaQueryWrapper<User> lqw = new LambdaQueryWrapper<User>();
        //查询名字为jerry密码也为jerry的用户，eq()相当于"="
        lqw.eq(User::getName, "Jerry").eq(User::getPassword, "jerry");
        User loginUser = userDao.selectOne(lqw);//查询一个
        System.out.println(loginUser);
    }
```

**范围查询**:

* gt():大于(>)
* ge():大于等于(>=)
* lt():小于(<)
* lte():小于等于(<=)
* between():between ? and ?

```java
    @Test
    void testGetAll(){
        LambdaQueryWrapper<User> lqw = new LambdaQueryWrapper<User>();
        lqw.between(User::getAge, 10, 30);
        //SELECT id,name,password,age,tel FROM user WHERE (age BETWEEN ? AND ?)
        List<User> userList = userDao.selectList(lqw);
        System.out.println(userList);
    }
```

**模糊查询**

```java 
    @Test
    void testGetAll(){
        LambdaQueryWrapper<User> lqw = new LambdaQueryWrapper<User>();
        lqw.likeLeft(User::getName, "J");
        //SELECT id,name,password,age,tel FROM user WHERE (name LIKE %?)
        List<User> userList = userDao.selectList(lqw);
        System.out.println(userList);
    }
```

## 映射匹配兼容性

- 当表的列名和模型类的属性名发生不一致，就会导致数据封装不到模型对象

  @TableField:使用该注解可以实现模型类属性名和表的列名之间的映射关系

  ![1631030550100](D:\学习\SSM\资料\基础框架8笔记\Mybatisplus笔记\assets\1631030550100.png)

- 编码中添加了数据库未定义的属性

  在未定义的属性上加上@TableField（exist = false）表示数据库里不存在

- 不想让密码被查询出来

  ```java
  public class User{
      @TableField(value="pwd",select = false)
      private String password;
  }
  ```

- 表名与编码开发设计不同

  ```java
  @TableName("tbl_user")
  public class User{
  }
  ```

## id生成策略控制

**不同的表应用不同的id生成策略**

* 日志：自增（1,2,3,4，……）
* 购物订单：特殊规则（FQ23948AK3843）
* 外卖单：关联地区日期等信息（10 04 20200314 34 91）
* 关系表：可省略id

```java
public class User {
    @TableId(type = IdType.AUTO)
    private Long id;}
```

* NONE: 不设置id生成策略

* AUTO：使数据库ID自增

* INPUT：用户手工输入id（user.setId(xxx)）

  这种ID生成策略，需要将表的自增策略删除掉

* ASSIGN_ID：雪花算法生成id(可兼容数值型与字符串型)

  主键的类型是bigInt

* ASSIGN_UUID：以UUID生成算法作为id生成策略

  主键改成varchar(String)类型，长度要大于32

* 其他的几个策略均已过时，都将被ASSIGN_ID和ASSIGN_UUID代替掉。

**所有模型使用相同的id生成策略**

```yaml
mybatis-plus:
  global-config:
    db-config:
    	id-type: assign_id
    	#默认表名前缀
    	table-prefix: tbl_
```

## 多数据操作（删除与查询）

```java
    @Test
    void testDelete(){
        //删除指定多条数据
        List<Integer> list = new ArrayList<>();
        list.add(1);
        list.add(2);
        list.add(3);
        //根据传入的id集合将数据库表中的数据删除掉
        userDao.deleteBatchIds(list);
    }

    @Test
    void testGetByIds(){
        //查询指定多条数据
        List<Integer> list = new ArrayList<>();
        list.add(1);
        list.add(3);
        list.add(4);
        userDao.selectBatchIds(list);
    }
```

## 逻辑删除

逻辑删除:为数据设置是否可用状态字段，删除时设置状态字段为不可用状态，数据保留在数据库中，执行的是update操作

1. 修改数据库表添加`deleted`列（设置默认值为0）

2. 实体类添加属性

   ```java
   public class User {
   @TableLogic(value="0",delval="1")
       //value为正常数据的值，delval为删除数据的值
       private Integer deleted;}
   ```

   或者在配置文件中添加全局配置

   ```yaml
   mybatis-plus:
     global-config:
       db-config:
         # 逻辑删除字段名
         logic-delete-field: deleted
         # 逻辑删除字面值：未删除为0
         logic-not-delete-value: 0
         # 逻辑删除字面值：删除为1
         logic-delete-value: 1
   ```

3. MP的逻辑删除会将所有的查询都添加一个未被删除的条件，已经被删除的数据没有查询出来。

## 乐观锁

业务并发现象带来的问题:==秒杀==：假如有100个商品或者票在出售，为了能保证每个商品或者票只能被一个人购买，如何保证不会出现超买或者重复卖

乐观锁适用于并发量小于2000的情况

1. 列名可以任意，比如使用`version`,给列设置默认值为`1`

2. 在模型类中添加对应的属性

   ```java
   public class User {    
       @Version  
       private Integer version;
   }
   ```

3. 添加乐观锁的拦截器

   ```java
   @Configuration
   public class MpConfig {
       @Bean
       public MybatisPlusInterceptor mpInterceptor() {
           //1.定义Mp拦截器
           MybatisPlusInterceptor mpInterceptor = new MybatisPlusInterceptor();
           //2.添加乐观锁拦截器
           mpInterceptor.addInnerInterceptor(new OptimisticLockerInnerInterceptor());
           return mpInterceptor;
       }
   }
   ```

4. 要想实现乐观锁，首先第一步应该是拿到表中的version，然后拿version当条件将version加1更新回到数据库表中

   两种方法：

   ```java
       @Test
       void testUpdate(){
           User user = new User();
           user.setId(3L);
           user.setName("Jock666");
           user.setVersion(1);
           userDao.updateById(user);
       }
   ```

   ```java
   @Test
   void testUpdate(){
       //1.先通过要修改的数据id将当前数据查询出来
       User user = userDao.selectById(3L);
       //2.将要修改的属性逐一设置进去
       user.setName("Jock888");
       userDao.updateById(user);
   }
   ```

原理：

- 假如第一个线程先执行更新，会把version改为2，

* 第二个线程再更新的时候，set version = 2 where version = 1,此时数据库表的数据version已经为2，所以第二个线程会修改失败
* 不管谁先执行都会确保只能有一个线程更新数据，这就是MP提供的乐观锁的实现原理分析。

```java
    @Test
    void testUpdate(){
       //1.先通过要修改的数据id将当前数据查询出来
        User user = userDao.selectById(3L);     //version=1
        User user2 = userDao.selectById(3L);    //version=1
        user2.setName("Jock aaa");
        userDao.updateById(user2);              //version=2
        user.setName("Jock bbb");
        userDao.updateById(user);               //verion=1条件不成立
    }
```

## 代码生成器

1. 导入对应的jar包

   ```xml
           <!--代码生成器-->
           <dependency>
               <groupId>com.baomidou</groupId>
               <artifactId>mybatis-plus-generator</artifactId>
               <version>3.4.1</version>
           </dependency>
   
           <!--velocity模板引擎-->
           <dependency>
               <groupId>org.apache.velocity</groupId>
               <artifactId>velocity-engine-core</artifactId>
               <version>2.3</version>
           </dependency>
   ```

2. 创建代码生成类

   ```java
   public class CodeGenerator {
       public static void main(String[] args) {
           //1.获取代码生成器的对象
           AutoGenerator autoGenerator = new AutoGenerator();
   
           //设置数据库相关配置
           DataSourceConfig dataSource = new DataSourceConfig();
           dataSource.setDriverName("com.mysql.cj.jdbc.Driver");
           dataSource.setUrl("jdbc:mysql://localhost:3306/ssm");
           dataSource.setUsername("root");
           dataSource.setPassword("1234");
           autoGenerator.setDataSource(dataSource);
   
           //设置全局配置
           GlobalConfig globalConfig = new GlobalConfig();
           //设置代码生成位置       
   globalConfig.setOutputDir(System.getProperty("user.dir")+"/模块名/src/main/java");    
       globalConfig.setOpen(false);    //设置生成完毕后是否打开生成代码所在的目录
       globalConfig.setAuthor("xxx");    //设置作者
       globalConfig.setFileOverride(true);  //设置是否覆盖原始生成的文件
       globalConfig.setMapperName("%sDao"); //设置数据层接口名，%s为占位符，指代模块名称
       globalConfig.setIdType(IdType.ASSIGN_ID);   //设置Id生成策略
       autoGenerator.setGlobalConfig(globalConfig);
   
           //设置包名相关配置
           PackageConfig packageInfo = new PackageConfig();
           packageInfo.setParent("com.zxp");   //设置生成的包名
           packageInfo.setEntity("domain");    //设置实体类包名
           packageInfo.setMapper("dao");   //设置数据层包名
           autoGenerator.setPackageInfo(packageInfo);
   
        //策略设置
        StrategyConfig strategyConfig = new StrategyConfig();
        strategyConfig.setInclude("tbl_user");  //设置当前参与生成的表名，参数为可变参数
        strategyConfig.setTablePrefix("tbl_");  //设置数据库表的前缀名称，tbl_会去掉
           strategyConfig.setRestControllerStyle(true);    //设置是否启用Rest风格
           strategyConfig.setVersionFieldName("version");  //设置乐观锁字段名
           strategyConfig.setLogicDeleteFieldName("deleted");  //设置逻辑删除字段名
           strategyConfig.setEntityLombokModel(true);  //设置是否启用lombok
           autoGenerator.setStrategy(strategyConfig);
           //2.执行生成操作
           autoGenerator.execute();
       }
   }
   ```


## 公共字段自动填充

MybatisPlus公共字段自动填充，就是在插入或者更新的时候为指定字段赋予指定的值，使用它的好处是可以统一对这些字段进行处理，避免了重复代码

实现步骤：

1. 在实体类中的属性上加入@TableField注解，指定自动填充的策略
2. 按照框架要求编写元数据对象处理器，此类需要实现MetaObjectHandler接口