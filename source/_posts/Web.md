---
title: JavaWeb笔记
date: 2021-01-15
tags: JavaWeb
categories: 笔记
description: JavaWeb笔记
cover: https://zhuxinpeng.oss-cn-hangzhou.aliyuncs.com/10.jpg
---
**service层**：服务层，对一个或多个DAO层进行封装

**controller层**：业务层，主要用于给前端返回数据的以及接收前端的数据的

**dao层**：数据访问层，**业务逻辑层**与**数据库层**之间的中间层

# SQL

登录MySql：mysql -u用户名 -p密码 -h要连接的mysql服务器的ip地址(默认127.0.0.1) -P端口号(默认3306)

单行注释：-- 注释内容（注意--后边有空格）或#注释内容（MySQL特有）
多行注释：/*注释*/

**数据类型**

- bigint：8个字节，一般用作id

- tinyint：1个字节，一般用作性别int：大整数类型，占四个字节

- double：浮点类型 使用格式： 字段名 double(总长度,小数点后保留的位数)

- date：日期值。只包含年月日

- datetime：混合日期和时间值。包含年月日时分秒

- char：定长字符串，存储性能高，但是浪费空间

  name char(10) 如果存储的数据字符个数不足10个，也会占10个的空间

- varchar：变长字符，节约空间，但是存储性能底

  varchar(11)：最多存储11个字符，超过则不存

  name varchar(10) 如果存储的数据字符个数不足10个，那就数据字符个数是几就占几个的空间

- Decimal可用来保存具有小数点而且数值确定的数值，2”表示小数部分的位数，如果插入的值未指定小数部分或者小数部分不足两位则会自动补到2位小数，若插入的值小数部分超过了2为则会发生截断，截取前2位小数。

- enum 类型允许将一个字段的值限制在一组指定的值之内，这样就可以避免输入不合法的性别值。

  ```mysql
  CREATE TABLE mytable (
    id INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
    gender ENUM('男', '女', '未知') NOT NULL
  );
  ```

**SQL分类**：

**DDL**：数据定义语言，用来定义数据库对象：数据库，表，列等；就是用来操作数据库，表等

**操作数据库**：

- SHOW DATABASES：查询所有的数据库
- CREATE DATABASE 数据库名称：创建数据库
- CREATE DATABASE IF NOT EXISTS 数据库名称：创建数据库(判断，如果不存在则创建)
- DROP DATABASE 数据库名称：删除数据库
- DROP DATABASE IF EXISTS 数据库名称：删除数据库(判断，如果存在则删除)
- USE 数据库名称：使用数据库
- SELECT DATABASE()：查看当前使用的数据库

**操作表**：

在MySQL数据库中，字段或列的注释是用属性**comment**来添加。创建新表的脚本中，可在字段定义脚本中添加comment属性来添加注释。

ENGINE=InnoDB：存储引擎是innodb

innoDB 是 MySQL 上第一个提供外键约束的数据存储引擎，除了提供事务处理外，InnoDB 还支持行锁，提供和 Oracle 一样的一致性的不加锁读取，能增加并发读的用户数量并提高性能，不会增加锁的数量。InnoDB 的设计目标是处理大容量数据时最大化性能，它的 CPU 利用率是其他所有基于磁盘的关系数据库引擎中最有效率的。

- 查询当前数据库下所有表名称：SHOW TABLES：
- 创建表：CREATE TABLE 表名 ( 字段名1 数据类型1, 字段名2 数据类型2, …字段名n 数据类型n );
- 查询表结构：DESC 表名称
- 删除表：DROP TABLE 表名;
- 删除表时判断表是否存在：DROP TABLE IF EXISTS 表名;
- 修改表名：ALTER TABLE 表名 RENAME TO 新的表名;
- 添加一列：ALTER TABLE 表名 ADD 列名 数据类型;
- 修改数据类型：ALTER TABLE 表名 MODIFY 列名 新数据类型;
- 修改列名和数据类型：ALTER TABLE 表名 CHANGE 列名 新列名 新数据类型;
- 删除列：ALTER TABLE 表名 DROP 列名;

**MySQL的BTREE索引和HASH索引**

为什么要用索引？

- 使用索引后减少了存储引擎需要扫描的数据量，加快查询速度
- 索引可以把随机I/O变为顺序I/O
- 索引可以帮助我们对所搜结果进行排序以避免使用磁盘临时表

Mysql支持的索引类型：B-TREE索引与HASH索引

**B-TREE索引**

- B-TREEB-TREE以B+树结构存储数据，大大加快了数据的查询速度
- B-TREE索引在范围查找的SQL语句中更加适合（顺序存储）

**B-TREE索引使用场景**

- 全值匹配的查询SQL，如 where act_id= '1111_act'
- 联合索引汇中匹配到最左前缀查询，如联合索引 KEY idx_actid_name(act_id,act_name) USING BTREE，只要条件中使用到了联合索引的第一列，就会用到该索引，但如果查询使用到的是联合索引的第二列act_name，该SQL则便无法使用到该联合索引（注：覆盖索引除外）
- 匹配模糊查询的前匹配，如where act_name like '11_act%'
- 匹配范围值的SQL查询，如where act_date > '9865123547215'（not in和<>无法使用索引）
- 覆盖索引的SQL查询，就是说select出来的字段都建立了索引

**HASH索引**

- Hash索引基于Hash表实现，只有查询条件精确匹配Hash索引中的所有列才会用到hash索引
- 存储引擎会为Hash索引中的每一列都计算hash码，Hash索引中存储的即hash码，所以每次读取都会进行两次查询
- Hash索引无法用于排序
- Hash不适用于区分度小的列上，如性别字段



**DML**：数据操作语言，用来对数据库中表的数据进行增删改；就是对表中数据进行增删改

- 给指定列添加数据：INSERT INTO 表名(列名1,列名2,…) VALUES(值1,值2,…);

- 给全部列添加数据：INSERT INTO 表名 VALUES(值1,值2,…);

- 批量添加数据：INSERT INTO 表名(列名1,列名2,…) VALUES(值1,值2,…)；

  INSERT INTO 表名 VALUES(值1,值2,…)；//给所有列添加可以省略

- 修改表数据：UPDATE 表名 SET 列名1=值1,列名2=值2,… [WHERE 条件] ;

  修改语句中如果不加条件，则将表中所有数据都修改！

  像上面的语句中的中括号，表示在写sql语句中可以省略这部分

- 删除数据：DELETE FROM 表名 [WHERE 条件] ;//如果不加条件，则将表中所有数据都被删除

**DQL**：数据查询语言，用来查询数据库中表的记录(数据)；就是对数据进行查询操作。

SELECT         字段列表
FROM           表名列表
WHERE         条件列表
GROUP BY   分组字段
HAVING       分组后条件
ORDER BY    排序字段
LIMIT             分页限定

- 查询所有列的数据可以用*代替，不推荐
- 使用distinct修饰字段可以去除重复记录
- 字段后加as可以给字段起别名
- 字段后加like表示占位符 //模糊查询  _ : 代表单个任意字符     % : 代表任意个数字符
   - select * from stu where name like '马%'; //查询姓'马'的学员信息
   - select * from stu where name like '_花%';//查询第二个字是'花'的学员信息
   - select * from stu where name like '%德%';//查询名字中包含 '德' 的学员信息

**字段排序**：SELECT 字段列表 FROM 表名 ORDER BY 排序字段名1 [排序方式1],排序字段名2 [排序方式2] …;

