---
title: SpringCloud笔记
date: 2022-08-30
tags: SpringCloud
categories: 笔记
description: SpringCloud笔记
cover: https://zhuxinpeng.oss-cn-hangzhou.aliyuncs.com/3.jpg
---

# SpringCloud

## 认识微服务

**单体架构**：将业务的所有功能集中在一个项目中开发，打成一个包部署。

**优点：**

- 架构简单
- 部署成本低

**缺点：**

- 耦合度高（维护困难、升级困难）



**分布式架构**：根据业务功能对系统做拆分，每个业务功能模块作为独立项目开发，称为一个服务。

**优点：**

- 降低服务耦合
- 有利于服务升级和拓展

**缺点：**

- 服务调用关系错综复杂



**微服务**

微服务的架构特征：

- 单一职责：微服务拆分粒度更小，每一个服务都对应唯一的业务能力，做到单一职责
- 自治：团队独立、技术独立、数据独立，独立部署和交付
- 面向服务：服务提供统一标准的接口，与语言和技术无关
- 隔离性强：服务调用做好隔离、容错、降级，避免出现级联问题

微服务的上述特性其实是在给分布式架构制定一个标准，进一步降低服务之间的耦合度，提供服务的独立性和灵活性。做到高内聚，低耦合，因此，可以认为**微服务**是一种经过良好架构设计的**分布式架构方案**

国内最知名的微服务是**SpringCloud**和阿里的**Dubbo**

**微服务结构**

- 服务集群：拆分的服务组成的集群
- 注册中心：维护微服务每个节点的信息监控节点的状态
- 配置中心：统一管理微服务群的配置
- 服务网关：请求路由

**微服务技术对比**

|                |        Dubbo        |       SpringCloud        |    SpringCloudAlibaba    |
| :------------: | :-----------------: | :----------------------: | :----------------------: |
|    注册中心    |  zookeeper、Redis   |      Eureka、Consul      |      Nacos、Eureka       |
|  服务远程调用  |      Dubbo协议      |     Feign(http协议)      |       Dubbo、Feign       |
|    配置中心    |         无          |    SpringCloudConfig     | SpringCloudConfig、Nacos |
|    服务网关    |         无          | SpringCloudGateway、Zuul | SpringCloudGateway、Zuul |
| 服务监控和保护 | dubbo-admin，功能弱 |         Hystrix          |         Sentinel         |



**SpringCloud**

SpringCloud是目前国内使用最广泛的微服务框架。官网地址：https://spring.io/projects/spring-cloud。

SpringCloud集成了各种微服务功能组件，并基于SpringBoot实现了这些组件的自动装配，从而提供了良好的即用体验。

其中常见的组件包括：

- 服务注册发现：Eureka、Nacos、Consul
- 服务远程调用：OpenFeign、Dubbo
- 服务链路监控：Zipkin、Sleuth
- 统一配置管理：SpringCloudConfig、Nacos
- 统一网关路由：SpringCloudGateway、Zuul
- 流控、降级、保护：Hystix、Sentinel

SpringCloud底层是依赖于SpringBoot的，并且有版本的兼容关系

![image-20210713205003790](D:\学习\SpringCloud\day01-SpringCloud01\讲义\assets\image-20210713205003790.png)



## 服务拆分和远程调用

任何分布式架构都离不开服务的拆分，微服务也是一样。

**服务拆分原则**

- 不同微服务，不要重复开发相同业务
- 微服务数据独立，不要访问其它微服务的数据库
- 微服务可以将自己的业务暴露为接口，供其它微服务调用

**服务拆分示例**

案例需求：修改order-service中的根据id查询订单业务，要求在查询订单的同时，根据订单中包含的userId查询出用户信息，一起返回。

`RestTemplate`是由`Spring`框架提供的一个可用于应用中调用`rest`服务的类，它简化了与`http`服务的通信方式，统一了`RESTFul`的标准，封装了`http`连接，我们只需要传入`url`及其返回值类型即可。

- **注册一个RestTemplate实例到Spring容器**

  ```java
  @MapperScan("cn.itcast.order.mapper")
  @SpringBootApplication
  public class OrderApplication {
  
      public static void main(String[] args) {
          SpringApplication.run(OrderApplication.class, args);
      }
      
      //创建RestTemplate并注入Spring容器
      @Bean
      public RestTemplate restTemplate(){
          return new RestTemplate();
      } 
  }
  ```

