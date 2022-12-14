---
title: MybatisPlus笔记
date: 2022-01-30
tags: MybatisPlus
categories: 笔记
description: MybatisPlus笔记
cover: https://zhuxinpeng.oss-cn-hangzhou.aliyuncs.com/2.jpg
---
# MyBatisPlus

MybatisPlus(简称MP)是基于MyBatis框架基础上开发的增强型工具，旨在简化开发、提供效率。

入门案例：

1. 创建Spring Initializr模块，在SQL中选择Mysql依赖

2. 导入mybatis-plus-boot-starter和druid坐标

3. 定义user类和UserDao类

   ``` java
   package com.zxp.dao;
   
   import com.baomidou.mybatisplus.core.mapper.BaseMapper;
   import com.zxp.domain.User;
   import org.apache.ibatis.annotations.Mapper;
   
   @Mapper
   public interface UserDao extends BaseMapper<User> {
   }
   ```

4. 测试

   ``` java
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

- 新增:int insert (T t)//新增成功后返回1，失败返回0

  ``` java
      @Test
      void testSave(){
          User user = new User();
          user.setName("a");
          userDao.insert(user);
      }
  ```

- 删除:int deleteById (Serializable id)//Serializable：参数类型(String等的父类)

  ``` java
      @Test
      void testDelete(){
          userDao.deleteById(3);
      }
  ```

- 修改:int updateById(T t);

  ``` java
  @Test
      void testUpdate(){
          User user = new User();
          user.setId(1L);//L表示这是一个long类型的数字
          user.setName("b");
          userDao.updateById(user);
      }
  ```

- 根据ID查询:T selectById (Serializable id)

  ``` java
      @Test
      void testGetById() {
          User user = userDao.selectById(1);
          System.out.println(user);
      }
  ```

- 查询所有:List<T> selectList(Wrapper<T> queryWrapper)
  //Wrapper：用来构建条件查询的条件，目前我们没有可直接传为Null

  ``` java
      @Test
      void testGetAll(){
          List<User> userList = userDao.selectList(null);
          System.out.println(userList);
      }
  ```

- 分页查询

  ``` java
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

  ``` java
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

## Lombok

Lombok：一个Java类库，提供了一组注解，简化POJO实体类开发

添加lombok依赖

``` xml
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

``` yaml
#开启mp的日志（输出到控制台）
mybatis-plus:
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
```

条件查询三种格式：

``` java
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

``` java
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

后台如果想接收前端的两个数据，该如何接收?

新建一个模型类,让其继承User类，并在其中添加age2属性

``` java
@Data//Lombok注解
public class UserQuery extends User {
    private Integer age2;
}
```

进行null判定

``` java
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

``` java
    @Test
    void testGetAll(){
        LambdaQueryWrapper<User> lqw = new LambdaQueryWrapper<User>();
        //只查询Id和Name
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

  ``` java
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

``` java
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

``` java
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

``` java
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

``` java 
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

- 编码中添加了数据库未定义的属性

  在未定义的属性上加上@TableField（exist = false）表示数据库里不存在

- 不想让密码被查询出来

  ``` java
  public class User{
      @TableField(value="pwd",select = false)
      private String password;
  }
  ```

- 表名与编码开发设计不同

  ``` java
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

``` java
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

``` yaml
mybatis-plus:
  global-config:
    db-config:
    	id-type: assign_id
    	#默认表名前缀
    	table-prefix: tbl_
```

## 多数据操作（删除与查询）

``` java
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

   ``` java
   public class User {
   @TableLogic(value="0",delval="1")
       //value为正常数据的值，delval为删除数据的值
       private Integer deleted;}
   ```

   或者在配置文件中添加全局配置

   ``` yaml
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

   ``` java
   public class User {    
       @Version  
       private Integer version;
   }
   ```

3. 添加乐观锁的拦截器

   ``` java
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

   ``` java
       @Test
       void testUpdate(){
           User user = new User();
           user.setId(3L);
           user.setName("Jock666");
           user.setVersion(1);
           userDao.updateById(user);
       }
   ```

   ``` java
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

``` java
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

   ``` xml
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

   ``` java
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

   