- ASC ： 升序排列 （默认值） DESC ： 降序排列
- 如果有多个排序条件，当前边的条件值一样时，才会根据第二条件进行排序

**聚合函数**：将一列数据作为一个整体，进行纵向计算。

格式：SELECT 聚合函数名(列名) FROM 表;  //null 值不参与所有聚合函数运算

- count(列名) ：统计数量
- max(列名) ：  最大值
- min(列名) ：  最小值
- sum(列名) ： 求和
- avg(列名) ： 平均值

**分组查询**：SELECT 字段列表 FROM 表名 [WHERE 分组前条件限定] GROUP BY 分组字段名 [HAVING 分组后条件过滤];
//分组之后，查询的字段为聚合函数和分组字段，查询其他字段无任何意义

**分页查询**：SELECT 字段列表 FROM 表名 LIMIT 起始索引 , 查询条目数;

- 上述语句中的起始索引是从0开始
- 计算公式：起始索引 = (当前页码 - 1) * 每页显示的条数
- limit是MySQL方言，Oracle使用rownumber,SQL Server使用top

**DCL**：数据控制语言，用来定义数据库的访问权限和安全级别，及创建用户；就是对数据库进行权限控制。

**约束**：在创建表时，添加元素后面添加约束，比如id int NOT NUL UNIQUE表示非空且唯一，不需要连接符

- 非空约束： 关键字是 NOT NULL，保证列中所有的数据不能有null值。

   - 建完表后添加：ALTER TABLE 表名 MODIFY 字段名 数据类型 NOT NULL;
   - 删除约束：ALTER TABLE 表名 MODIFY 字段名 数据类型;

- 唯一约束： 关键字是 UNIQUE，保证列中所有数据各不相同。

  唯一约束(Unique Key)：表示用Unique Key标记的字段的所有内容不能出现重复，否则将会报错

   - 建完表后添加：ALTER TABLE 表名 MODIFY 字段名 数据类型 UNIQUE;
   - 删除约束：ALTER TABLE 表名 DROP INDEX 字段名;

- 主键约束： 关键字是 PRIMARY KEY，主键是一行数据的唯一标识，要求非空且唯一。一般我们都会给每张表添加一个主键列用来唯一标识数据，例如id

   - 建完表后添加： ALTER TABLE 表名 ADD PRIMARY KEY(字段名);
   - 删除约束：ALTER TABLE 表名 DROP PRIMARY KEY;

- 检查约束： 关键字是 CHECK，保证列中的值满足某一条件。//MySQL不支持检查约束。


- 默认约束： 关键字是 DEFAULT，保存数据时，未指定值则采用默认值。
   - 建完表后添加： ALTER TABLE 表名 ALTER 列名 SET DEFAULT 默认值;
   - 删除约束：ALTER TABLE 表名 ALTER 列名 DROP DEFAULT;

- 自动增长： auto_increment 当列是数字类型 并且唯一约束，会随着行数自动增长


- 外键约束： 关键字是 FOREIGN KEY，外键用来让两个表的数据之间建立链接，保证数据的一致性和完整性。

  执行SET FOREIGN_KEY_CHECKS=0，禁用外键约束，这样就可以删除由于foreign key关联，无法更新或删除数据。

   - 创建表时添加：

  ```mysql
  CREATE TABLE 表名( 
       列名 数据类型,
       …
       [CONSTRAINT]  [外键名称] FOREIGN KEY (外键列名)  REFERENCES 主表(主表列名) );
  ```

   - 建完表后添加：ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY (外键字段名称) REFERENCES 主表名称(主表列名称);

   - 删除外键约束：ALTER TABLE 表名 DROP FOREIGN KEY 外键名称;

   - 先添加主表，再添加从表

- 多对多表关系：实现方式建立第三张中间表，中间表至少包含两个外键，分别关联两方主键

- 一对一表关系：实现方式在任意一方加入外键，关联另一方主键，并且设置外键列为唯一(UNIQUE)



内连接查询：相当于查询AB交集数据  例：select * from emp , dept where emp.dep_id = dept.did;

- 隐式内连接 SELECT 字段列表 FROM 表1,表2… WHERE 条件;

- 显示内连接 SELECT 字段列表 FROM 表1 [INNER] JOIN 表2 ON 条件;

外连接查询：

- 左外连接查询 ：相当于查询A表所有数据和交集部门数据         SELECT 字段列表 FROM 表1 LEFT [OUTER] JOIN 表2 ON 条件;

- 右外连接查询 ：相当于查询B表所有数据和交集部分数据         SELECT 字段列表 FROM 表1 RIGHT [OUTER] JOIN 表2 ON 条件;

子查询：查询中嵌套查询，称嵌套查询为子查询。

- 子查询语句结果是单行单列，子查询语句作为条件值，使用 = != > < 等进行条件判断

  ```mysql
  例：select * from emp where salary > (select salary from emp where name = '猪八戒');
  ```

- 子查询语句结果是多行单列，子查询语句作为条件值，使用 in 等关键字进行条件判断

  ```mysql
  例：select * from emp where dep_id in 
  (select did from dept where dname = '财务部' or dname = '市场部');
  ```

- 子查询语句结果是多行多列，子查询语句作为虚拟表

  ```mysql
  例：select * from (select * from emp where join_date > '2011-11-11' ) t1, 
  dept where t1.dep_id = dept.did;      
  ```



**数据库的事务**：包含了一组数据库操作命令，这一组数据库命令要么同时成功，要么同时失败。

- 开启事务：START TRANSACTION 或者 BEGIN
- 提交事务：commit
- 回滚事务：rollback

**事务的四大特征ACID**：

- 原子性：事务是不可分割的最小操作单位，要么同时成功，要么同时失败

- 一致性：事务完成时，必须使所有的数据都保持一致状态

- 隔离性：多个事务之间，操作的可见性

- 持久性：事务一旦提交或回滚，它对数据库中的数据的改变就是永久的




## 读写分离：主从复制

主库进行增删改操作，从库尽心读操作

MySQL主从复制是一个异步的复制过程，底层是基于Mysql数据库自带的**二进制日志**功能。就是一台或多台MYSQL数据库（slave从库）从另一台MySQL数据库（master主库）进行日志的复制然后再解析日志并应用到自身，最终实现从库的数据和主库的数据保持一致。MySQL主从复制，是MySQL数据库自带的功能，无需借助第三方工具。

MySql复制分为三步

- mster将改变记录到二进制日志
- slave将master的binary log拷贝到它的中继日志(relay log)
- slave重做中继日志中的事件，将改变应用到自己的数据库中

**配置主库**

- 修改Mysql数据库的配置文件/etc/my.cnf

- 找到[mysqld]，在下边加上

  log-bin=mysql-bin #启用二进制日志

  server-id=100 #服务器唯一ID

- 重启MySQL服务：systemctl restart mysqld

- 登录数据库，执行

  grant replication slave on *.* to 'zxp'@'%' identified by 'Root@1234';

  创建一个用户zxp，密码为Root@1234，并且给zxp用户授予 replication slave 权限。常用于建立复制时所需要用到的用户权限，也就是slave必须master授权具有该权限的用户，才能通过该用户复制。

- 执行下面SQL，记录下File和position的值：show master status;

  注：上面SQL是为了查看master状态，执行完后不要再执行任何操作

**配置从库**

- 修改Mysql配置文件/etc/my.cnf

- 找到[mysqld]，在下边加上

  server-id=101#服务器唯一ID