- **实现远程调用**

  ```java
  @RestController//@Controller + @ResponseBody
  @RequestMapping("order")
  public class OrderController {
  
     @Autowired
     private OrderService orderService;
     @Autowired
     private RestTemplate restTemplate;
  
     // 根据id查询订单并返回
      @GetMapping("{orderId}")
      public Order queryOrderByUserId(@PathVariable("orderId") Long orderId) {
          Order order = orderService.queryOrderById(orderId);
          String url = "http://localhost:8081/user/" + order.getUserId();
          //restful风格
          User user = restTemplate.getForObject(url,User.class);
          order.setUser(user);
          return order;
      }
  }
  ```

**提供者与消费者**

在服务调用关系中，会有两个不同的角色：

**服务提供者**：一次业务中，被其它微服务调用的服务。（提供接口给其它微服务）

**服务消费者**：一次业务中，调用其它微服务的服务。（调用其它微服务提供的接口）



## Eureka

![image-20210713220104956](D:\学习\SpringCloud\day01-SpringCloud01\讲义\assets\image-20210713220104956.png)

- 消费者该如何获取服务提供者具体信息
  - 服务提供者启动时向eureka注册自己的信息
  - eureka保存这些信息
  - 消费者根据服务名称向eureka拉取提供者信息
- 如果有多个服务提供者，消费者该如何选择
  - 服务消费者利用负载均衡算法，从服务列表中挑选一个
- 消费者如何感知服务提供者健康状态
  - 服务提供者会每隔30s向EurekaServer发送心跳请求，报告健康状态
  - eureka会更新记录服务列表信息，心跳不正常会被剔除
  - 消费者就可以拉取到最新的消息

**搭建eureka-server**

- 创建springboot程序勾选eureka

- 启动类上加上@EnableEurekaServer注解

- 编写配置文件

  ```yaml
  server:
    port: 10086
  spring:
    application:
      name: eurekaserver #eureka的服务名称
  eureka:
    client:
      service-url:   #eureka地址信息
        defaultZone: http://127.0.0.1:10086/eureka
  ```

**注册user-service**

- 在user-service的pom文件中，引入下面的eureka-client依赖：

  ```xml
  <dependency>
      <groupId>org.springframework.cloud</groupId>
      <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
  </dependency>
  ```

- 配置文件

  ```yaml
  spring:
    application:
      name: userservice
  eureka:
    client:
      service-url:
        defaultZone: http://127.0.0.1:10086/eureka
  ```

**服务发现**

将order-service的逻辑修改：向eureka-server拉取user-service的信息，实现服务发现

- 引入依赖：在order-service的pom文件中，引入下面的eureka-client依赖

  ```xml
  <dependency>
      <groupId>org.springframework.cloud</groupId>
      <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
  </dependency>
  ```

- 配置文件

  ```yaml
  spring:
    application:
      name: orderservice
  eureka:
    client:
      service-url:
        defaultZone: http://127.0.0.1:10086/eureka
  ```

- 服务拉取和负载均衡

  在eureka-server中拉取user-service服务的实例列表，并且实现负载均衡。

  - 在order-service的启动类中中，给RestTemplate这个Bean添加一个@LoadBalanced注解

  - 修改访问的url路径，用服务名代替ip、端口

    ```java
        // 根据id查询订单并返回
        @GetMapping("{orderId}")
        public Order queryOrderByUserId(@PathVariable("orderId") Long orderId) {
            Order order = orderService.queryOrderById(orderId);
            String url = "http://userservice/user/" + order.getUserId();
            User user = restTemplate.getForObject(url, User.class);
            order.setUser(user);
            return order;
        }
    ```

    spring会自动帮助我们从eureka-server端，根据userservice这个服务名称，获取实例列表，而后完成负载均衡。

## Ribbon

SpringCloud底层其实是利用了一个名为Ribbon的组件，来实现负载均衡功能的

`LoadBalancerInterceptor`会对RestTemplate的请求进行拦截，然后从Eureka根据服务id获取服务列表，随后利用负载均衡算法得到真实的服务地址信息，替换服务id。