---
title: Maven笔记
date: 2022-02-15
tags: Maven
categories: 笔记
description: Maven高级笔记
cover: https://zhuxinpeng.oss-cn-hangzhou.aliyuncs.com/6.jpg
---
# Maven高级

Maven分模块开发：

1. 在主程序导入分模块的坐标
2. 把分模块用Maven指令进行install

## 依赖

可选依赖：在坐标中添加<optional>true<optional>可以隐藏使用的依赖，将不具有传递性

排除依赖：再导入别的模块坐标时，添加上<exclusions>排除依赖

``` xml
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

``` xml
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

  ``` xml
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

  ``` xml
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

  ``` xml
  <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <scope>test</scope>
  </dependency>
  ```

## 属性

- 父工程中定义属性

  ``` xml
  <properties>
      <spring.version>5.2.10.RELEASE</spring.version>
      <junit.version>4.12</junit.version>
      <mybatis-spring.version>1.3.0</mybatis-spring.version>
  </properties>
  ```

- 修改依赖的version

  ``` xml
  <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-core</artifactId>
      <version>${spring.version}</version>
  </dependency>
  ```

**配置文件加载属性**

- 父工程定义属性

  ``` xml
  <properties>
     <jdbc.url>jdbc:mysql://127.1.1.1:3306/ssm_db</jdbc.url>
  </properties>
  ```

- jdbc.properties文件中引用属性

  ``` properties
  jdbc.driver=com.mysql.jdbc.Driver
  jdbc.url=${jdbc.url}
  jdbc.username=root
  jdbc.password=root
  ```

- 设置maven过滤文件范围

  ``` xml
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

  ``` xml
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

``` xml
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

``` xml
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

**跳过测试**

- 方式一:IDEA工具实现跳过测试，Maven右上角闪电按钮跳过所有测试

- 方式二：在父工程中的pom.xml中添加测试插件配置

  ``` xml
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

``` xml
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

``` xml
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

``` xml
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