- 重启MySQL服务：systemctl restart mysqld

- 登录MySQL执行：

  change master to master_host='104.243.21.3',master_user='zxp',master_password='Root@1234',master_log_file='mysql-bin.000002',master_log_pos=436;

   - master_host：主库ip
   - master_user：主库设置的user
   - master_password：主库设置的密码
   - master_log_file：主库中file的值
   - master_log_pos：主库position的值

  在MySQL执行：start slave;

- 查看从库状态：show slave status;

**测试**

在主库创建一个数据库，如果从库也同时创建说明成功

**Sharding-JDBC**

Sharding-JDBC定位为轻量级Java框架，在Java的JDBC层提供的额外服务。它使用客户端直连数据库，以jar包形式提供服务，无需额外部署和依赖，可理解为增强版的JDBC驱动，完全兼容JDBC和各种ORM框架。

使用Sharding-JDBC可以在程序中轻松的实现数据库读写分离

- 适用于任何基于JDBC和ORM框架，如：JPA，Hibernate，Mybatis，Spring JDBC Tempalte或直接使用JDBC
- 支持任何第三方的数据库连接池，如：DBCP，C3P0，BoneCP，Druid，HikariCP等
- 支持任意实现JDBC规范的数据库，目前支持MySQL，Oracle，SQLServer，PostagreSQL以及任何遵循SQL92标准的数据库

1.导入maven坐标

```xml
<dependency>
  <groupId>org.apache.shardingsphere</groupId>
  <artifactId>sharding-jdbc-spring-boot-starter</artifactId>
  <version>4.0.0-RC1</version>
</dependency>
```

2.配置文件

```yaml
spring:
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
```

# **JDBC**

```java
// 加载配置文件
Properties prop = new Properties();
prop.load(new FileInputStream("s-demo/src/druid.properties")); 

//获取连接池对象
DataSource dataSource = DruidDataSourceFactory.createDataSource(prop);

// 获取数据库连接
Connection conn = dataSource.getConnection();

//获取PreparedStatement对象(固定) 
PreparedStatement pstmt = conn.prepareStatement(sql);

//执行SQL
ResultSet rs = pstmt.executeQuery();

//释放资源(固定)
rs.close(); //增删改时没有rs  
pstmt.close();  
conn.close();
```



**DriverManager**：驱动管理类

static void registerDriver(Driver driver)：使用DriverManager注册给定的驱动程序

static Connection getConnection(String url, String user, String password)：尝试建立与给定数据库URL的连接，并返回一个连接对象

URL：连接路径

语法：jdbc:mysql://ip地址(域名):端口号/数据库名称?参数键值对1&参数键值对2…

示例：jdbc:mysql://127.0.0.1:3306/db1

- 如果连接的是本机mysql服务器，并且mysql服务默认端口是3306，则url可以简写为：jdbc:mysql:///数据库名称
- 配置 useSSL=false 参数，禁用安全连接方式，解决警提示jdbc:mysql://db1?useSSL=false



**Connection**：数据库连接类        作用：获取执行 SQL 的对象和管理事务

- 普通执行SQL对象：Statement createStatement()
- 预编译SQL的执行SQL对象（防止SQL注入）：PreparedStatement prepareStatement(sql)
- 执行存储过程的对象：CallableStatement prepareCall(sql)

**JDBC事务管理的方法**：Connection接口中定义了3个对应的方法

- 开启事务：void setAutoCommit(boolean autoCommit) //true为自动提交事务，false为手动提交事务
- 提交事务：void commit()
- 回滚事务：void rollback() //try catch

**Statement**：执行SQL语句

- int executeUpdate(String sql)：执行DML（对表中数据进行增删改）返回值为DML影响的行数

- ResultSet executeQuery(String sql)：执行DQL（对数据进行查询）语句

- ResultSet结果集对象：封装了DQL查询语句的结果

- boolean next()：将光标从当前位置向前移动一行，判断当前行是否为有效行，true ： 有效行，当前行有数据  false ： 无效行，当前行没有数据

- xxx getXxx(参数)：获取数据 xxx : 数据类型；

  int getInt(参数)，参数类型为int，代表第几列(从1开始)   
  例：String name = rs.getString(2)：获取该行第二列的字符串， 参数类型为String，代表列的名称



SQL注入是通过操作输入来修改事先定义好的SQL语句，用以达到执行代码对服务器进行攻击的方法。

**PreparedStatement**：预编译SQL语句并执行，性能更高，预防SQL注入，将敏感字符进行转义

在url后面加上useServerPrepStmts = true 参数开启预编译功能

- 获取获取 PreparedStatement 对象

```java
// SQL语句中的参数值，使用?占位符替代
String sql = "select * from user where username = ? and password = ?";

// 通过Connection对象获取，并传入对应的sql语句
PreparedStatement pstmt = conn.prepareStatement(sql);
```

- 设置?参数值

  PreparedStatement对象：setXxx(参数1，参数2)：给 ? 赋 值

  Xxx：数据类型 ； 如 setInt (参数1，参数2)

  参数1： ？的位置编号，从1 开始

  参数2： ？的值

- 执行SQL语句

  //调用这两个方法时不需要传递SQL语句，因为获取SQL语句执行对象时已经对SQL语句进行预编译了。

  executeUpdate(); 执行DDL语句和DML语句

  executeQuery(); 执行DQL语句



**数据库连接池**：数据库连接池是个容器，负责分配、管理数据库连接

优点：资源重用，提升系统响应速度，避免数据库连接遗漏

**DataSource**：数据库连接池标准接口

该接口提供了获取连接的功能：Connection getConnection()

常见的数据库连接池：DBCP，C3P0，Druid（常用）

System.out.println(System.getProperty("user.dir")) ：找到当前绝对路径



# Maven

Maven：是专门用于管理和构建Java项目的工具

- 提供了一套标准化的项目结构
- 提供了一套标准化的构建流程（编译，测试，打包，发布……）
- 提供了一套依赖管理机制

常用命令：

- compile ：编译
- clean：清理
- test：测试
- package：打包
- install：安装jar包到仓库

Maven 中的坐标是资源的唯一标识，使用坐标来定义项目或引入项目中需要的依赖

Maven 坐标主要组成：

- groupId：定义当前Maven项目隶属组织名称（通常是域名反写，例如：com.itheima）
- artifactId：定义当前Maven项目名称（通常是模块名称，例如 order-service、goods-service）
- version：定义当前项目版本号

Maven项目导入：导入pom文件即可
导入jar包：<dependencies> → <dependency>        如果报错右上角下载
想要移除jar包或者添加jar包，要右上角刷新后才能生效
alt + insert：选择dependcy快速搜索已经下载的jar包

依赖范围：依赖通过 scope 标签指定依赖的作用范围

- compile ：作用于编译环境、测试环境、运行环境
- test ： 作用于测试环境。典型的就是Junit坐标，以后使用Junit时，都会将scope指定为该值
- provided ：作用于编译环境、测试环境。我们后面会学习 servlet-api ，在使用它时，必须将 scope 设置为该值，不然运行时就会报错
- runtime ： 作用于测试环境、运行环境。jdbc驱动一般将 scope 设置为该值，当然不设置也没有任何问题
- 如果引入坐标不指定 scope 标签时，默认就是 compile 值。以后大部分jar包都是使用默认值。



**Maven依赖管理:**
如果一个项目一要用到项目二的资源，把项目二的坐标复制后，以denpendency的方式添加到项目一：

```xml
    <dependency>
  <groupId>org.example</groupId>
  <artifactId>login-register</artifactId>
  <version>1.0-SNAPSHOT</version>
    </dependency>
```

如果不想用项目二的某个资源可以使用排除依赖：

```xml
    <dependency>
  <groupId>org.example</groupId>
  <artifactId>login-register</artifactId>
  <version>1.0-SNAPSHOT</version>
<exclusions>
<exclusion>
<groupId>log4j</groupId>
<artifactId>log4j</artifactId>
</exclusion>
</exclusions>
    </dependency>

<optional>true</optional>：对外隐藏该资源
```

依赖范围：依赖通过 scope 标签指定依赖的作用范围

- compile ：作用于编译环境、测试环境、运行环境
- test ： 作用于测试环境。典型的就是Junit坐标，以后使用Junit时，都会将scope指定为该值
- provided ：作用于编译环境、测试环境。我们后面会学习 servlet-api ，在使用它时，必须将 scope 设置为该值，不然运行时就会报错
- runtime ： 作用于测试环境、运行环境。jdbc驱动一般将 scope 设置为该值，当然不设置也没有任何问题
- 如果引入坐标不指定 scope 标签时，默认就是 compile 值。以后大部分jar包都是使用默认值。

Maven插件：

- 插件与生命周期内的阶段绑定，在执行到对应生命周期时执行对应的插件功能
- 默认maven在各个生命周期上绑定有预设的功能
- 通过插件可以自定义其他功能



# Mybatis

Mybatis：MyBatis 是一款优秀的持久层框架，用于简化 JDBC 开发

持久层：负责将数据到保存到数据库的那一层代码。

调用Mybatis完成sql语句：

- 获取SqlSessionFactory对象
- 获取SqlSession对象
- 获取Mapper
- 调用方法
- 释放资源

创建SqlSessionFactoryUtils工具类：

- 定义SqlSessionFactory sqlSessionFactory成员变量

- 用静态代码块的形式存放这三行代码，这样sql工厂的创建只随着类的加载自动创建一次，且只执行一次

  ```java
  String resource = "mybatis-config.xml";
  InputStream inputStream = Resources.getResourceAsStream(resource);
  sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
  ```

- 在方法中返回sql工厂对象

**Mapper代理开发**：

- 定义返回值对象类，如User类

- 定义与SQL映射文件同名的Mapper接口（如UserMapper），并将Mapper接口与SQL映射文件(UserMapper.xml)放置在同一目录

- 在resource文件下新建一个com/zxp/mapper，把SQL映射文件放进去，使SQL映射文件和Mapper在同一个目录下

- 设置SQL映射文件的namespace属性为Mapper接口全限定名，namespace = com.zxp.mapper.UserMapper

- 在Mapper接口中定义方法，方法名就是SQL映射文件sql语句的id，并保持参数类型和返回值类型一致

- 在mybatis-config中重新加载sql映射文件，把原来的mapper resource替换成

  <package name = "com.zxp.mapper"/>，把所有映射文件的目录传进去

- 主体代码（见文件）

可以在sql映射文件中给字段进行映射：在sql文件中<resultMap>，id为唯一标识，type为类所在位置。

在内部，id为主键字段，result为一般字段映射，column为表的列名，property为实体类的属性名
<result column="brand_name" property="brandName"/>

然后把下边的resultType换成resultMap = 前边定义的唯一标识

参数占位符：
#{ }：将sql语句中对应位置替换为?，为了防止sql注入，里边最好填对应参数的名称
${ }：拼sql，会存在sql注入问题

特殊字符处理：
转义字符：小于："&lt;"   大于："&gt;"
CDATA区：输入CD在里边写符号

**多条件查询**：(like后的%在service中拼接)

```mysql
<select id="selectByCondition" resultMap="brandResultMap">
        select *
        from tb_brand
        where status = #{status}
          and company_name like #{companyName}
          and brand_name like #{brandName};
</select>
```

给参数加上注释，注释的名称应该与占位参数名称一致

```java
List<Brand> selectByCondition(@Param("status")int status,@Param("companyName") String 

companyName,@Param("brandName") String brandName);//括号里的名称与参数占位符里的名称一致
```


List<Brand> selectByCondition(Brand brand);

把参数封装成一个对象，把参数传给该对象

```mysql
 select *
        from tb_brand
        <where>
            <if test="status != null">
                and status = #{brand.status}
            </if>
            <if test="companyName != null and companyName != '' ">
                and company_name like #{brand.companyName}
            </if>
            <if test="brandName != null and brandName != '' ">
                and brand_name like #{brand.brandName}
            </if>
        </where>
```

List<Brand> selectByCondition(Map map);

把参数名称和参数传递到哈希表中，把map对象传给方法



**动态sql查询**：

- if：注意test后边的参数名要和你传入的参数名一致，

```mysql
<where>
<if test="companyName != null and companyName != '' ">
            and company_name like #{companyName}
</if>
<if test="brandName != null and brandName != '' ">
            and brand_name like #{brandName}
</if>
</where>
```

- 加上where标签可以防止and字符报错

- choose（when,otherwise）：选择，类似于switch语句

  ```mysql
  - <choose>
  -  <when test="brandName != null and brandName != '' ">
  -    brand_name like #{brandName}
  - </when>
  - <otherwise> 1=1 </otherwise>
  - </choose>
  - 可以把otherwise替换成when标签
  ```



使用java进行sql添加功能时

- 要手动提交事务sqlSession.commit

- SqlSession sqlSession = sqlSessionFactory.openSession(true);打开自动提交（关闭事务）

- ```xml
  <insert id="add" useGeneratedKeys="true" keyProperty="id">//这样添加的信息就可以访问主键了
  ```



批量删除sql：参数加@Param注释，否则collection默认应该为array，separator为连接符，open="("：开始拼一个（

```xml
<delete id="deleteById">
        delete from tb_brand where id
        in(<foreach collection="ids" item="id" separator=",">
            #{id}
        </foreach> );
</delete>
```



# HTML

W3C标准：网页主要由结构：HTML，表现：CSS，行为：JavaScript

**HTML标签**

**表单标签**

表单：在网页中主要负责数据采集功能，使用<form>标签定义表单

表单项（元素）：不同类型的input元素，下拉列表，文本域等

![image-20221023163707043](C:\Users\98110\AppData\Roaming\Typora\typora-user-images\image-20221023163707043.png)

form：定义表单

- action：规定当提交表单时向何处发送表单数据，URL
- method：规定用于发送表单数据的方式

<img src="C:\Users\98110\AppData\Roaming\Typora\typora-user-images\image-20221023163932035.png" alt="image-20221023163932035" style="zoom:67%;" />

**表格标签**

<img src="C:\Users\98110\AppData\Roaming\Typora\typora-user-images\image-20221023163947242.png" alt="image-20221023163947242" style="zoom: 80%;" />

**布局标签**

<img src="C:\Users\98110\AppData\Roaming\Typora\typora-user-images\image-20221023164004259.png" alt="image-20221023164004259" style="zoom: 80%;" />

**超链接标签**

<img src="C:\Users\98110\AppData\Roaming\Typora\typora-user-images\image-20221023164018720.png" alt="image-20221023164018720" style="zoom:67%;" />

**基础标签**

![image-20221023164033163](C:\Users\98110\AppData\Roaming\Typora\typora-user-images\image-20221023164033163.png)

**图片音频视频标签**

<img src="C:\Users\98110\AppData\Roaming\Typora\typora-user-images\image-20221023164250897.png" alt="image-20221023164250897" style="zoom:67%;" />

**列表标签**

<img src="C:\Users\98110\AppData\Roaming\Typora\typora-user-images\image-20221023164313684.png" alt="image-20221023164313684" style="zoom:67%;" />

**JavaScript**

JavaScript 引入方式（HTML 和 JavaScript 的结合方式）有两种：

内部脚本：将 JS代码定义在HTML页面中

- 在 HTML 中，JavaScript 代码必须位于 <script> 与 </script> 标签之间
- 一般把脚本置于元素的底部，可改善显示速度因为浏览器在加载页面的时候会从上往下进行加载并解析。 我们应该让用户看到页面内容，然后再展示动态的效果

外部脚本：将 JS代码定义在外部 JS文件中，然后引入到 HTML页面中

- 定义外部 js 文件，如定义名为 demo.js的文件
- 在页面中引入外部的js文件
- 在页面使用 script 标签中使用 src 属性指定 js 文件的 URL 路径。
- 外部脚本不能包含 <script> 标签：在js文件中直接写 js 代码即可，不要在 js文件中写 script 标签
- 标签不能自闭合在页面中引入外部js文件时，不能写成 <script src="../js/demo.js" />

JavaScript书写语法

- 区分大小写：与 Java 一样，变量名、函数名以及其他一切东西都是区分大小写的
- 每行结尾的分号可有可无
- 如果一行上写多个语句时，必须加分号用来区分多个语句。
- 注释：
   - 单行注释：// 注释内容
   - 多行注释：/* 注释内容 */



JavaScript 没有文档注释

输出语句：

- 使用 window.alert() 写入警告框
- 使用 document.write() 写入 HTML 输出
- 使用 console.log() 写入浏览器控制台



变量：JavaScript 中用 var 关键字（variable 的缩写）来声明变量。格式 var 变量名 = 数据值

JavaScript 是一门弱类型语言，变量可以存放不同类型的值

var关键字：

- 作用域：全局变量
- 变量可以重复定义
- let 关键字定义变量,它的用法类似于 var ，但是所声明的变量，只在 let 关键字所在的代码块内有效，且不允许重复声明。
- const关键字，用来声明一个只读的常量，一旦声明，常量的值就不能改变。



**数据类型**：原始类型和引用类型

原始数据类型：

- number：数字（整数、小数、NaN(Not a Number)）

- string：字符、字符串，单双引皆可

- boolean：布尔。true，false

- null：对象为空

- undefined：当声明的变量未初始化时，该变量的默认值是 undefined

- 使用typeof运算符可以获取数据类型



== 和 ===区别：
==：

1. 判断类型是否一样，如果不一样，则进行类型转换
2. 再去比较其值
   ===：js 中的全等于
3. 判断类型是否一样，如果不一样，直接返回false
4. 再去比较其值
5.

**类型转换**：

string 转换为 number 类型：按照字符串的字面值，转为数字。如果字面值不是数字，则转为NaN

- 使用 + 正号运算符
- 使用 parseInt() 函数(方法)

1. boolean 转换为 number 类型：true 转为1，false转为0

2. number 类型转换为 boolean 类型：0和NaN转为false，其他的数字转为true
3. string 类型转换为 boolean 类型：空字符串转为false，其他的字符串转为true
4. null类型转换为 boolean 类型是 false
5. undefined 转换为 boolean 类型是false

函数：是被设计为执行特定任务的代码块；JavaScript 函数通过 function 关键词进行定义

- function 函数名(参数1,参数2..){ 要执行的代码 }
- var 函数名 = function (参数列表){ 要执行的代码 }
- 形式参数不需要类型。因为JavaScript是弱类型语言
- 返回值也不需要定义类型，可以在函数内部直接使用return返回即可

JavaScript Array对象用于定义数组:

- var 变量名 = new Array(元素列表);
- var 变量名 = [元素列表];
- JavaScript 中数组的长度是可以变化的， JavaScript 是弱类型，所以可以存储任意类型的数据。

String：trim方法可以去除字符串的空白部分

自定义对象：

```javascript
var 对象名称 = { 
属性名称1:属性值1, 属性名称2:属性值2, ..., 
函数名称:function (形参列表){},
 ... };
```

BOM：Browser Object Model 浏览器对象模型

- JavaScript 将浏览器的各个组成部分封装为对象
- Window：浏览器窗口对象
- Navigator：浏览器对象
- Screen：屏幕对象
- History：历史记录对象
- Location：地址栏对象

**Window**：浏览器窗口对象
方法：
alert()：显示带有一段消息和一个确认按钮的警告框
confirm()：显示带有一段消息一级确认按钮和取消按钮的对话框
setInterval(function,毫秒值)：按照指定的周期（以毫秒计）来调用函数或计算表达式
setTimeout(function,毫秒值 )：在指定的毫秒数后调用函数或计算表达式

**History**：历史记录对象
获取：使用window.history.方法()        window可以省略
方法：
back()：加载history列表中的上一个URL
forward()：加载history列表中的下一个URL

**Location**：地址栏对象
获取：window.location.方法()     window可以省略
属性：
href：设置或返回完整的URL

**DOM**：Document Object Model 文档对象模型。

- JavaScript 将 HTML 文档的各个组成部分封装为对象
- Document：整个文档对象
- Element：元素对象
- Attribute：属性对象
- Text：文本对象
- Comment：注释对象

作用：

- JavaScript 通过 DOM， 就能够对 HTML进行操作了
- 改变 HTML 元素的内容
- 改变 HTML 元素的样式（CSS） 对 HTML DOM 事件作出反应
- 添加和删除 HTML 元素

Element：HTML 中的 Element 对象可以通过 Document 对象获取，而 Document 对象是通过 window 对象获取。

Document 对象中提供了以下获取 Element 元素对象的函数：

- getElementById() ：根据id属性值获取，返回单个Element对象
- getElementsByTagName() ：根据标签名称获取，返回Element对象数组
- getElementsByName() ：根据name属性值获取，返回Element对象数组
- getElementsByClassName() ：根据class属性值获取，返回Element对象数组

style：设置元素css样式
innerHTML：设置元素内容
checked：使所有的复选框呈现被选中的状态



**事件**：是发生在 HTML 元素上的“事情，比如：页面上的按钮被点击，鼠标移动到元素之上 ，按下键盘按键 等都是事件

事件监听：是JavaScript 可以在事件被侦测到时执行一段逻辑代码

事件绑定

- 通过 HTML标签中的事件属性进行绑定

  ```javascript
  - <input type="button" onclick='on()'>
  - function on(){ alert("我被点了"); }
  ```

- 通过 DOM 元素属性绑定

  ```javascript
  <input type="button" id="btn">
  
  document.getElementById("btn").onclick = function (){ alert("我被点了"); }
  ```

常见事件：

- onclick 鼠标单击事件
- onblur 元素失去焦点
- onfocus 元素获得焦点
- onload 某个页面或图像被完成加载
- onsubmit 当表单提交时触发该事件
- onmouseover 鼠标被移到某元素之上
- onmouseout 鼠标从某元素移开



**正则表达式**：

- ^：表示开始
- $：表示结束
  - [ ]：代表某个范围内的单个字符，比如： [0-9] 单个数字字符
- .：代表任意单个字符，除了换行和行结束符
- \w：代表单词字符：字母、数字、下划线()，相当于 [A-Za-z0-9]
- \d：代表数字字符： 相当于 [0-9]

量词：

- +：至少一个
- *：零个或多个
- ？：零个或一个
- {x}：x个
- {m,}：至少m个
- {m,n}：至少m个，最多n个

正则对象创建方式：
直接量方式：var reg = /正则表达式/                 /^\w{6,12}$/
创建 RegExp 对象：var reg = new RegExp("正则表达式")



# HTTP

**HTTP**：超文本传输协议，规定了浏览器和服务器之间数据传输的规则
特点：

- 基于TCP协议：面向连接，安全

- 基于请求-响应模型的：一次请求对应一次响应，  请求和响应是一 一对应关系

- HTTP协议是无状态协议：对于事物处理没有记忆能力，每次请求-响应都是独立的

  无状态指的是客户端发送HTTP请求给服务端之后，服务端根据请求响应数据，响应完后，不会记录任何信息，这种特性有优点也有缺点

优点：速度快
缺点：多次请求间不能共享数据

HTTP请求数据格式：

请求行：请求数据的第一行，包含三部分，请求方式，请求URL路径，HTTP协议及版本

- 请求方式
- /表示请求资源路径
- HTTP/1.1表示协议版本

请求头：第二行开始，格式为key: value形式
常见的请求头

- Host: 表示请求的主机名
- User-Agent: 浏览器版本,例如Chrome浏览器的标识类似Mozilla/5.0 ...Chrome/79，IE浏览器的标识类似Mozilla/5.0 (Windows NT ...)like Gecko；
- Accept：表示浏览器能接收的资源类型，如text/*，image/*或者*/*表示所有；
- Accept-Language：表示浏览器偏好的语言，服务器可以据此返回不同语言的网页；
- Accept-Encoding：表示浏览器可以支持的压缩类型，例如gzip, deflate等。

请求体（和请求头之间空一行）：POST请求的最后一行，存放请求参数

GET和POST请求之间的区别：
GET请求请求参数在请求行中，没有请求体，POST请求请求参数在请求体中
GET请求请求参数大小有限制，POST没有

HTTP响应数据格式：

**响应行**：响应数据的第一行，响应行包含三块内容，HTTP协议及版本，响应状态码，状态码的描述

- HTTP/1.1[HTTP协议及版本]
- 200[响应状态码]
- ok[状态码的描述]

**响应头**：第二行开始，格式为key：value
HTTP常见响应头

- Content-Type：表示该响应内容的类型，例如text/html，image/jpeg；
- Content-Length：表示该响应内容的长度（字节数）；
- Content-Encoding：表示该响应压缩算法，例如gzip；
- Cache-Control：指示客户端应如何缓存，例如max-age=300表示可以最多缓存300秒

**响应体**：最后一部分，存放响应数据

状态码分类：

- 1xx：响应中，表示请求已经接受，告诉客户端应该继续请求或者如果已经完成则忽略他
- 2xx：成功，表示请求已经被成功接受，处理已完成
- 3xx：重定向，重定向到其他地方：它让客户端再发送一个请求以完成整个处理
- 4xx：客户端错误，处理发生错误，责任在客户端，如：客户端请求一个不存在的资源，客户端未被授权，禁止访问等
- 5xx：服务器端错误，处理发生错误，责任在服务器，如服务器抛出异常，路由错误，HTTP版本不支持等

# **ThreadLocal**

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

# Tomcat和Servlet

MavenWeb项目创建：

- 新建Maven模块，创建时选择maven-archetype-webapp
- 选择maven对应的文件夹和仓库
- 把poml文件按照电脑里的修改
- 新建java和resource文件夹

Tomcat部署项目：将项目放置到webapps目录下，即部署完成，一般项目会被打成war包，放在webapps目录下会被自动解压

Maven Web项目结构：

webapp：Web项目特有目录

webapp子目录：

- html：HTML文件目录（可自定义）

- WEB-INF：Web项目核心目录（必须叫这个名字）

  WEB-INF子文件：web.xml（Web项目配置文件）

Servlet：Java提供的一门动态web资源开发技术，是JavaEE规范之一，也就是接口

**Servlet快速入门**

- 创建web项目，导入Servlet依赖坐标
- 创建一个类，实现Servlet接口，重写所有方法，并在service方法中输入一句话
- 配置:在类上使用@WebServlet注解（属性名称为value且只有一个属性时，value可以省略），配置该Servlet的访问路径
- 访问:启动Tomcat,浏览器中输入URL地址访问该Servlet
- 访问后，在控制台会打印service方法的话，说明servlet程序已经成功运行。

```java
1. 创建Web项目`web-demo`，导入Servlet依赖坐标
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <version>3.1.0</version>
    <!--provided指的是在编译和测试过程中有效,最后生成的war包时不会加入-->
    <scope>provided</scope>
    
2. 创建:定义一个类，实现Servlet接口，并重写接口中所有方法，并在service方法中输入一句话
public class ServletDemo1 implements Servlet {

    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        System.out.println("servlet hello world~");
    }
    public void init(ServletConfig servletConfig) throws ServletException {

    }

    public ServletConfig getServletConfig() {
        return null;
    }

    public String getServletInfo() {
        return null;
    }

    public void destroy() {

    }
}

3. 配置:在类上使用@WebServlet注解，配置该Servlet的访问路径
@WebServlet("/demo1")
public class ServletDemo1 implements Servlet

4. 访问:启动Tomcat,浏览器中输入URL地址访问该Servlet
http://localhost:8080/web-demo/demo1

5. 器访问后，在控制台会打印`servlet hello world~` 说明servlet程序已经成功运行。
```

Servlet运行在Servlet容器(web服务器)中，其生命周期由容器来管理，分为4个阶段：

- 加载和实例化：默认情况下（当配置loadOnStartup(默认为负整数)= 0或正整数时，服务器启动时创建Servlet对象，数字越小优先级越高），当Servlet第一次被访问时，由容器创建Servlet对象
- 初始化：在Servlet实例化之后，容器将调用Servlet的init()方法初始化这个对象，完成一些如加载配置文件、创建连接等初始化的工作，该方法只调用一次
- 请求处理：每次请求Servlet时，Servlet容器都会调用Servlet的service()方法对请求进行处理
- 服务终止：当需要释放内存或者容器关闭时，容器就会调用Servlet实例的destroy()方法完成资源的释放。在destroy()方法调用之后，容器会释放这个Servlet实例，该实例随后会被Java的垃圾收集器所回收

urlPattern配置：Servlet类访问路径（@WebServlet(urlPatterns = {"/demo7","/demo8"})）

- 一个Servlet可以配置多个访问路径
- 精确匹配：@WebServlet(urlPatterns = "/user/select")
- 目录匹配：@WebServlet(urlPatterns = "/user/*")
- 扩展名匹配：@WebServlet(urlPatterns = "  * . do   ")
- 任意匹配（不要使用）：@WebServlet(“/”）或@WebServlet(“/*”）



# Request和Respone

**Request**的继承体系：

- ServletRequest：Java提供的请求对象根接口
- HttpServletRequest：Java提供的对Http协议封装的请求对象接口
- RequestFacade：Tomcat定义的实现类

HttpServletRequest：Java提供的对Http协议封装的请求对象接口：
请求行：

- String getMethod()：获取请求方式: GET
- String getContextPath()：获取虚拟目录(项目访问路径): /request-demo
- StringBuffer getRequestURL()：获取URL(统一资源定位符): http://localhost:8080/request-demo/req1
- String getRequestURI()：获取URI(统一资源标识符): /request-demo/req1
- String getQueryString()：获取请求参数(GET方式): username=zhangsan&password=123

请求头：
String getHeader(String name)：根据请求头名称获取对应值的方法

请求体（post方法）

- ServletInputStream getInputStream()：取字节输入流，比如传递的是文件数据，则使用该方法
- BufferedReader getReader()：获取字符输入流，如果前端发送的是纯文本数据，则使用该方法（不需要手动关闭）

Request将获取请求参数的方法进行了封装：

- Map<String,String[]> getParameterMap()：获取所有参数Map集合
- String[] getParameterValues(String name)：根据名称获取参数值（数组）
- String getParameter(String name)：根据名称获取参数值(单个值)

post解决中文乱码问题：
setCharacterEncoding("UTF-8")：设置字符输入流的编码

get解决中文乱码问题：

```java
byte[] bytes = username.getBytes(StandardCharsets.ISO_8859_1);
username = new String(bytes,StandardCharsets.UTF_8)
```

Request请求转发：

- request.getRequsetDispatcher("path").forward(request,response)：请求转发
- void setAttribute(String name,Object o)：存储数据到request域
- Object getAttribute(String name)：根据key获取值
- void removeAttribute(String name)：根据key删除该键值对

**Response**:设置响应数据

Reponse的继承体系：

- ServletReponse：Java提供的请求对象根接口
- HttpServletReponse：Java提供的对Http协议封装的请求对象接口
- RequestReponse：Tomcat定义的实现类

响应行：
void setStatus(int sc)：设置响应状态码
响应头：
void setHeader(String name,String value)：设置响应头键值对
响应体：
PrintWriter getWriter()：获取字符输出流
ServletOutputStream getOutputStream()：获取字节输出流

Response完成重定向：

- response.setStatus(302)：设置响应状态码302
- response.setHeader("location","资源B的访问路径")
  简化方式完成重定向：response.sendRedirect("location","资源B的访问路径")

重定向的特点:

- 浏览器地址栏路径发送变化
- 可以重定向到任何位置的资源(服务内容、外部均可)
- 两次请求，不能在多个资源使用request共享数据

请求转发的特点

- 浏览器地址栏路径不发生变化
- 只能转发到当前服务器的内部资源
- 一次请求，可以在转发资源间使用request共享数据

Response响应字符数据

- PrintWriter writer = resp.getWriter()
- response.setHeader("content-type","text/html")    //以html文本的形式响应
- writer.write("<h1>aaa</h1>")

响应数据包含中文时：

- response.setContentType（"text/html"；"charset=utf-8"）
- PrintWriter writer = resp.getWriter()
- writer.write("")

Response响应字节数据：

- 通过Response对象获取字节输出流：ServletOutputStream outputStream = resp.getOutputStream()
- 通过字节输出流写数据: outputStream.write(字节数据)

路径问题：
浏览器使用：需要加虚拟目录(项目访问路径)
服务端使用：不需要加虚拟目录



# Cookie和Session

**Cookie**：客户端会话技术，将数据保存到客户端（浏览器），以后每次请求都携带Cookie数据进行访问。

发送Cookie

- 创建Cookie对象，并设置数据

  Cookie cookie = new Cookie("key","value");

- 发送Cookie到客户端：使用response对象

  response.addCookie(cookie)

获取Cookie

- 获取客户端携带的所有Cookie，使用request对象

  Cookie[] cookies = request.getCookies();

- 遍历数组，获取每一个Cookie对象：for each

- 使用Cookie对象方法获取数据

  cookie.getName();
  cookie.getValue();

Cookie的原理：Cookie的实现原理是基于HTTP协议的
发送Cookie：响应头：set-cookie
获取Cookie：请求头: cookie

Cookie的存活时间：
默认情况下，Cookie存储在浏览器内存中，当浏览器关闭，内存释放，则Cookie被销毁

setMaxAge(int seconds)：设置Cookie存活时间

- 正数：将Cookie写入浏览器所在电脑的硬盘，持久化存储。到时间自动删除
- 负数：默认值，Cookie在当前浏览器内存中，当浏览器关闭，则Cookie被销毁
- 零：删除对应Cookie

Cookie存储中文：

- String value = "张三"；
- 对中文进行URL编码：对中文进行URL编码  value = URLEncoder.encode(value, "UTF-8");
- 将编码后的值存入Cookie中：Cookie cookie = new Cookie("username",value);
- 获取Cookie数组：Cookie[] cookies = request.getCookies();
- String value = cookie.getValue()；
- URL解码 value = URLDecoder.decode(value,"UTF-8");



**Session**：服务端会话跟踪技术：将数据保存到服务端

Session的基本使用：

获取Session对象,使用的是HttpServletRequest对象：HttpSession session = httpServletRequestrequest.getSession();

Session对象提供的功能:

- void setAttribute(String name, Object o)：存储数据到 session 域中
- Object getAttribute(String name)：根据 key，获取值
- void removeAttribute(String name)：根据 key，删除该键值对

Session的原理：Session是基于Cookie实现的

Session钝化与活化
钝化：在服务器正常关闭后，Tomcat会自动将Session数据写入硬盘的文件中
活化：再次启动服务器后，从文件中加载数据到Session中

session的销毁会有两种方式:

- 默认情况下，无操作，30分钟自动销毁

  ```xml
  在web.xml中配置销毁时间，单位为分钟
  <session-config> <session-timeout>100</session-timeout> </session-config>
  ```

- 调用Session对象的invalidate()进行销毁

**Cookie和Session区别**

- 存储位置：Cookie 是将数据存储在客户端，Session 将数据存储在服务端
- 安全性：Cookie不安全，Session安全
- 数据大小：Cookie最大3KB，Session无大小限制
- 存储时间：Cookie可以通过setMaxAge()长期存储，Session默认30分钟
- 服务器性能：Cookie不占服务器资源，Session占用服务器资源



# Filter和Listener

SpringBootApplication上使用**@ServletComponentScan** 注解后

- Servlet可以直接通过@WebServlet注解自动注册
- Filter可以直接通过@WebFilter注解自动注册
- Listener可以直接通过@WebListener 注解自动注册

**Filter** 表示过滤器，是 JavaWeb 三大组件(Servlet、Filter、Listener)之一

- 过滤器可以把对资源的请求拦截下来，从而实现一些特殊的功能
- 过滤器一般完成一些通用的操作
- 实现权限控制

Filter开发步骤：

- 定义类，实现 Filter接口，并重写其所有方法
- 配置Filter拦截资源的路径：在类上定义 @WebFilter 注解。而注解的 value 属性值 /* 表示拦截所有的资源
- SpringBootApplication上使用**@ServletComponentScan** 注解
- 在doFilter方法中输出一句话，并放行

Filter执行流程:

- 执行放行前逻辑（对request数据进行处理）
- 放行
- 访问资源
- 执行放行后逻辑（对response数据进行处理）

**Filter处理逻辑：**

- 获取本次请求URI
- 判断本次请求是否需要处理
- 如果不需要处理，则直接放行
- 判断登录状态，如果已登录，则直接放行
- 如果未登录则返回未登录结果

Filter拦截路径配置：

- 拦截具体的资源：/index.jsp：只有访问index.jsp时才会被拦截

- 目录拦截：/user/*：访问/user下的所有资源，都会被拦截

- 后缀名拦截：*.jsp：访问后缀名为jsp的资源，都会被拦截

- 拦截所有：/*：访问所有资源，都会被拦截

```java
/**
 * 检查用户是否已经登陆
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
                "backend/**",
                "/front/**"
        };

        //2.判断本次请求是否需要处理
        boolean check = check(urls, requestURI);

        //3.如果不需要处理，则直接放行
        if (check){
            //filterChain过滤器链，调用filterChain.doFilter才可以访问下一个匹配的filter
            filterChain.doFilter(request,response);
            return;
        }

        //4.判断登录状态，如果已登录，则直接放行
        if (request.getSession().getAttribute("employee") != null){
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

**AntPathMatcher**

因为equal方法无法识别通配符所以要用匹配器，AntPathMatcher可以做URLs匹配，规则如下

- ？匹配一个字符
- * 匹配0个或多个字符
- **匹配0个或多个目录



**Listener** 表示监听器，是 JavaWeb 三大组件(Servlet、Filter、Listener)之一

- 监听器是在 application ， session ， request 三个对象创建、销毁或者往其中添加修改删除属性时自动执行代码的功能组件
- application 是 ServletContext 类型的对象
- ServletContext 代表整个web应用，在服务器启动的时候，tomcat会自动创建该对象。在服务器关闭时会自动销毁该对象

使用方法：

1. 定义类，实现接口
2. 在类上添加@WebListener注解



# AJAX和JSON

AJAX：异步的 JavaScript 和 XML

- 与服务器进行数据交换：通过AJAX可以给服务器发送请求，服务器将数据直接响应回给浏览器
- 使用AJAX和服务器进行通信，以达到使用 HTML+AJAX来替换JSP页面
- 异步交互：可以在不重新加载整个页面的情况下，与服务器交换数据并更新部分网页的技术，如：搜索联想、用户名是否可用校验

Axios：对原生的AJAX进行封装，简化书写

1. 导入js文件并在html中引入：<script src="js/axios-0.18.0.js"></script> 1

2. 发送 get 请求

   ```javascript
   axios({ 
   method:"get", 
   url:"http://localhost:8080/ajax-demo1/aJAXDemo1?username=zhangsan" 
   }).then(function (resp){ 
   alert(resp.data); }) 
   ```

3. 发送 post 请求

   ```javascript
   axios({ 
   method:"post", 
   url:"http://localhost:8080/ajax-demo1/aJAXDemo1", 
   data:"username=zhangsan" 
   }).then(function (resp){ 
   alert(resp.data); });
   ```

- method 属性：用来设置请求方式的。取值为 get 或者 post
- url 属性：用来书写请求的资源路径。如果是 get 请求，需要将请求参数拼接到路径的后面，格式为： url?参数名=参 数值&参数名2=参数值2
- data 属性：作为请求体被发送的数据。也就是说如果是 post 请求的话，数据需要作为 data 属性的值。
- then() 需要传递一个匿名函数。我们将 then() 中传递的匿名函数称为 回调函数，意思是该匿名函数在发送请求时不会被调用，而是在成功响应后调用的函数。
- 该回调函数中的 resp 参数是对响应的数据进行封装的对象，通过 resp.data 可以获取到响应的数据

```javascript
axios.get("http://localhost:8080/ajax-demo/axiosServlet?username=zhangsan")
.then(function (resp) { 
alert(resp.data); });

axios.post("http://localhost:8080/ajax-demo/axiosServlet","username=zhangsan")
.then(function (resp) { 
alert(resp.data); })
```



**JSON**：JavaScript Object Notation，JavaScript 对象表示法
作用：由于其语法格式简单，层次结构鲜明，现多用于作为数据载体，在网络中进行数据传输。
格式：var 变量名 = {"key":value,"key":value,...};
JSON 串的键要求必须使用双引号括起来，而值根据要表示的类型确定。

value 的数据类型分为如下:

- 数字（整数或浮点数）
- 字符串（使用双引号括起来）
- 逻辑值（true或者false）
- 数组（在方括号中）[]
- 对象（在花括号中）{}
- null

**Fastjson** 可以实现 Java 对象和 JSON 字符串的相互转换。

- 导入坐标

  ```xml
  <dependency> 
  <groupId>com.alibaba</groupId> 
  <artifactId>fastjson</artifactId> 
  <version>1.2.62</version> 
  </dependency>
  ```

- Java对象转JSON：String jsonStr = JSON.toJSONString(user)

- JSON字符串转Java对象：User user = JSON.parseObject(jsonStr, User.class)



# VUE和Element

**Vue** 是一套前端框架，免除原生JavaScript中的DOM操作，简化书写。

- 新建 HTML 页面，引入Vue.js文件：<script src="js/vue.js"></script>

- 在JS代码区域，创建Vue核心对象，进行数据绑定

  ```javascript
  new Vue({ 
  el: "#app", 
  data() { 
      return { 
          username: "" //模型数据
      } } });
  ```

  el：用来指定哪儿些标签受 Vue 管理。 该属性取值 #app 中的 app 需要是受管理的标签的id属性值

  data：用来定义数据模型

  methods：用来定义函数。这个我们在后面就会用到

- 编写视图（报错时考虑将③放在②上边）

  ```xml
  <div id="app"> 
  <input  v-model="username" > 
  {{username}}     //{{}} 是 Vue 中定义的插值表达式 ，在里面写数据模型，到时候会将该模型的数据值展示在这个位置。
  </div>
  ```



Vue 指令：

- v-bind：为HTML标签绑定属性值，如设置 href , css样式等
- v-model：在表单元素上创建双向数据绑定
- v-on：为HTML标签绑定事件
- v-if：条件性的渲染某元素，判定为true时渲染,否则不渲染
- v-else：条件性的渲染某元素，判定为true时渲染,否则不渲染
- v-else-if：条件性的渲染某元素，判定为true时渲染,否则不渲染
- v-show：根据条件展示某元素，区别在于切换的是display属性的值
- v-for：列表渲染，遍历容器的元素或者对象的属性**

Vue生命周期的八个阶段：每触发一个生命周期事件，会自动执行一个生命周期方法，也被称为钩子方法。

- beforeCreate：创建前

- created：创建后

- beforeMount：载入前

- mounted：挂载完成

- beforeUpdate：更新前

- updated：更新后

- beforeDestory：销毁前

- destoryed：销毁后

- mounted：挂载完成，Vue初始化成功，HTML页面渲染成功

  ```javascript
  new Vue({
  el:"#app",
  mouted(){
  alert("vue挂载完成，发送异步请求");
  }});
  ```



**Element**：是饿了么公司前端开发团队提供的一套基于 Vue 的网站组件库，用于快速构建网页。

- 将element-ui文件夹拷贝到webapp或静态资源目录下

- 创建页面，并在页面引入Element 的css、js文件 和 Vue.js

  ```xml
  <script src="js/vue.js"></script> 
  <script src="element-ui/lib/index.js"></script> 
  
  <link rel="stylesheet" href="element-ui/lib/theme-chalk/index.css">
  ```

- 创建Vue核心对象

  ```javascript
  <script> 
  new Vue({ 
  el:"#app" }) </script>
  ```


css样式放在<head>标签中

Layout 局部：通过基础的 24 分栏，迅速简便地创建布局。也就是默认将一行分为 24 栏，根据页面要求给每一列设置所占的栏数。

Container 布局容器：用于布局的容器组件，方便快速搭建页面的基本结构。如下图就是布局容器效果。

